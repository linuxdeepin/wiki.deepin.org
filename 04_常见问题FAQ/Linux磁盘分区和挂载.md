---
title: Linux 磁盘分区和挂载
description: 
published: true
date: 2023-03-15T06:03:20.034Z
tags: 
editor: markdown
dateCreated: 2023-03-15T05:45:24.062Z
---

# Linux磁盘分区和挂载
#### linux 分区

**原理介绍**

- 1.Linux 来说 wulun 有几个分区，分给哪一目录使用，他归根结底只有一个根目录，一个独立且唯一的文件结构，Linux 中每个分区都是用来组成整个文件系统的一部分。
- 2.Linux 采用了一种叫 "载入" 的处理方法，它的整个文件系统中包含了一整套的文件和目录，且将一个分区和一个目录联系起来，这是要载入的一个分区将使它的存储空间在一个，目录下获得。

分区和文件关系示意图：

![2023-3-15_20296.png](/2023-3-15_20296.png)

**硬盘说明**

- 1.Linux 硬盘分 IDE 硬盘和 SCSI 硬盘，目前基本上是 SCSI 硬盘
- 2.对于**IDE 硬盘**，驱动器标识符为 "hdx~"，其中 "hd" 表明分区所在设备的类型，这里是指 IDE 硬盘了。"x" 为盘号 (a 为基本盘，b 为基本从属盘，c 为辅助主盘，d 为辅助从属盘)，"~" 代表分区，前四个分区用数字 1 到 4 表示，它们是主分区或扩展分区，从 5 开始就是逻辑分区。例，hda3 表示为第一个 IDE 硬盘上的第三个主分区或扩展分区，hdb2 表示为第二个 IDE 硬盘上的第二个主分区或扩展分区。
- 3.对于**SCSI 硬盘**则标识为 "sdx~"，SCSI 硬盘是用 "sd" 来表示分区所在设备的类型的，其余则和 IDE 硬盘的表示方法一样（x 可以为 abcd 分别对应第 1、2、3、4 块硬盘）。

#### 查看所有设备挂载情况

> 指令：**lsblk 或者 lsblk -f**

```
[root@kongchao03 ~]# lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0    20G  0 disk 
├─sda1   8:1    0  1023M  0 part /boot
├─sda2   8:2    0    17G  0 part /
└─sda3   8:3    0     2G  0 part [SWAP]
sr0     11:0    1 729.9M  0 rom  /run/media/root/20210907_143734
[root@kongchao03 ~]#
```

![2023-3-15_842.png](/2023-3-15_842.png)

![2023-3-15_77939.png](/2023-3-15_77939.png)

> 这里 sda1、2、3 分别代表第一块硬盘的第一分区第二分区...

#### 挂载案例

### 步骤 1：新建一块硬盘

在虚拟机菜单中，设置增加一块硬盘，完成后**重启**可以生效识别

![2023-3-15_64808.png](/2023-3-15_64808.png)

使用 lsblk 命令查看

![2023-3-15_29813.png](/2023-3-15_29813.png)

### 操作步骤 2：虚拟机硬盘分区

> 分区指令：**fdisk  /dev/sdb**

开始对 sdb 分区

- m 显示命令列表
- p 显示磁盘分区同 fdisk -l
- n 新增分区
- d 删除分区
- w 写入并退出

> 说明：开始分区后输入 n，新增分区，然后选择 p，分区类型为主分区。两次回车默认剩余全部空间，最后输入 w 写入分区并退出，若不保存退出输入 q

```
[root@kongchao03 ~]# fdisk /dev/sdb
欢迎使用 fdisk (util-linux 2.23.2)。
 
> 更改将停留在内存中，直到您决定将更改写入磁盘。使用写入命令前请三思。
 
> Device does not contain a recognized partition table
>> 使用磁盘标识符 0xdf03b737 创建新的 DOS 磁盘标签。
 
命令(输入 m 获取帮助)：m            
命令操作
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   g   create a new empty GPT partition table
   G   create an IRIX (SGI) partition table
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)
命令(输入 m 获取帮助)：n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
分区号 (1-4，默认 1)：1
起始 扇区 (2048-2097151，默认为 2048)：
将使用默认值 2048
Last 扇区, +扇区 or +size{K,M,G} (2048-2097151，默认为 2097151)：
将使用默认值 2097151
分区 1 已设置为 Linux 类型，大小设为 1023 MiB
命令(输入 m 获取帮助)：w
The partition table has been altered!
Calling ioctl() to re-read partition table.
正在同步磁盘。
[root@kongchao03 ~]#
```

![2023-3-15_87841.png](/2023-3-15_87841.png)

### 步骤 3：虚拟机硬盘分区格式化

格式化磁盘，格式化之后才会分配 UUID

> 格式化指令：**mkfs -t ext4   /dev/sdb1**

其中 ext4 是分区类型

```
mkfs -t ext4 /dev/sdb1
lsblk -f
```

![2023-3-15_92113.png](/2023-3-15_92113.png)

### 步骤 4：将磁盘挂载到根目录下 newdisk 目录下

> 也可以到其他目录下挂载：将一个分区与一个目录联系起来，

#### mount 挂载

> 挂载语法：**mount  设备名称  挂载目录** （挂载目录是任意的）







