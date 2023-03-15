---
title: Linux 磁盘分区和挂载
description: 
published: true
date: 2023-03-15T06:00:23.166Z
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

