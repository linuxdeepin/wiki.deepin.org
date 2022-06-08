---
title: UEFI 安装15.4 RC 踩坑记
description: 
published: true
date: 2022-05-17T11:37:03.509Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:43:36.059Z
---

## 简介
本经验由论坛用户(comzhong)分享，[原文链接](https://bbs.deepin.org/forum.php?mod=viewthread&tid=136621)

我一直都用U盘启动ISO镜像进行安装，这次新装15.4RC，一台电脑 legacy BIOS一下就装好了，自己的电脑 UEFI BIOS 却一直失败，记录一下关键点。

## 正文

### 非标准ESP分区
我的ESP分区是手动分的，安装Windows都是进PE后，DiskGenius调整分区，用wim镜像安装器进行安装（解压镜像，添加引导），WinNTSetup可以手动指定引导分区，安装的引导类型，非常清楚。

Deepin安装器不识别非标准ESP分区，又不能手动指定，那就要在启动安装器之前（重点），将ES分区标志设置正确，设置为：esp，这样再打开安装器就可以识别了。

### UEFI引导安装
我之前是装了15.3的，这次直接覆盖安装（选了格式化分区），但是到67%就卡死，试了几次，又打开资源监视器看进程，发现是在efiboormgr处理的地方卡死，百思不得其姐，试了最少7、8次，想到尝试删除原来15.3留在ESP分区的引导文件，也就是(EPS)/EFI/deepin 和 (ESP)/EFI/ubuntu 两个文件夹，结果居然可以了，这样的问题不应该出现的吧？

### fstab文件，分区挂载问题
安装完以后，本以为OK了，重启开机，图标满了，没出现登录界面，提示cannot open access to console,the root account is locked. 又搜索，发现是分区挂载问题，进live挂载系统分区找到/etc/fstab文件，发现有这么多多余的东西，删掉后重启，重要进入了期待的登录界面，这个问题也完全可以避免的，安装后只写入必要分区的挂载就行，多余的不要。

###我安装之前，UEFI启动序列有一个grub2启动项，但是文件目录和Deepin的grub2启动文件不冲突，为什么给我删掉了？
###安装成功时，突然弹出光驱，吓我一跳，难道不能检测到是不是物理光驱启动的吗？
###这条算建议，UEFI启动项名就是个“deepin”，弄个“Deepin Linux”多好。