---
title: 双系统安装时为什么要先安装windows再安装linux
description: 
published: true
date: 2023-02-22T09:38:44.836Z
tags: 双系统 windows
editor: markdown
dateCreated: 2023-02-22T02:21:06.302Z
---

# 首先了解一下开机的过程有哪些？
以个人计算机架设的Linux主机为例，按下电源键后

1. 加载BIOS硬件信息，进行硬件系统的自我测试，取得第一个可开机的装置（BIOS决定）
2. 读取并执行MBR（开机装置的第一个扇区（Sector））的开机管理程序（也叫Boot loader）
3. 开机管理程序(Boot loader)指定使用哪个核心（kernel）来开机，kernel开始侦测硬件与加载驱动程序。
4. kernel呼叫systemd程序，开机。

# Boot Loader
## Boot loader的功能

- **提供选单**：用户可以选择不同的开机项目，这也是多重引导的重要功能！
- **载入核心文件**：识别操作系统的文件格式据以加载核心到主存储器中执行,来开始操作系统 (注意：不同的操作系统的文件格式不一样，因此每种操作系统都有自己的boot loader）
- **转交其他loader**: 将开机管理功能转交给其他loader

如果在主机上安装不同的操作系统

1. 必须要使用自己的loader才能加载属于自己的操作系统核心
2. 系统的MBR只有一个，如何在一部主机上安装windows与Linux呢？

回顾linux文件系统，每个文件系统（file system，或时partition）都会保留一块**启动扇区**（boot sector）提供操作系统安装boot loader，所以当同时安装windows和linux后，该boot sector，boot loader与MBR的相关性如下：

![2023-2-22_14622.png](/2023-2-22_14622.png)

如图，各个操作系统都可以安装一份boot loader到boot sector中，这样各个操作系统就可以透过自己的boot loader来加载核心了。

**但是！因为Winodws的loader不具有控制权转交的功能，即不能使用windows的loader来加载Linux的loader！所以需要先装windows在装linux。这样MBR里面，先装的windows的loader才会被后装的windows的boot loader所覆盖，使用Linux的loader的选单功能，才能够启动双系统**

![2023-2-22_17558.png](/2023-2-22_17558.png)

