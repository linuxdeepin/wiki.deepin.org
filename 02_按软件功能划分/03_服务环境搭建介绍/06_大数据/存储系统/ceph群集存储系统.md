---
title: ceph群集存储系统
description: 
published: true
date: 2022-10-25T02:09:19.124Z
tags: ceph
editor: markdown
dateCreated: 2022-06-28T07:57:05.836Z
---

# ceph群集存储系统

## 前言

ceph是一个群集网络存储的系统，听上去很重量级，实际也是重量级，不过这篇文章目的是用它来做共享文件夹。

## 架构

1. osd ： 存储数据的节点
   1. pg ：归置组
      1. object : 存储单元对象
2. monitor ： 维护网络
3. mds ： ceph file system的元数据

## 演练

创建三个虚拟机节点：

1. `qemu-img create -f qcow2 -o preallocation=full osd.qcow2 10g`
2. `virt-manager` 图形化配置虚拟机，安装debian testing cd。

安装完毕磁盘情况如下：

```bash
lsblk -o name,size,fsused,fsuse%,mountpoint,fstype
NAME                 SIZE FSUSED FSUSE% MOUNTPOINT FSTYPE
sr0                 1024M                          
vda                   10G                          
|-vda1               512M   5.1M     1% /boot/efi  vfat
|-vda2               244M  83.7M    35% /boot      ext2
`-vda3               9.3G                          LVM2_member
  |-osd1--vg-root    3.4G   1.3G    38% /          ext4
  |-osd1--vg-swap_1 1020M               [SWAP]     swap
  `-osd1--vg-home    4.9G  19.5M     0% /home      ext4

```

3. 创建磁盘快照，目的是让各个虚拟机共享当前基础映像，节省空间。

```bash
qemu-img create -f qcow2 -o backing_file=osd.qcow2 osd1.qcow2 #创建虚拟机1的磁盘，共享基础映像，如此类推创建osd2.qcow2和osd3.qcow2
```

4. 克隆虚拟机

```bash
#基础虚拟机osd，克隆三个，修改磁盘配置即可
sudo cp /var/lib/libvirt/qemu/nvram/osd_VARS.fd /var/lib/libvirt/qemu/nvram/osd1_VARS.fd  #因为是uefi启动的，需要保存一份这个玩意（启动资料），不知道为什么克隆虚拟机命令无法自动完成这一步
virt-clone --connect qemu:///system -o osd -n osd1 -f osd1.qcow2 --preserve-data #克隆虚拟机osd为osd1,指定吃盘为osd1.qcow2， --preserve-data参数指定不复制磁盘内容。图形化工具virt-manager 不支持设定这套参数，原因不明，所以只能用命令行

```

5. 设置ssh服务器和帐号

```bash
ssh-copy-id  -i ~/.ssh/htqxgit_rsa htqx@192.168.122.131 #将公钥添加到节点的htqx账户下

nano -w ~/.ssh/config # 修改ssh登录配置，具体操作百度

# 配置好节点服务器的帐号，配置sudo，并且可以免密码。类似 htqx ALL=(ALL) NOPASSWD:ALL
```

6. 设置ntp时间同步

```bash
#登录节点,安装ntp
sudo apt install ntpdate
sudo nano -w /etc/default/ntpdate #编辑配置将ntp1.aliyun.com 添加进去
sudo ntpdate-debian #同步时间
```

7. 添加ceph-deploy 工具

```bash
# 安装包的key
wget -q -O- 'https://download.ceph.com/keys/release.asc' | sudo apt-key add - 
# 添加apt源，luminous是ceph 12.*.*的代号。debian官方没有这个工具，所以最好还是添加ceph官方源，最新版本14 （nautilus）; 13是mimic; 10是jewel 。 stretch 是debian当前的正式版。
echo deb http://download.ceph.com/debian-luminous/ stretch main | sudo tee /etc/apt/sources.list.d/ceph.list  

sudo apt install ceph-deploy #安装ceph-deploy
sudo rm /etc/apt/sources.list.d/ceph.list #删除源，因为会和debian testing自带的产生冲突
```

8. 编辑网络，固定ip：`virsh -c qemu:///system net-edit default`

```xml
<host mac='52:54:00:73:7c:67' ip='192.168.122.101'  />
<host mac='52:54:00:39:da:84' ip='192.168.122.102' /> 
<host mac='52:54:00:0b:1c:92' ip='192.168.122.103' />
```

管理主机编辑/etc/hosts ，将ip 和主机名字绑定，方便将来操作：

```bash
# ceph
192.168.122.1   admin.ceph      admin
192.168.122.101 osd1.ceph       osd1
192.168.122.102 osd2.ceph       osd2
192.168.122.103 osd3.ceph       osd3
```

9. 创建群集。

```bash
ceph-deploy new osd1 #产生ceph-deploy-ceph.log日志、ceph.conf配置、ceph.mon.keyring密钥
```

编辑ceph.conf，添加

```bash
# osd 配置由默认3改为2
osd pool default size = 2
public_network=192.168.122.0/24
```

```bash
ceph-deploy install  {admin,osd1,osd2,osd3}  --no-adjust-repos #安装工具，--no-adjust-repos 参数指定不要修改apt源，否则会自动生成一个连接ceph官方的源，会和debian testing默认的产生冲突

ceph-deploy mon create-initial #初始化monitor，产生：ceph.client.admin.keyring、ceph.bootstrap-osd.keyring、ceph.bootstrap-mds.keyring、ceph.bootstrap-rgw.keyring、ceph.bootstrap-mgr.keyring
```

这里注意，官方中文文档已经过时，很多操作不可实现。这里介绍现在可行的方案：

### 9.1 调整lvm磁盘

```bash
# 不能创建文件夹的存储目录，只能用整个磁盘，因为一开始没有准备，所以调整了一下现在的磁盘。
# 磁盘之前是lvm格式的，osd1--vg-home    4.9G  19.5M     0% /home      ext4 ，这个准备减少2gb，用来创建新空盘store
ssh osd2 #进入节点2
sudo vgdisplay #查看lvm磁盘组，得到pe大小4m（相当于传统扇区）
sudo lvdisplay osd1-vg/home #查看lv逻辑分区
cd / # 离开home目录
sudo umoun /home #卸载磁盘
sudo e2fsck -f /dev/osd1-vg/home #检查磁盘
sudo resize2fs /dev/osd1-vg/home 2928m # 调整ext文件系统到2928m = 4m× 732 个pe
sudo lvreduce -l 732 osd1-vg/home #对应的调整lv逻辑分区到732(pe）,然后就空出来523个pe
sudo lvcreate -l 523 -n store osd1-vg #用空出来的创建store分区
```

回到管理主机上：

```bash
ceph-deploy osd create osd2  --data /dev/osd1-vg/store  #添加存储磁盘，同样的方式添加osd3上的磁盘。
ceph-deploy admin admin osd1 osd2 osd3 #分发管理密钥ceph.client.admin.keyring，注意第一个admin是命令，第二个是我的管理主机的名字
sudo chmod +r /etc/ceph/ceph.client.admin.keyring  #添加权限

ceph health #查看群集系统是否正常
ceph -w #查看群集信息

ceph-deploy mds create osd1 #添加元数据服务器
ceph-deploy rgw create osd1 #添加对象网关服务器，监听7480端口

ceph-deploy mon add osd2 #添加监视器，同样添加osd3
ceph quorum_status --format xml-pretty #检查监视服务器到位情况
```

10. 使用数据
    〇、创建存储池

```bash
ceph osd pool create mypool 8 #创建一个存储池叫mypool

```

一、块设备RBD

```bash
rbd create foo --size 1024 --pool mypool #分配1G的存储块，命名为foo
rbd info foo -p mypool #查看块的信息
sudo  rbd map mypool/foo --name client.admin -k /etc/ceph/ceph.client.admin.keyring -m osd1.ceph  #创建一个设备/dev/rbd0 绑定块，参数指定了用户名和密钥，-m是监视器的地址
mkfs.ext4 -m0 /dev/rbd0 #格式化
mkdir /mnt/cephbd 
mount /dev/rbd0 /mnt/cephbd #挂载
```

二、文件

```bash
ceph osd pool create cephfs_data 8
ceph osd pool create cephfs_metadata 8
ceph fs new cephfs cephfs_metadata cephfs_data #用两个池创建名为cephfs的文件系统
ceph mds stat #查看状态
sudo mkdir /mnt/ceph
#内核驱动挂载
sudo mount -t ceph osd1.ceph:6789:/ /mnt/ceph # 挂载
# 用户空间文件系统 fuse
sudo apt install ceph-fuse
sudo ceph-fuse -m osd1.ceph:6789 /mnt/ceph
```

三、对象

```bash
rados put my-obj-1 1.txt --pool mypool #添加一个对象到存储池，命名为my-obj-1 ，内容是1.txt文件
rados -p mypool ls #列出对象
ceph osd map mypool my-obj-1 #查看在服务器中的位置
```

11. 共享
    ceph 有两个方式共享，但是客户机是windows xp的情况，ceph-dokan 我没有实现连接。

另一个方式是iscsi，这个xp需要安装官方的Microsoft iSCSI Software Initiator Version 2.08  ：<https://www.microsoft.com/en-us/download/details.aspx?id=18986>

服务器安装： `sudo apt install tgt tgt-rbd`

```bash
sudo tgtadm --lld iscsi --op show --mode system #查看支持的文件系统类型

rbd map mypool/foo # 添加ceph块绑定为本地设备
```

编辑配置文档：`sudo nano -w /etc/tgt/targets.conf` 内容如下：

```bash
include /etc/tgt/conf.d/*.conf

<target iqn.2019-5.localhost:iscsi> #名字
 driver iscsi #驱动iscsi
 bs-type rbd #类型ceph rbd块
 backing-store mypool/foo #存储池/块
</target>
```

重启服务：

```bash
systemctl status tgt #查看服务状态
systemctl restart tgt #重启服务
systemctl status tgt #查看是否成功

sudo tgt-admin -show #查看iscsi设备情况
```

## 概念

存储方式：

1. 文件系统cephfs
2. 块设备 rbd
3. 对象网关 rgw
   1. ceph-radosgw 守护进程，
   2. civetweb 前端，7480端口

部署模式：

1. `ceph-deploy install` 安装相关apt包
2. `ceph-deploy admin` 安装管理密钥
3. `ceph-deploy --overwrite-conf config push` 安装新配置文件
   1. 修改配置文件ceph.conf
4. `systemctl restart` 重启守护进程
   1. `iptables` 开放防火墙相关端口

群集：

1. 监视器mon
   1. fsid 标识
      1. `uuidgen` 生成一个id
      2. fsid={uuid}
   2. 群集名称
      1. ceph.conf  其中ceph就是群集名称
   3. 监视器名字
      1. mon initial members = osd1
      2. mon host=192.168.122.101
   4. 监视器图
      1. `monmaptool --create --add osd1 192.168.122.101 --fsid a46ddc52-d181-43f4-95d3-b6b35a264143 monmap`  用监视器名、ip、fsid生成monmap
      2. `ceph-mon --mkfs -i mon1 --monmap monmap --keyring ceph.mon.keyring --mon-data mondata` 生成初始数据，输出到mondata目录。默认/var/lib/ceph/mon/ceph-osd1
   5. 监视器密钥环
      1. `ceph-authtool --create-keyring ceph.mon.keyring --gen-key -n mon. --cap mon 'allow *' `   生成密钥环和密钥
   6. 管理密钥环
      1. `ceph-authtool --create-keyring ceph.client.admin.keyring --gen-key -n client.admin --set-uid=0 --cap mon 'allow *' --cap osd 'allow *' --cap mds 'allow'` 
2. 全局配置
   1. `public network =`
   2. `cluster network = `
   3. `auth cluster required = cephx`
   4. `auth service required = cephx`
   5. `auth client required = cephx`
   6. `osd journal size=`
   7. `filestore xattr use omap = true`
   8. `osd pool default size= 2  ` osd个数
   9. `osd pool default min size=`
   10. `osd pool default pg num=`
   11. `osd pool default pgp num=`
   12. `osd crush chooseleaf type=`
3. 存储服务器osd


## 参考

1. 官方中文文档： <http://docs.ceph.org.cn/start/hardware-recommendations/>
2. 官方英文文档： <http://docs.ceph.com/docs/master/>
3. 使用ceph-deploy安装Ceph 12.x（三） 安装Ceph集群: <https://blog.csdn.net/nirendao/article/details/79359767>
4. 大话 Ceph -- CephX 那点事儿: <http://www.xuxiaopang.com/2017/08/23/easy-ceph-CephX/>
5. ceph(存储之块设备、文件系统、对象存储):<https://blog.csdn.net/fuyinggui/article/details/79425031>