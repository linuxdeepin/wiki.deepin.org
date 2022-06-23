---
title: 创建RAID磁盘阵列
description: 
published: true
date: 2022-06-23T05:53:02.140Z
tags: 
editor: markdown
dateCreated: 2022-06-23T05:53:00.031Z
---

# 创建RAID磁盘阵列
使用mdadm命令来创建RAID磁盘阵列，关于mdadm详细用法请参考[man mdadm](https://linux.die.net/man/8/mdadm)
## 磁盘格式化
使用fdisk对需要使用的磁盘进行格式化
## 创建raid6
使用如下命令：
```
mdadm -C -v /dev/md6 -a yes -l 6 -n 7 /dev/sd[b-h]1
```
此处列出目前使用到的参数介绍： - -C: 创建选项 - -v: 显示详细过程 - /dev/md6: 创建raid卷后的存放路径 - -l 6: raid级别 - -n 7: 创建raid卷所需要的磁盘数 - /dev/sd[b-h]1: 构建raid卷的具体磁盘
## 验证创建结果
```
root@devops-PC:~# mdadm -D /dev/md6
/dev/md6:
          Version : 1.2
    Creation Time : Wed Mar  3 10:27:15 2021
       Raid Level : raid6
       Array Size : 2929646080 (2793.93 GiB 2999.96 GB)
    Used Dev Size : 585929216 (558.79 GiB 599.99 GB)
     Raid Devices : 7
    Total Devices : 7
      Persistence : Superblock is persistent
    Intent Bitmap : Internal
      Update Time : Wed Mar  3 10:29:36 2021
            State : clean, resyncing 
   Active Devices : 7
  Working Devices : 7
   Failed Devices : 0
    Spare Devices : 0
           Layout : left-symmetric
       Chunk Size : 512K
Consistency Policy : bitmap
    Resync Status : 3% complete
             Name : devops-PC:6  (local to host devops-PC)
             UUID : e0e63169:752d5cab:df0efcea:98b1f6dd
           Events : 28
   Number   Major   Minor   RaidDevice State
      0       8       16        0      active sync   /dev/sdb
      1       8       32        1      active sync   /dev/sdc
      2       8       48        2      active sync   /dev/sdd
      3       8       64        3      active sync   /dev/sde
      4       8       80        4      active sync   /dev/sdf
      5       8       96        5      active sync   /dev/sdg
      6       8      112        6      active sync   /dev/sdh
```
## 格式化
```
root@devops-PC:~# mkfs.ext4 /dev/md6
mke2fs 1.44.5 (15-Dec-2018)
Creating filesystem with 732410240 4k blocks and 183107584 inodes
Filesystem UUID: f2ee12a8-2c96-48d5-af11-f555d2eef1f5
Superblock backups stored on blocks: 
       32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
       4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968, 
       102400000, 214990848, 512000000, 550731776, 644972544
Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (262144 blocks): done
Writing superblocks and filesystem accounting information: 
done    
```
## 配置开机自动挂载
### 1.首先确定挂载目录，没有则创建一个
```
mkdir -p /media/md6
```
### 2. 查看分区信息，使用`blkid`获取挂载磁盘的uuid
```
root@devops-PC:~# blkid 
/dev/sde1: UUID="519f7038-2bfe-d51e-42cb-ce1c30127df3" UUID_SUB="8c577c73-7874-cc59-8c3e- 4af69b60f1ca" LABEL="devops-PC:6" TYPE="linux_raid_member" PARTUUID="692e402e-6a02-6049- 8522-2e734dadac9c"
/dev/sda1: UUID="4C24-B439" TYPE="vfat" PARTUUID="9a968dec-f86a-4777-a7e5-ba9f1354001f"
/dev/sda2: UUID="02055532-ca10-44c8-9003-c953361e99f8" TYPE="swap" PARTUUID="bb280c49-f29a-430f-a08a-66047dfd4259"
/dev/sda3: UUID="a3de0611-85fa-45d4-b1dc-f9155d351677" TYPE="ext4" PARTUUID="49887d7f-f912-4c65-b71b-6d9c43998653"
/dev/sdc1: UUID="519f7038-2bfe-d51e-42cb-ce1c30127df3" UUID_SUB="081a3a35-773f-894e-e016-ac1b7f665c24" LABEL="devops-PC:6" TYPE="linux_raid_member" PARTUUID="f7be70bf-420b-e448-83ce-5474e3ff6851"
/dev/sdb1: UUID="519f7038-2bfe-d51e-42cb-ce1c30127df3" UUID_SUB="f01c0367-8cc4-d072-2b36-d499c68cca9d" LABEL="devops-PC:6" TYPE="linux_raid_member" PARTUUID="1ddccb32-0b02-9748-81c7-7ac56a7c516e"
/dev/sdd1: UUID="519f7038-2bfe-d51e-42cb-ce1c30127df3" UUID_SUB="57c7a45e-9480-afb9-2ecd-c80dbbf66caf" LABEL="devops-PC:6" TYPE="linux_raid_member" PARTUUID="a0d9a6a1-944b-114c-b5ef-808f30992143"
/dev/sdf1: UUID="519f7038-2bfe-d51e-42cb-ce1c30127df3" UUID_SUB="4b103385-cdf4-84ef-484f-e00f1d33b784" LABEL="devops-PC:6" TYPE="linux_raid_member" PARTUUID="565a9e39-629f-1441-8daa-ed8e78053d88"
/dev/sdg1: UUID="519f7038-2bfe-d51e-42cb-ce1c30127df3" UUID_SUB="b0ec4128-95a8-157a-198c-862f996fd9c2" LABEL="devops-PC:6" TYPE="linux_raid_member" PARTUUID="f8c4966c-0d68-8f47-8af5-ed9ab0689d02"
/dev/sdh1: UUID="519f7038-2bfe-d51e-42cb-ce1c30127df3" UUID_SUB="e6a043a8-4f39-ba9f-a68c-bcc234ad5166" LABEL="devops-PC:6" TYPE="linux_raid_member" PARTUUID="4a9c3ae2-94ee-3c40-a8ce-c90f4a33ae97"
/dev/md6: UUID="f2ee12a8-2c96-48d5-af11-f555d2eef1f5" TYPE="ext4"
```
### 3. 获取到UUID后，在/etc/fstab表中，写入自动挂载条目 - 编写fstab条目说明
```
<fs spec> <fs file> <fs vfstype> <fs mntops> <fs freq> <fs passno>
```
具体说明，以挂载/dev/sdb1为例：
```
<fs spec>：分区定位，可以给UUID或LABEL，例如：UUID=f2ee12a8-2c96或LABEL=software
<fs file>：具体挂载点的位置，例如：/media/md6
<fs vfstype>：挂载磁盘类型，linux分区一般为ext4，windows分区一般为ntfs
<fs mntops>：挂载参数，一般为defaults
<fs freq>：磁盘检查，默认为0
<fs passno>：磁盘检查，默认为0，不需要检查
```
- 编写示例
```
root@devops-PC:~# cat /etc/fstab 
# /dev/sda3
UUID=a3de0611-85fa-45d4-b1dc-f9155d351677       /               ext4            rw,relatime     0 1
```
```
# /dev/sda1
UUID=4C24-B439          /boot/efi       vfat            rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=iso8859-1,shortname=mixed,utf8,errors=remount-ro 0 2
```
```
# /dev/sda2
UUID=02055532-ca10-44c8-9003-c953361e99f8       none            swap            defaults,pri=-2 0 0
```
```
# /dev/md6
UUID=f2ee12a8-2c96-48d5-af11-f555d2eef1f5       /media/md6      ext4            defaults 0 0
```
配置后，执行`mount -a`验证，是否有fstab条目的任何错误
```
root@devops-PC:/media/md6# mount -av
/                        : ignored
/boot/efi                : already mounted
none                     : ignored
/media/md6               : already mounted
```
## 保存raid6配置
确保上述没有问题后，将raid6的配置信息，保存起来，需要写入/etc/mdadm/mdadm.conf文件中
```
root@devops-PC:/media/md6# mdadm --detail --scan --verbose >> /etc/mdadm/mdadm.conf
root@devops-PC:~# cat /etc/mdadm/mdadm.conf 
# mdadm.conf
#
# !NB! Run update-initramfs -u after updating this file.
# !NB! This will ensure that initramfs has an uptodate copy.
#
# Please refer to mdadm.conf(5) for information about this file.
#

# by default (built-in), scan all partitions (/proc/partitions) and all
# containers for MD superblocks. alternatively, specify devices to scan, using
# wildcards if desired.
#DEVICE partitions containers
```
```
# automatically tag new arrays as belonging to the local system
HOMEHOST <system>
```
```
# instruct the monitoring daemon where to send mail alerts
MAILADDR root
```
```
# This configuration was auto-generated on Thu, 28 Jan 2021 08:19:55 +0000 by mkconf
ARRAY /dev/md6 level=raid6 num-devices=7 metadata=1.2 name=devops-PC:6 
UUID=519f7038:2bfed51e:42cbce1c:30127df3
devices=/dev/sdb1,/dev/sdc1,/dev/sdd1,/dev/sde1,/dev/sdf1,/dev/sdg1,/dev/sdh1
```
执行`update-initramfs -u`更新引导文件，确保重启后，RAID的名称不会改变
```
root@devops-PC:/etc# update-initramfs -u
update-initramfs: Generating /boot/initrd.img-4.19.0-arm64-server
cryptsetup: WARNING: The initramfs image may not contain cryptsetup binaries 
   nor crypto modules. If that's on purpose, you may want to uninstall the 
   'cryptsetup-initramfs' package in order to disable the cryptsetup initramfs 
   integration and avoid this warning.
I: The initramfs will attempt to resume from /dev/sda2
I: (UUID=02055532-ca10-44c8-9003-c953361e99f8)
I: Set the RESUME variable to override this.
```
  


