---
title: Deepin文件系统
description: 
published: true
date: 2022-05-07T07:47:22.126Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:32:38.662Z
---

## deepin默认文件系统
deepin的默认文件系统是ext4，大多数linux系统均采用ext格式的磁盘文件系统。ext4（Fourth extended filesystem）汉语译为“第四代扩展文件系统”，是linux下的日志文件系统。关于ext4可以访问<a href="https://baike.baidu.com/item/Ext4/1858450?fr=aladdin">ext4-百度百科</a>或者访问<a href="https://zh.wikipedia.org/wiki/Ext4">ext4-维基百科</a>。
查看磁盘文件类型的命令是<br/>
`df -Th`<br/>
每个分区的磁盘文件系统类型是不一致的。<br/>

|分区|磁盘文件类型|备注|
|-|-|-|
|/boot|ext4|引导分区|
|/boot/efi|vfat(fat32)|efi引导分区|
|/(根分区)|ext4|整个linux磁盘分区|
|/dev|udev<sup>[1]</sup>|设备管理分区|
|/dev/shm|tmpfs<sup>[2]</sup>|管理磁盘，内存等等设备|
|/run|tmpfs<sup>[2]</sup>|以前的/dev/run<br/>正在运行线程和设备|
|/run/lock|tmpfs<sup>[2]</sup>|部分线程锁文件|
|/run/user/1000|tmpfs<sup>[2]</sup>|正在运行的设备|
|sys/fs/cgroup|tmpfs<sup>[3]</sup>|分组线程进行管理|

*注释
>>[1]：udev是设备节点，参考资料<a href="https://baike.baidu.com/item/udev/989800?fr=aladdin">udev-百度百科</a>和<a href="https://zh.wikipedia.org/wiki/Udev">udev-维基百科</a>

>>[2]：tmpfs是基于内存的暂时文件系统，参考资料<a href="https://baike.baidu.com/item/tmpfs/1476960?fr=aladdin">tmpfs-百度百科</a>和<a href="https://zh.wikipedia.org/wiki/Tmpfs">tmpfs-维基百科</a>

## deepin文件系统的U盘挂载
deepin linux是自动挂载U盘或者移动硬盘的，如果需要在命令行下操作U盘/移动硬盘，挂载点在<br/>
`/media/(用户名)/(U盘名字或者U盘的uuid)`
