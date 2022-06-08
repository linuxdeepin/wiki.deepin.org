---
title: 安装virtualbox增强功能全屏化显示
description: 
published: true
date: 2022-05-18T11:26:33.166Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:48:03.665Z
---

在Windows上通过Virtualbox安装deepin深度操作系统，安装完成后，为了让deepin可以在virtualbox中全屏显示，需要安装增强工具，但是不少同学点击安装增强工具后会出现无法安装的情况。

在【设备】中点击【安装增强功能】时，会跳出“未能加载虚拟光盘C:\Program Files\Oracle\VirtualBox\VBoxGuestAdditions.iso 到虚拟电脑Deepin.”的提示框，从而无法安装增强功能实现deepin全屏化的显示。

遇到这个问题该怎么处理呢？下面，小编告诉大家怎样借助命令行来安装增强功能。

## 安装步骤
一、首先检查安装的VBox的目录中有无增强工具的镜像文件（VBoxGuestAdditions.iso）。

一般而言，官网下载的VBox安装后是可以找到的，打开VBoxGuestAdditions.iso镜像文件，找到VBoxLinuxAdditions.run。（这个是安装增强的关键运行文件）

二、在deepin服务其中的操作

（一）打开deepin的文件管理器查看刚刚寻找的文件：

「玩转deepin」如何安装VirtualBox增强功能使得deepin全屏显示？
（二）运行VBoxLinuxAdditions.run文件

在deepin系统中运行VBoxLinuxAdditions.run文件，则可实现全屏化运行效果。<kbd>ctrl</kbd>+<kbd>alt</kbd>+<kbd>t</kbd> 打开 terminal窗口，输入如图命令，deepin是我的本机用户名，在操作时应参考你的本机名。

找到增强工具的运行文件，然后运行VBoxLinuxAdditions.run文件。

命令解释：ls 列出查看当前目录。cd ..回到上级目录 cd /文件夹名或者路径名/ 转到对应的目录文件。

输入命令 `sudo ./VBoxLinuxAdditions.run`，运行结束后，重启virtualbox中的deepin系统即可实现全屏化运行。

不太懂命令行的同学可以在deepin的系统盘中找到media文件夹—用户名文件夹（小编的用户名为deepin），打开后可以看到VBox_GAs_6.0.10，然后右击在终端中打开。

在终端中打开后，进入终端页面，只需要输入`sudo ./VBoxLinuxAdditions.run`，回车，运行完成重启后可实现全屏化运行。
