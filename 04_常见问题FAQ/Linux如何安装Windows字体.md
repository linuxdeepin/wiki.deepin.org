---
title: Linux如何安装Windows字体
description: 
published: true
date: 2022-07-19T16:14:24.357Z
tags: 
editor: markdown
dateCreated: 2022-07-19T16:13:30.403Z
---

在安装 WPS Office 后，受限于字体版权，并没有自带 Windows 字体，而很多 Windows 字体又是必备的。下面介绍一下 Linux 安装 Windows 字体的方法。

**备注：如果不想自己提取，可以下载我从 Win10 LTSC2019 ISO 中提取好的字体包（7z 格式），然后直接看本文的“安装 Win10 字体”部分。**
夸克网盘链接：https://pan.quark.cn/s/5e9bdc9c638a （提取码：ccBP）
百度网盘链接：https://pan.baidu.com/s/1tKd_jYkwee-gUbIy_I-7jw （提取码：a4f4）
## 挂载 Win10 镜像
Win10 镜像可从 https://next.itellyou.cn/ 下载。
下载完成后就要挂载 Win10 镜像了。
执行下面命令创建挂载点：
```
sudo mkdir /mnt/iso
```
然后执行下面命令挂载 Win10 镜像：
```
sudo mount -o loop 镜像路径 /mnt/iso
```
备注：-o 表示 options（选项），参数 loop 表示将一个文件作为硬盘分区挂载

## 解压 Win10 镜像

挂载完成后，需要安装 wimlib/wimtools 来解压 install.wim/install.esd 文件
```
sudo apt update
sudo apt install wimtools
```
先创建一个用于临时放置字体的文件夹：
```
mkdir Win10Fonts
```
执行命令，解压 install.wim/install.esd 中的 Fonts 文件夹（原版 Win10 镜像一般是 install.wim）：
```
sudo wimextract /mnt/iso/sources/install.wim 1 /Windows/Fonts --dest-dir=Win10Fonts #适用 install.wim
sudo wimextract /mnt/iso/sources/install.esd 1 /Windows/Fonts --dest-dir=Win10Fonts #适用 install.esd
```
## 安装 Win10 字体
在系统字体目录新建一个文件夹用于存放 Win10 字体：
```
sudo mkdir /usr/share/fonts/Win10Fonts
```
将解压得到的 ttf、ttc 字体复制到系统字体目录：
```
sudo cp -r Win10Fonts/*.ttf /usr/share/fonts/Win10Fonts/
sudo cp -r Win10Fonts/*.ttc /usr/share/fonts/Win10Fonts/
```
更改权限：
```
sudo chmod 644 /usr/share/fonts/Win10Fonts/*
```
刷新字体缓存：
```
sudo fc-cache -fv
```
试着打开一下 WPS Office，那些熟悉的 Windows 字体是不是出现了呢？

![wps.webp](/wps.webp)
## 备注
我也在 B 站上发表过本篇教程：https://www.bilibili.com/read/cv16481012
