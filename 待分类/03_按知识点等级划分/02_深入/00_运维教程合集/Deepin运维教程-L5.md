---
title: Deepin运维教程-L5
description: 
published: true
date: 2022-10-18T03:29:56.428Z
tags: 
editor: markdown
dateCreated: 2022-06-28T05:56:47.466Z
---

# Deepin运维教程-L5
#### grep文件内容过滤

- grep用于查找文件中符合条件的字符串，它能利用正则表达式搜索文件中的字符串，并把匹配到的字符串的行打印出来
- 命令格式：grep [-选项] "查找条件" 目标文件
- 常用选项：
  - -n #以行号形式输出
  - -i #忽略字符串大小写
  - -v #显示不包含匹配的行（排除）
- 常用正则表达式符号
  - ^字符串 #显示以该字符串开头的行
  - $字符串 #显示以该字符串结尾的行
  - ^$ #显示空行
- grep命令示例

```shell
#过滤包含root关键字的行
[root@localhost ~]# grep root /etc/passwd
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin

#以行号形式过滤包含root关键字的行
[root@localhost ~]# grep -n root /etc/passwd
1:root:x:0:0:root:/root:/bin/bash
10:operator:x:11:0:operator:/root:/sbin/nologin

[root@localhost ~]# grep -n bash /etc/passwd
[root@localhost ~]# grep -n : /etc/passwd

#忽略大小写过滤
[root@localhost ~]# grep -i -n ssh /etc/passwd
38:sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin

#排除包含#号的行
[root@localhost ~]# grep -n -v '^#' /etc/fstab

#过滤以root开头的行
[root@localhost ~]# grep ^root /etc/passwd

#过滤以root结尾的行
[root@localhost ~]# grep -n 'root$' /etc/passwd
[root@localhost ~]# grep -n 'bash$' /etc/passwd

#语法错误示范
[root@localhost ~]# grep -n -v '^#' ^$  /etc/fstab
grep: ^$: 没有那个文件或目录
/etc/fstab:1:
/etc/fstab:9:/dev/mapper/centos-root /                       xfs     defaults        0 0
/etc/fstab:10:UUID=ae55ec6b-973b-498e-a366-f35e14b3d153 /boot                   xfs     defaults        0 0
/etc/fstab:11:/dev/mapper/centos-swap swap    

#语法错误示范
[root@localhost ~]# grep -n -v '^#' /etc/fstab | grep -v ^$
1:
9:/dev/mapper/centos-root /                       xfs     defaults        0 0
10:UUID=ae55ec6b-973b-498e-a366-f35e14b3d153 /boot                   xfs     defaults        0 0
11:/dev/mapper/centos-swap swap                    swap    defaults        0 0

#正确语法
[root@localhost ~]# grep  -v '^#' /etc/fstab | grep -v ^$ -n
2:/dev/mapper/centos-root /                       xfs     defaults        0 0
3:UUID=ae55ec6b-973b-498e-a366-f35e14b3d153 /boot                   xfs     defaults        0 0
4:/dev/mapper/centos-swap swap                    swap    defaults        0 0

#显示该文件内有效配置的行
[root@localhost ~]# grep -v '^#' /etc/login.defs | grep -v ^$ -n | wc -l
```

#### find文件/目录查找命令

- find 命令根据预设的条件递归查找文件或目录所在位置
- 命令格式：find 查找路径 查找条件1 查找条件2 .. [-exec 处理命令 {} \; ]
  - –exec 可接额外的命令来处理查找到结果
  - {} 代表find查找到的内容被放置{}中
  - \; 代表额外处理命令结束
- 常用查找条件
  - -type 类型（f文件 d目录 l链接文件）
  - -name “文件名”
  - -iname 按文件名查找忽略大小写
  - -size 文件大小（k、M、G + 大于 - 小于）
  - -a （并且）两个条件同时满足
  - -o （或者）两个条件满足任意一个即可
  - -user 用户名
  - -mtime 按日期查找（+ 代表多少天之前 - 代表多少天之内，0代表24小时之内）
- find命令范例

```shell
[root@localhost ~]# ls /var/log

#按照类型查找，类型为文件
[root@localhost ~]# find /var/log -type f
[root@localhost ~]# ll boot.log-20210417
[root@localhost ~]# ll /var/log/boot.log-20210417
[root@localhost ~]# ll /var/log/vmware-network.2.log

#按照类型查找，类型为目录
[root@localhost ~]# find /var/log -type d
[root@localhost ~]# ll -d /var/log/tuned
[root@localhost ~]# ll -d /var/log/qemu-ga

#按照类型查找，类型为链接文件
[root@localhost ~]# find /var/log -type l
[root@localhost ~]# fin /etc/ -type l
[root@localhost ~]# find /etc/ -type l
[root@localhost ~]# ll /etc/scl/conf

#按照名字查找
[root@localhost ~]# find /etc/ -name passwd
/etc/passwd
/etc/pam.d/passwd

#按照名字查找，类型为文件
[root@localhost ~]# find /etc/ -name passwd -type f

#按照名字查找，以tab结尾，类型为文件
[root@localhost ~]# find /etc/ -name '*tab' -type f

#按照名字查找，以pass开头，类型为文件
[root@localhost ~]# find /etc/ -name 'pass*' -type f
[root@localhost etc]# find . -name '*.conf' -type f

[root@localhost ~]# find /etc/ -name '*tab*' -type f

#按照名字忽略大小写查找，类型为文件
[root@localhost ~]# find /etc/ -iname FSTAB -type f
/etc/fstab
[root@localhost ~]# find /etc/ -name FSTAB -type f

#查找大于10k的文件
[root@localhost ~]# find /var/log -size +10k -type f
[root@localhost ~]# du -h /var/log/boot.log-20210417
16K	/var/log/boot.log-20210417

#查找大于1M的文件
[root@localhost ~]# find /var/log -size +1M -type f
[root@localhost ~]# du -h /var/log/audit/audit.log
2.4M	/var/log/audit/audit.log

[root@localhost ~]# find /home -size +1M -type f

#查找小于1M的文件
[root@localhost ~]# find /var/log -size -1M -type f
[root@localhost ~]# du -h /var/log/spooler
0	/var/log/spooler

#查找大于10k并且下于20k，类型为文件
[root@localhost ~]# find /var/log -size +10k -a -size -20k -type f

#查找大于10k或者小于100k，类型为文件
[root@localhost ~]# find /var/log -size +10k -o -size -100k -type f

#查找属于lisi用户的文件/目录
[root@localhost ~]# find /home -user lisi

#查找30天之前被修改过，类型为文件
[root@localhost ~]# find /var/log -mtime +30 -type f
[root@localhost ~]# find /var/log -mtime +10 -type f

#查找10天之内被修改过，类型为文件
[root@localhost ~]# find /var/log -mtime -10 -type f
root@localhost ~]# find /var/log -mtime -30 -type f

#查找30之前被修改过，类型为文件，拷贝到/opt目录下
[root@localhost ~]# find /var/log -mtime -30 -type f -exec cp {} /opt \;
```

题型：

- 查找/etc/目录下以.conf结尾的文件（只能在/etc这一层目录去查找）

  [root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# ls /etc/*.conf

- 查找/etc/目录下以.conf结尾的文件（包含所有的子目录）

  [root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# find /etc/ -name '*.conf' -type f

#### 压缩与解压缩

- Linux独有压缩格式及命令工具:
  - gzip---> .gz
  - bzip2---> .bz2
  - xz---> .xz
- 压缩命令格式
  - gzip [选项...] 文件名
    - 常用选项：-d 解压缩
  - bzip2 [选项...] 文件名
    - 常用选项：-d 解压缩
  - xz [选项...] 文件名
    - 常用选项：-d 解压缩
- 查看压缩文件内容
  - zcat [选项...] 文件名
  - bzcat [选项...] 文件名
  - xzcat [选项...] 文件名

```shell
[root@localhost ~]# cp /etc/services /opt
[root@localhost ~]# cd /opt
[root@localhost opt]# ll services 
-rw-r--r--. 1 root root 670293 4月  17 17:06 services
[root@localhost opt]# ll -h services 
-rw-r--r--. 1 root root 655K 4月  17 17:06 services

#使用gzip格式对文件进行压缩
[root@localhost opt]# gzip services 
[root@localhost opt]# ls
services.gz
[root@localhost opt]# ll -h services.gz 
-rw-r--r--. 1 root root 133K 4月  17 17:06 services.gz

#不解压查看压缩文件内容
[root@localhost opt]# zcat services.gz 

#解压文件
[root@localhost opt]# gzip -d services.gz 

#使用bzip2格式对文件进行压缩
[root@localhost opt]# bzip2 services 
[root@localhost opt]# ls
services.bz2
[root@localhost opt]# ll -h services.bz2 
-rw-r--r--. 1 root root 122K 4月  17 17:06 services.bz2

#不解压查看文件内容
[root@localhost opt]# bzcat services.bz2 

#解压文件
[root@localhost opt]# bzip2 -d services.bz2 

#使用xz格式对文件进行压缩
[root@localhost opt]# xz services 
[root@localhost opt]# ls
services.xz
[root@localhost opt]# ll -h services.xz 
-rw-r--r--. 1 root root 98K 4月  17 17:06 services.xz

#解压文件
[root@localhost opt]# xz -d services.xz 
```

#### tar打包工具

- tar命令用在linux下用于对文件/目录打包，使用 tar 程序打出来的包常称为 tar 包，tar 包文件通常都是以 .tar 结尾
- tar 命令格式：tar 选项 压缩包名字 被压缩文件
- 常用选项：
  - -c 创建打包文件
  - -f 指定打包后的文件名称
  - -z 调用gzip压缩工具 -J 调用xz压缩工具 -j 调用bzip2压缩工具
  - -t 列出打包文档内容
  - -x 释放打包文件
  - -C 指定解压路径
  - -v 显示详细信息
- tar命令范例

```shell
#同时打包多个文件/目录并使用gzip格式压缩
[root@localhost opt]# tar -czf xxx.tar.gz /etc/passwd /etc/fstab /home

#将压缩包数据解压到/media目录
[root@localhost opt]# tar -xf xxx.tar.gz -C /media/
[root@localhost opt]# ls /media/etc
[root@localhost opt]# rm -rf xxx.tar.gz 

#同时打包多个文件/目录并使用xz格式压缩
[root@localhost opt]# tar -cJf xx.tar.xz /etc/hostname /etc/services /home

#错误语法，f选项要放到所有选项右边
[root@localhost opt]# tar -ft xx.tar.xz 
tar: 您必须从"-Acdtrux"或是"--test-label"选项中指定一个
请用“tar --help”或“tar --usage”获得更多信息。

#不解压查看压缩包数据
[root@localhost opt]# tar -tf xx.tar.xz 
etc/hostname

#将压缩包数据解压到/tmp目录
[root@localhost opt]# tar -vxf xx.tar.xz -C /tmp
[root@localhost opt]# ls /tmp

#同时打包多个文件/目录并使用bzip2格式压缩
[root@localhost opt]# tar -cjf abc.tar.bz2 /etc/hostname /etc/group /home

#解压缩
[root@localhost opt]# tar -xf abc.tar.bz2 -C /media/
```

#### 磁盘介绍

![1619020721408](https://hu60.cn/q.php/link.img.html?url64=QzpcVXNlcnNcemhpeV9cQXBwRGF0YVxSb2FtaW5nXFR5cG9yYVx0eXBvcmEtdXNlci1pbWFnZXNcMTYxOTAyMDcyMTQwOC5wbmc.)

#### 分区过程

添加新硬盘--分区--格式化文件系统--挂载使用

扇区是磁盘存储数据的最小单元，默认一个扇区可以存储512字节的数据

#### 磁盘类型介绍

- IDE接口类型：主要用于个人家用计算机领域，优点价格便宜，缺点数据传输速度慢
- SCSI接口类型：主要用于服务器理领域，数据传输速度快，支持热插拔
- SATA接口类型：串口磁盘，主要用于个人家用计算机领域
- NVMe接口类型：固态硬盘接口

#### Linux常用分区格式

- MBR分区格式：比较古老的分区格式，分为主分区，只能划分4个主分区，扩展分区（容器）逻辑分区，最大支持2.2T磁盘容量
  - IDE接口硬盘逻辑分区最多可以划分59个
  - SCSI接口硬盘逻辑分区最多可以划分11个
  - 最大支持2.2T以内磁盘容量
- GPT分区格式：可划分128个主分区，最大支持18EB磁盘容量（1EB=1024PB，1PB=1024TB，1TB=1024GB）

#### 文件系统类型详解

- 文件管理系统，赋予分区文件系统分区才可以正常的使用，根文件系统，多少个多少个文件系统
- CentOS5：分区默认使用文件系统类型ext3
- CentOS6：分区默认使用文件系统类型ext4
  - ext4日志记录功能，意外宕机，通过日志记录把没有保存的数据，在系统再次重启时快速恢复回来
  - 单个文件系统最大支持1EB的分区容量，单个文件最大可以存储16TB数据
- CentOS7：分区默认使用文件系统类型xfs
  - xfs开启了日志记录的功能，数据恢复的时候比ext4文件系统块
  - 单个文件系统最大支持8EB分区容量，单个文件最大可以存储500TB的数据
  - 单个文件每秒读写数据的速度可以达到4G
- swap文件系统：交换分区，硬盘空间去充当内存去使用

#### 挂载

- 在Linux系统中用户无法直接使用硬件设备的，硬件设备在系统中都是以只读的方式存在的，必须挂载
- 挂载就是给我们用户提供一个可以使用设备的一个接口
- 挂载注意事项：
  - 挂载点必须是一个目录，理论上还得是一个空目录
  - 一个文件系统不允许重复挂载到多个目录下
  - 一个目录不允许重复挂载多个文件系统

#### lsblk查看系统所有磁盘信息

- lsblk（英文全拼：list block）用于列出当前系统所有磁盘与磁盘内的分区信息
- 命令格式：lsblk [选项...] [设备名]
- 常用选项：
  - -d #仅显示磁盘本身，不会列出磁盘的分区数据
  - -f #列出磁盘分区使用的文件系统类型
- lsblk命令示例

```shell
#列出当前系统所有磁盘与磁盘内的分区信息
[root@localhost ~]# lsblk 
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0   20G  0 disk 
├─sda1            8:1    0    1G  0 part /boot
└─sda2            8:2    0   19G  0 part 
  ├─centos-root 253:0    0   17G  0 lvm  /
  └─centos-swap 253:1    0    2G  0 lvm  [SWAP]
sr0              11:0    1  4.3G  0 rom  /mnt/centos

#sda1：sd代表SCSI磁盘，a代表第一块磁盘，1代表第一个分区
#sdb：sd代表SCSI磁盘，b代表第二块磁盘，1代表第一个分区
#解释：
NAME		#设备名称
MAJ:MIN		#主设备号:次设备号，内核通过主次设备号识别磁盘
RM			#是否为可卸载设备，1可卸载，0不可卸载
SIZE		#设备的容量大小
RO			#表示设备是否为只读，0非只读设备，1只读设备
TYPE		#表示设备类型（disk为磁盘，part为分区，lvm逻辑卷，rom只读）
MOUNTPOINT	#设备挂载点（SWAP没有挂载点）

#列出指定的磁盘信息
[root@localhost ~]# lsblk -d /dev/sda
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
sda    8:0    0  20G  0 disk 

#列出所有磁盘分区内使用的文件系统类型
[root@localhost ~]# lsblk -f 
NAME            FSTYPE      LABEL           UUID                                   MOUNTPOINT
sda                                                                                
├─sda1          xfs                         4cb9bb38-c34a-4415-9614-ba38642bb86d   /boot
└─sda2          LVM2_member                 cKn0jP-z8Bq-SNvl-BsNa-7vTg-GBU2-OiHCro 
  ├─centos-root xfs                         55dad88d-a600-42d1-b387-236db62ce396   /
  └─centos-swap swap                        2e91599a-6d72-483d-add8-6dfb84296170   [SWAP]
sr0             iso9660     CentOS 7 x86_64 2018-11-25-23-54-16-00                 /mnt/centos

#列出指定分区的文件系统类型
[root@localhost ~]# lsblk -df /dev/sda1
NAME FSTYPE LABEL UUID                                 MOUNTPOINT
sda1 xfs          4cb9bb38-c34a-4415-9614-ba38642bb86d /boot
```

#### df查看分区使用情况

- df命令用于查看文件系统使用情况
- 命令格式：df [选项...] [参数...]
- 常用选项：
  - -h 以人类易读方式显示文件系统容量
  - T 显示文件系统类型
- df 命令示例

```shell
[root@localhost ~]# df
Filesystem              1K-blocks    Used Available Use% Mounted on
/dev/mapper/centos-root  17811456 3746320  14065136  22% /
devtmpfs                   480884       0    480884   0% /dev
tmpfs                      497948       0    497948   0% /dev/shm
tmpfs                      497948    8340    489608   2% /run
tmpfs                      497948       0    497948   0% /sys/fs/cgroup
/dev/sr0                  4480476 4480476         0 100% /mnt
/dev/sda1                 1038336  169448    868888  17% /boot
tmpfs                       99592      12     99580   1% /run/user/42
tmpfs                       99592       0     99592   0% /run/user/0

[root@localhost ~]# df -h /
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   17G  3.6G   14G  22% /
```

#### du统计文件/目录大小

- du命令用于统计磁盘下目录或文件大小
- 命令格式：du [选项...] [参数...]
- 常用选项：
  - -h #以人类易读方式（Kb，MB，GB）显示文件大小
  - -s #只统计每个参数的总数
- du 命令示例

```shell

```

- /dev/目录下文件详解

```shell
[root@localhost ~]# ls /dev
hd[a-t]:IDE设备
sd[a-z]:SCSI设备
fd[0-7]：软盘驱动设备
md[0-32]：软RAID设备
loop[0-7]：本地回环设设备
lp[0-3]:打印机设备
mem：内存设备
null：空设备，也称为黑洞，任何写入的数据都将被丢弃
zero：零资源设备，任何写入的数据都将被丢弃
full：满设备，任何写入的数据都将失败
tty[0-63]：虚拟终端设备
random：随机数设备
urandom：随机数设备
port：存取I/O端口
```

#### blkid查看设备属性

- blkid命令显示块设备属性信息（设备名称，设备UUID，文件系统类型）
- 命令格式：blkid [选项...] [参数...]
- blkid命令示例

```shell
#显示系统所有块设备属性信息
[root@localhost ~]# blkid
/dev/sda1: UUID="4cb9bb38-c34a-4415-9614-ba38642bb86d" TYPE="xfs" 
/dev/sda2: UUID="cKn0jP-z8Bq-SNvl-BsNa-7vTg-GBU2-OiHCro" TYPE="LVM2_member" 
/dev/sr0: UUID="2018-11-25-23-54-16-00" LABEL="CentOS 7 x86_64" TYPE="iso9660" PTTYPE="dos" 
/dev/mapper/centos-root: UUID="55dad88d-a600-42d1-b387-236db62ce396" TYPE="xfs" 
/dev/mapper/centos-swap: UUID="2e91599a-6d72-483d-add8-6dfb84296170" TYPE="swap" 

#查看执行分区属性信息
root@localhost ~]# blkid /dev/sda1
/dev/sda1: UUID="4cb9bb38-c34a-4415-9614-ba38642bb86d" TYPE="xfs" 
```

#### MBR分区格式

- fdisk命令用于查看磁盘使用情况和磁盘分区（MBR分区格式）
- 命令格式：fdisk [选项...] [设备路径]
- 常用选项：-l 列出磁盘分区表类型与分区信息
- 分区

```shell
[root@localhost ~]# fdisk /dev/sdb
m	#获取命令帮助		※
p	#显示磁盘分区表   ※
n	#新增加一个分区   ※
q	#不保存分区退出   ※
d	#删除一个分区     ※
w	#保存分区退出     ※
a	#设置可引导标记
b	#编辑bsd磁盘标签
c	#设置DOS操作系统兼容标记
l	#显示已知的文件系统类型，82为swap交换分区，83为Linux分区
o	#建立空白DOS分区表
s	#新建空白SUN磁盘标签
t	#改变分区的系统ID
u	#改变显示记录单位
v	#验证分区表
x	#附加功能

命令(输入 m 获取帮助)：m
命令(输入 m 获取帮助)：p

#划分第一个主分区
命令(输入 m 获取帮助)：n
Select (default p):   回车
分区号 (1-4，默认 1)：回车
起始 扇区 (2048-209715199，默认为 2048)：回车
Last 扇区, +扇区 or +size{K,M,G} (2048-209715199，默认为 209715199)：+10G  #指定大小（K,M,G）
分区 1 已设置为 Linux 类型，大小设为 10 GiB

命令(输入 m 获取帮助)：p
磁盘标签类型：dos
磁盘标识符：0xefc65503

   设备 Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    20973567    10485760   83  Linux

#划分第二个主分区
命令(输入 m 获取帮助)：n
Select (default p): 
分区号 (2-4，默认 2)：
起始 扇区 (20973568-209715199，默认为 20973568)：
Last 扇区, +扇区 or +size{K,M,G} (20973568-209715199，默认为 209715199)：+10G  #指定分区大小

#划分第三个主分区
命令(输入 m 获取帮助)：n
Select (default p): 
分区号 (3,4，默认 3)：
起始 扇区 (41945088-209715199，默认为 41945088)：
Last 扇区, +扇区 or +size{K,M,G} (41945088-209715199，默认为 209715199)：+10G

#查看分区信息
命令(输入 m 获取帮助)：p
磁盘标签类型：dos
磁盘标识符：0xefc65503

   设备 Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    20973567    10485760   83  Linux
/dev/sdb2        20973568    41945087    10485760   83  Linux
/dev/sdb3        41945088    62916607    10485760   83  Linux

#划分第四个分区
命令(输入 m 获取帮助)：n
Select (default e): p
起始 扇区 (62916608-209715199，默认为 62916608)：
Last 扇区, +扇区 or +size{K,M,G} (62916608-209715199，默认为 209715199)：+10G

#继续划分分区
命令(输入 m 获取帮助)：n
If you want to create more than four partitions, you must replace a
primary partition with an extended partition first.
#提示如果想要创建更多的分区，先将一个主分区替换为扩展分区

#删除分区
命令(输入 m 获取帮助)：d4
分区号 (1-4，默认 4)：
分区 4 已删除

命令(输入 m 获取帮助)：d
分区号 (1-3，默认 3)：3
分区 3 已删除

命令(输入 m 获取帮助)：p
磁盘标签类型：dos
磁盘标识符：0xefc65503

   设备 Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    20973567    10485760   83  Linux
/dev/sdb2        20973568    41945087    10485760   83  Linux

#创建主分区
命令(输入 m 获取帮助)：n
Select (default p): 
分区号 (3,4，默认 3)：
起始 扇区 (41945088-209715199，默认为 41945088)：
Last 扇区, +扇区 or +size{K,M,G} (41945088-209715199，默认为 209715199)：+10G

#创建按扩展分区
命令(输入 m 获取帮助)：n
Select (default e): 
Using default response e
已选择分区 4
起始 扇区 (62916608-209715199，默认为 62916608)：
Last 扇区, +扇区 or +size{K,M,G} (62916608-209715199，默认为 209715199)：
分区 4 已设置为 Extended 类型，大小设为 70 GiB

#创建逻辑分区
命令(输入 m 获取帮助)：n
添加逻辑分区 5
起始 扇区 (62918656-209715199，默认为 62918656)：
Last 扇区, +扇区 or +size{K,M,G} (62918656-209715199，默认为 209715199)：+10G
分区 5 已设置为 Linux 类型，大小设为 10 GiB

命令(输入 m 获取帮助)：p
磁盘 /dev/sdb：107.4 GB, 107374182400 字节，209715200 个扇区
磁盘标签类型：dos
磁盘标识符：0xefc65503

   设备 Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    20973567    10485760   83  Linux
/dev/sdb2        20973568    41945087    10485760   83  Linux
/dev/sdb3        41945088    62916607    10485760   83  Linux
/dev/sdb4        62916608   209715199    73399296    5  Extended
/dev/sdb5        62918656    83890175    10485760   83  Linux
命令(输入 m 获取帮助)：w
```

#### 格式化文件系统

- mkfs命令用于在分区上建立文件系统
- 常用文件系统类型
  - ext4，xfs
- 命令格式：
  - mkfs.xfs 分区设备路径 #格式化为xfs类型文件系统
  - mkfs.ext4 分区设备路径 #格式化为ext4类型文件系统

```shell
#格式化文件系统
[root@localhost ~]# mkfs.xfs /dev/sdb1

#查看文件系统类型
[root@localhost ~]# blkid /dev/sdb1
/dev/sdb1: UUID="3bb79b0b-3f17-4ad9-ad47-f00dcb6a5afa" TYPE="xfs" 
```

#### mount挂载

- mount文件系统挂载命令
- 命令格式：mount 设备路径 挂载点目录

```shell
#创建挂载点目录
[root@localhost ~]# mkdir /mybak

#挂载文件系统
[root@localhost ~]# mount /dev/sdb1 /mybak

#查看正在使用中的分区信息
[root@localhost ~]# df -Th 

[root@localhost ~]# df -Th /mybak
文件系统       类型  容量  已用  可用 已用% 挂载点
/dev/sdb1      xfs    10G   33M   10G    1% /mybak
```

总结：

- 添加硬盘---查看系统是否识别新硬盘 lsblk
- 划分分区---fdisk 设备路径
- 格式化文件系统---mkfs.xfs
- 挂载---创建挂载点目录--挂载 mount 设备路径 挂载点目录
- 查看分区使用情况 df -hT

#### umount卸载

- umount命令用于卸载文件系统
- 命令格式：umount 挂载点目录

```shell
#卸载文件系统
[root@localhost ~]# umount /mybak
[root@localhost ~]# df -h 
```

#### 开机自动挂载

- /etc/fstab用于存放文件系统信息，当系统启动时，系统会自动读取文件内容将指定的文件系统挂载到指定的目录
- 文件内容详解

```shell
[root@localhost ~]# vim /etc/fstab
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=5d36a8b5-5a58-450f-acf9-81fcddaa62de /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0
#解释：该文件内容为6个字段，每个字段详解如下
第一个字段：要挂载的设备路径
第二个字段：挂载点目录
第三个字段：设备文件系统类型
第四个字段：挂载参数，参数如下↓
sync，async：  此文件系统是否使用同步写入 (sync) 或异步 (async) 的内存机制，默认为异步（async） 
atime，noatime：更新访问时间/不更新访问时间，访问分区时，是否更新文件的访问时间，默认为更新
ro，rw：挂载文件为只读（ro）或读写（rw），默认为rw
auto，noauto：自动挂载/手动挂载，执行mount -a时，是否自动挂载/etc/fstab文件内容，默认为自动（auto）
dev，nodev：是否允许此文件系统上，可建立装置文件，默认为允许（dev）
suid，nosuid：是否允许文件系统上含有SUID与SGID特殊权限，默认为允许（SUID）
exec，noexec：是否允许文件系统上拥有可执行文件，默认为允许（exec）
user，nouser：是否允许普通用户执行挂载操作，默认为不允许（nouser），只有root用户可以挂载分区
defaults默认值：代表async，rw，auto，dev，suid，exec，nouser七个选项
第五个字段：是否对文件系统进行备份，0不备份，1为备份
第六个字段：是否检查文件系统顺序，允许的数字是0，1，2，0表示不检查，1的优先权最高

/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=ae55ec6b-973b-498e-a366-f35e14b3d153 /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0
/dev/sdb1 /mybak xfs defaults 0 0     #手动添加                                                    
```

- mount常用选项：
  - -a：依照配置文件/etc/fstab的数据将所有未挂载的磁盘都挂载上来
  - -o：该选项后边可跟挂载时额外参数
- remount命令：重新挂载文件系统，在文件系统出错时或重新挂载文件系统时非常重要，

```shell
[root@localhost ~]# mount -a
```

#### GPT分区格式

- gdisk命令用于查看磁盘使用情况和磁盘分区（GPT分区格式）
- 命令格式：gdisk [选项...] [设备路径]
- 常用选项：-l 列出磁盘分区表类型与分区信息

```shell
[root@localhost ~]# gdisk /dev/sdc
GPT fdisk (gdisk) version 0.8.10 #GPT版本

Partition table scan:   #分区表扫描
  MBR: not present		#MBR分区不存在
  BSD: not present		#BSD分区不存在
  APM: not present		#APM分区不存在
  GPT: not present		#GPT分区不存在

Creating new GPT entries.  #创建新的GPT分区

Command (? for help): ?   #输入？号获取命令帮助
p	#显示磁盘分区表   ※
n	#新增加一个分区   ※
q	#不保存分区退出   ※
d	#删除一个分区     ※
w	#保存分区退出     ※

#创建新的分区
Command (? for help): n
Partition number (1-128, default 1): 回车
First sector (34-209715166, default = 2048) or {+-}size{KMGTP}: 回车  #输入起始扇区，默认2048开始
Last sector (2048-209715166, default = 209715166) or {+-}size{KMGTP}: +20G #输入新增分区的大小，可以通过扇区数来增加，也可以通过+size{KMGTP}方式来增加
Hex code or GUID (L to show codes, Enter = 8300):  #这里要求输入分区的类型，直接回车就行

#查看分区类型
Command (? for help): p  #输入p查看创建的分区
Disk /dev/sdc: 209715200 sectors, 100.0 GiB  #磁盘总容量
...
Total free space is 167772093 sectors (80.0 GiB)  #磁盘剩余容量

Number  Start (sector)    End (sector)  Size       Code  Name
   1            2048        41945087   20.0 GiB    8300  Linux filesystem
#以创建的分区

Command (? for help): w   #输入w保存配置，如果不想保存可以输入q退出
Do you want to proceed? (Y/N): y  #问你是否相想继续，输入y继续 
OK; writing new GUID partition table (GPT) to /dev/sdc.
The operation has completed successfully.   #写入成功

#格式化文件系统
[root@localhost ~]# mkfs.xfs /dev/sdc1

#查看文件系统类型
[root@localhost ~]# blkid /dev/sdc1
/dev/sdc1: UUID="c57746eb-8170-4c86-82ad-6aae95de19f3" TYPE="xfs" 

#创建挂载点
[root@localhost ~]# mkdir /webbak
[root@localhost ~]# mount /dev/sdc1 /webbak
[root@localhost ~]# df -hT
/dev/sdc1               xfs        20G   33M   20G    1% /webbak

#开机自动挂载
[root@localhost ~]# vim /etc/fstab
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=ae55ec6b-973b-498e-a366-f35e14b3d153 /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0
/dev/sdb1              /mybak                   xfs     defaults        0 0 
/dev/sdc1              /webbak                  xfs     defaults        0 0   #手动添加

[root@localhost ~]# mount -a
```

#### LVM逻辑卷

- 逻辑卷：LVM（Logical Volume Manager）逻辑卷管理系统
- 逻辑卷可以实现将底层的物理分区整合成一个大的虚拟硬盘
- 逻辑卷技术是通过Linux系统内核dm（device mapper）设备映射组件

![1618737987640](https://hu60.cn/q.php/link.img.html?url64=QzpcVXNlcnNcemhpeV9cQXBwRGF0YVxSb2FtaW5nXFR5cG9yYVx0eXBvcmEtdXNlci1pbWFnZXNcMTYxODczNzk4NzY0MC5wbmc.)

#### 创建卷组

- 创建卷组思路：将创建好的物理卷组成卷组（或者直接创建卷组）
- 命令格式：vgcreate 卷组名 设备路径1 设备路径2...

```shell
#创建卷组
[root@localhost ~]# vgcreate systemvg  /dev/sdb2 /dev/sdb3

#详细显示卷组信息
[root@localhost ~]# vgdisplay  systemvg
  --- Volume group ---
  VG Name               systemvg  #卷组名字
  System ID             
  Format                lvm2      #卷组格式
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               19.99 GiB  #卷组大小
  PE Size               4.00 MiB
  Total PE              5118
  Alloc PE / Size       0 / 0   
  Free  PE / Size       5118 / 19.99 GiB
  VG UUID               KEP7XS-wrkI-rTUY-RqBa-UJA6-YRkK-iKDabR  #卷组UUID

#简要显示卷组信息
[root@localhost ~]# vgs systemvg
  VG       #PV #LV #SN Attr   VSize  VFree 
  systemvg   2   0   0 wz--n- 19.99g 19.99g
```

#### 创建逻辑卷

- 创建逻辑卷思路：从创建好的卷组中创建逻辑卷
- 命令格式：lvcreate -L 大小 -n 逻辑卷名 卷组名

```shell
#创建逻辑卷
[root@localhost ~]# lvcreate -L 10G -n mylv systemvg
  Logical volume "mylv" created.

#简要查看逻辑卷信息
[root@localhost ~]# lvs
  LV   VG       Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root centos   -wi-ao---- <17.00g                                                    
  swap centos   -wi-ao----   2.00g                                                    
  mylv systemvg -wi-a-----  10.00g     
[root@localhost ~]# lvs /dev/systemvg/mylv 
  LV   VG       Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  mylv systemvg -wi-a----- 10.00g    
  
 #查看卷组信息，卷组信息以变小
 [root@localhost ~]# vgs
  VG       #PV #LV #SN Attr   VSize   VFree
  centos     1   2   0 wz--n- <19.00g    0 
  systemvg   2   1   0 wz--n-  19.99g 9.99g
```

#### 格式化文件系统

```shell
#格式化文件系统
[root@localhost ~]# mkfs.xfs /dev/systemvg/mylv

#查看文件系统类型
[root@localhost ~]# blkid /dev/systemvg/mylv
/dev/systemvg/mylv: UUID="7f08daf8-ae3c-40b2-a282-4514a6f37111" TYPE="xfs" 

#挂载使用
[root@localhost ~]# mkdir /dbbak
[root@localhost ~]# mount /dev/systemvg/mylv /dbbak
[root@localhost ~]# df -hT
/dev/mapper/systemvg-mylv xfs        10G   33M   10G    1% /dbbak
```

#### 扩展逻辑卷

- 逻辑卷支线上扩容，逻辑卷的空间来源于卷组，当卷组有足够的空间是，才可以扩展逻辑卷
- 扩展命令：lvextend

```shell
#扩容逻辑卷
[root@localhost ~]# lvextend -L +9G /dev/systemvg/mylv 

#查看逻辑卷信息
[root@localhost ~]# lvs 
  LV   VG       Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root centos   -wi-ao---- <17.00g                                                    
  swap centos   -wi-ao----   2.00g                                                    
  mylv systemvg -wi-ao----  19.00g    #扩容成功
```

#### 扩展文件系统

- 当逻辑卷扩大以后，也需要多逻辑卷的文件系统进行扩展
- 刷新文件系统容量：
  - xfs_growfs #用于扩容XFS设备
  - resize2fs #用于扩容EXT3/EXT4设备（了解）

```shell
#扩展文件系统
[root@localhost ~]# xfs_growfs /dbbak

#[root@localhost ~]# df -hT
/dev/mapper/systemvg-mylv xfs        19G   33M   19G    1% /dbbak

#查看卷组信息
[root@localhost ~]# vgs
  VG       #PV #LV #SN Attr   VSize   VFree   
  centos     1   2   0 wz--n- <19.00g       0 
  systemvg   2   1   0 wz--n-  19.99g 1016.00m

#扩容卷组
```

#### 扩展卷组

- 卷组的空间来源于物理分区，当卷组没有足够空间提供给逻辑卷时，须扩容卷组
- 扩展卷组命令：vgextend

```shell
[root@localhost ~]# vgextend systemvg /dev/sdb5 /dev/sdb6 /dev/sdb7 /dev/sdb8

[root@localhost ~]# vgs
  VG       #PV #LV #SN Attr   VSize   VFree  
  centos     1   2   0 wz--n- <19.00g      0 
  systemvg   6   1   0 wz--n- <59.98g <40.98g

#扩容逻辑卷
[root@localhost ~]# lvextend -L +40G /dev/systemvg/mylv 


[root@localhost ~]# lvs
  LV   VG       Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root centos   -wi-ao---- <17.00g                                                    
  swap centos   -wi-ao----   2.00g                                                    
  mylv systemvg -wi-ao----  59.00g   
  
#扩展文件系统
[root@localhost ~]# xfs_growfs /dbbak
/dev/mapper/systemvg-mylv   59G   34M   59G    1% /dbbak
```

#### 课后作业

1.查看/var/log目录下以包含log的文件

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# ls /var/log/*log*

2.查看/var/log目录下以数字结尾的文件

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# ls /var/log/*[0-9]

3.查看/var/log目录下以字母结尾的文件（包括大写）

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# ls /var/log/*[a-Z]

4.过滤/etc/sudoers文件以root开头的行

root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# grep ^root /etc/sudoers
root ALL=(ALL) ALL

5.看/etc/sudoers文件有效的配置

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# grep -v '^#' /etc/sudoers | grep -v '^$' -n

6.查找/etc/目录下crontab文件存放位置，并查看文件内容

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# find /etc/ -name crontab -type f

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# cat /etc/crontab

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# find /etc/ -name crontab -type f -exec cat {} \;

7.查找10分钟内被修改的文件

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# find / -cmin -10 -type f

8.查找/var/log目录下30天之前被修改且大于1M的文件，清空文件内容

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# find /var/log -mtime +30 -type f -size +10k -exec cp /dev/null {} \;

9.Linux下你常熟悉的压缩格式有哪些？

gzip bzip2 xz

10.对/home目录打包并压缩，打包后名为home.tar.gz

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# tar -czf home.tar.gz /home

11.将home.tar.gz压缩包内容解压至/homebak目录下

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# tar -xvf home.tar.gz -C /homebak/

12.MBR分区格式可以划分多少个主分区？支持多大容量磁盘？

4个主分区，2.2T

13.GPT分区格式可以划分多少个主分区？支持多大容量磁盘？

128主分区，18EB

14.CentOS7分区默认使用的文件系统类型是什么？

xfs

15.如何查看一块磁盘的分区格式？及扩展分区大小？

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# fdisk -l /dev/sdc

磁盘标签类型：gpt

16如何查看一块磁盘剩余容量？

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# lsblk /dev/sdc

17.linux下开机自动挂载文件是哪个？

/etc/fstab

18.如何查看一个分区文件系统类型？及使用情况？

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# df -hT

19.为根分区扩容40G空间

```shell
#查看根分区卷组
[root@localhost ~]# vgs
  VG       #PV #LV #SN Attr   VSize   VFree   
  centos     1   2   0 wz--n- <19.00g       0 

#扩容根分区卷组
[root@localhost ~]# vgextend centos /dev/sdc2 /dev/sdc3

#查看根分区逻辑卷信息
[root@localhost ~]# lvs
  LV   VG       Attr       LSize   
  root centos   -wi-ao---- <17.00g  
  
#扩容逻辑卷
[root@localhost ~]# lvextend -L +39G /dev/centos/root 

#查看逻辑卷信息
[root@localhost ~]# lvs
  root centos   -wi-ao---- <56.00g  
  
#查看正在使用的分区信息
[root@localhost ~]# df -hT
文件系统                类型      容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root xfs        17G  4.4G   13G   26% /

#扩容文件系统
[root@localhost ~]# xfs_growfs /

#查看使用情况
[root@localhost ~]# df -h
文件系统                 容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root   56G  4.4G   52G    8% /
```
