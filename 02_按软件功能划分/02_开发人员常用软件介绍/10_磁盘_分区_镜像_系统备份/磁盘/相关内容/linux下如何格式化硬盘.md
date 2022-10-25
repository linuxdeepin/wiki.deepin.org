---
title: linux系统如何格式化硬盘
description: 
published: true
date: 2022-06-21T02:26:35.323Z
tags: linux, 格式化, 硬盘
editor: markdown
dateCreated: 2022-06-21T02:26:33.220Z
---

# linux下如何格式化硬盘
### 说说windows
让我们从一些问题开始：

- 当我们给win主机插入一块新的硬盘的时候，我们并不能在”我的电脑”看到”盘符”，而只能到”磁盘管理”或者打开”diskgenius”这些工具才能看到硬盘，为什么呢？
- 我们装机的时候可以在winpe用工具进行硬盘分区，得到了多个盘符，这又是什么原理呢？
- 格式化一个盘符的时候，NTFS和FAT32又是什么呢？

其实硬盘对于电脑来说属于一个”块设备”，”块设备”可以划分为多个”分区”，每个”分区”有一个”盘符”标识，对”分区”做”格式化”就可以在”我的电脑”里看到它了。

接下来，我们将在linux系统中安装一块新的硬盘，让大家对上述打引号的名词有一个清晰的认识。

### 在linux中操作
插入硬盘后，我们首先要查看”块设备”，确保系统识别了硬盘。

###### 1. 查看块设备
```linux 
pi@raspberrypi:~ $ lsblk 
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda           8:0    0  118G  0 disk 
mmcblk0     179:0    0 59.5G  0 disk 
├─mmcblk0p1 179:1    0 43.9M  0 part /boot
└─mmcblk0p2 179:2    0 59.4G  0 part /
```
 *lsblk命令可以列出所有系统识别的“块设备”* 

sda块设备是disk类型，也就是一块硬盘。

块设备首先需要分区，当然你可以把整块硬盘分成1个区也是可以的。

利用fdisk命令对/dev/sda块设备进行分区：
```linux
pi@raspberrypi:~ $ sudo fdisk /dev/sda

Welcome to fdisk (util-linux 2.29.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Command (m for help):
```
敲入m可以看帮助菜单。

F可以查看硬盘剩余的未分区空间大小：
```linux
Command (m for help): F
Unpartitioned space /dev/sda: 118 GiB, 126700469760 bytes, 247461855 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes

Start       End   Sectors  Size
 2048 247463902 247461855  118G
```
这里可以见/dev/sda还剩余118G。

p可以查看现在有几个分区（新硬盘应该是没有分区的）：
```linux
Command (m for help): p
Disk /dev/sda: 118 GiB, 126701535232 bytes, 247463936 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: E3B61140-3FE7-410D-8D65-EA59E46FE78F
```
接下来首先创建一个分区表，采用主流的GPT格式，该命令介绍如下：

*g create a new empty GPT partition table*
```linux
Command (m for help): g
Created a new GPT disklabel (GUID: 839F933F-2CA9-479F-9371-0E436802777F)
```

然后可以进行分区，命令介绍如下：

*n add a new partition*
```linux
Command (m for help): n 
Partition number (1-128, default 1): 
First sector (2048-247463902, default 2048): 
Last sector, +sectors or +size{K,M,G,T,P} (2048-247463902, default 247463902): 

Created a new partition 1 of type 'Linux filesystem' and of size 118 GiB.
Partition #1 contains a ext4 signature.

Do you want to remove the signature? [Y]es/[N]o: y

The signature will be removed by a write command.
```
同一个块设备的每个分区都有唯一编号，默认递增即可。

然后会让你选择分区的起始扇区与结束扇区，默认都是直接占满块设备剩余空间，也正符合我的需要，所以直接敲回车即可。

因为演示的原因，fdisk命令貌似检测到了我原先磁盘做过一次文件系统，所以选择抹除即可。

现在可以查看到分区/dev/sda1，它是/dev/sda这块盘的唯一分区，占据了所有空间：
```linux
Command (m for help): p
Disk /dev/sda: 118 GiB, 126701535232 bytes, 247463936 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 839F933F-2CA9-479F-9371-0E436802777F

Device     Start       End   Sectors  Size Type
/dev/sda1   2048 247463902 247461855  118G Linux filesystem
```
确认之后，输入w命令保存所有修改：
```linux
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

再次lsblk，你将看到sda盘已经有了一个sda1的分区：
```linux
pi@raspberrypi:~ $ lsblk 
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda           8:0    0  118G  0 disk 
└─sda1        8:1    0  118G  0 part 
mmcblk0     179:0    0 59.5G  0 disk 
├─mmcblk0p1 179:1    0 43.9M  0 part /boot
└─mmcblk0p2 179:2    0 59.4G  0 part /
```
###### 2. 格式化文件系统
分区仍旧是块设备，需要制作文件系统后，才能被操作系统挂载。

在windows上，文件系统就是NTFS或者FAT32，在Linux系统下面有很多选择，它们特性各不相同：
```linux
pi@raspberrypi:~ $ mkfs
mkfs         mkfs.bfs     mkfs.cramfs  mkfs.ext2    mkfs.ext3    mkfs.ext4    mkfs.fat     mkfs.minix   mkfs.msdos   mkfs.ntfs    mkfs.vfat
```

在linux最新版本上，特性比较均衡的就是ext4文件系统了，制作文件系统其实就是”格式化”：
```linux
pi@raspberrypi:~ $ sudo mkfs.ext4 /dev/sda1
mke2fs 1.43.4 (31-Jan-2017)
Creating filesystem with 30932731 4k blocks and 7733248 inodes
Filesystem UUID: 69d20b7f-186d-4a14-9615-6c7659a22c29
Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
        4096000, 7962624, 11239424, 20480000, 23887872

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (131072 blocks): 
done
Writing superblocks and filesystem accounting information:        
done
```
敲回车即可完成。

###### 3. 挂载目录
现在需要创建一个空目录，然后把”格式化”好的分区挂载上去：
```linux
mkdir /home/pi/data
```
目录权限根据自己需要修改，然后mount挂载即可：
```linux
sudo mount /dev/sda1 /home/pi/data/
```
现在/home/pi/data就可以访问了，也可以查看挂载情况：
```linux
pi@raspberrypi:~/data $ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        59G  2.8G   54G   5% /
devtmpfs        459M     0  459M   0% /dev
tmpfs           464M  260K  463M   1% /dev/shm
tmpfs           464M   13M  451M   3% /run
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           464M     0  464M   0% /sys/fs/cgroup
/dev/mmcblk0p1   44M   23M   21M  52% /boot
tmpfs            93M     0   93M   0% /run/user/999
tmpfs            93M     0   93M   0% /run/user/1000
/dev/sda1       116G   61M  110G   1% /home/pi/data
```
为了开机自动挂载，需要在/etc/fstab文件末尾追加一行配置：
```linux
/dev/sda1 /home/pi/data ext4 defaults 0 1
```
只需要修改前3个参数，其他参数默认即可（有兴趣可以自己查查）：
- /dev/sda1：哪一个分区
- /home/pi/data：挂载到哪个目录
- ext4：文件系统类型
> 一定要注意，如果fstab配置错误，机器重启就会失败，只能插显示器急救，所以我们在重启前要确保配置正确。
> 
> 只需要执行sudo mount -a重新挂载所有目录，只要没有报错那么就说明一切正常。
{.is-warning}


*关于linux系统的硬盘分区、格式化、挂载，就这些内容。*