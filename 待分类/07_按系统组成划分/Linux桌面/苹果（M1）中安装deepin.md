---
title: 苹果（M1）中安装deepin
description: 本文介绍苹果（M1）中安装deepin
published: true
date: 2023-07-05T05:46:00.891Z
tags: 
editor: markdown
dateCreated: 2023-07-05T05:30:20.180Z
---

# 苹果（M1）中安装deepin
## 重要参考资料
Asahi Linux: https://asahilinux.org/

Asahi Linux Wiki: https://github.com/AsahiLinux/docs/wiki/

Asahi Linux Debian installer: https://git.zerfleddert.de/cgi-bin/gitweb.cgi/m1-debian

## 环境
在 苹果（M1）设备上安装 deepin，需要准备一份 deepin 的安装用的 rootfs，使用 asahi 公开的一些内核补丁和 m1n1 完成对系统的引导。
为了完成在 苹果（M1）设备上安装并使用 deepin ，需要准备：
deepin rootfs
m1n1
打包可支持M1 GPU的Mesa修改版
启动DDE桌面
ARM仓库

## 安装流程
deepin ci仓库有提供现成的安装脚本。当然，你也可以通过自行搭建安装仓库的方法自行安装。

如果你不怕麻烦的话，也可以通过仅安装官方m1n1+uboot引导的方式，通过插入写好特制内容的deepin安装盘进行安装。

## 使用 deepin的安装仓库
在MacOS上打开Terminal，然后，运行以下命令执行安装脚本
```
curl https://ci.deepin.com/repo/deepin/deepin-ports/deepin-m1/deepin.install | sh
```
- 注意，跑完脚本后，它会让你关机．(Mac Mini)开机时，长摁开机键直至出现启动菜单．选择deepin，然后跟着脚本走来设置deepin为默认的启动项．
- deepin系统默认用户 hiweed, 密码为 1

## 自行搭建安装仓库
### 准备工作
- 打rootfs包的系统暂时只试过Linux (deepin V20, deepin V23, Arch Linux)，没试过Mac OS本地能否打包
- 安装必要的打rootfs包依赖：

	- debootstrap
	- eatmydata
	- pigz
	- qemu-user-static (非ARM机器上需要)
- 创建指向/usr/share/debootstrap/scripts/buster的/usr/share/debootstrap/scripts/beige脚本
```sudo ln -s /usr/share/debootstrap/scripts/buster /usr/share/debootstrap/scripts/beige```
- deepin以外的发行版需要的

	- 获取keyring: /usr/share/keyrings/deepin-archive-camel-keyring.gpg．非deepin发行版可以通过解包deepin-keyring包获得．
	- (Arch或其他非Debian衍生发行版) 	/usr/share/debootstrap/scripts/debian-common中，需要屏蔽添加usr-	is-merged依赖的那一段switch-case块．

### 搭建仓库
当前仅从Thomas Glanzmann的Asahi Linux Debian安装器仓库修改了bootstrap脚本生成rootfs压缩包．(如果想和上游对比的话，可以自行开启．打包时默认屏蔽掉了．) 

首先，因为安装脚本是在线安装模式的，所以需要先搭建一个安装仓库（推荐为http，其他的没试过，比如本地方式．听justforlxz说本地的话，会在其中某一步挂掉．我还没尝试）(**使用python的http.server搭建的服务器是无法被安装脚本使用的，本人试过了．后面用的apache2的http服务**)

仓库结构如下：
```
/path/to/repo
├── asahilinux.install (可选，一般是修改成使用本文件服务器地址的安装脚本)
├── installer_data.json (使用本项目带的)
└── os
    └── deepin-base.zip (运行本项目中的bootstrap.sh, 然后会在项目的build目录下生成)
    └── deepin-desktop.zip (运行本项目中的bootstrap.sh, 然后会在项目的build目录下生成)
```
    
搭好之后，直接参照[官方教程](https://asahilinux.org/2022/03/asahi-linux-alpha-release/)进行安装．这里只简单描述大致流程．

1. 跑Asahi Linux的安装脚本．一般拿[官方](https://alx.sh)的改INSTALLER_DATA变量成deepin安装仓库地址就行，也可以改本项目中asahilinux.install的REPO_BASE．

   ``` bash
   # 假设你在安装仓库根目录放了安装脚本
   curl protocol://hostname:port/path/to/repo/asahilinux.install | sh
   ```

2. 跟着脚本走就是了
    
## 使用deepin 23 for (M1)安装盘
这里所说的deepin安装盘可**不是给通常机器安装使用的iso镜像盘。**只需要在U盘上**创建一个FAT分区**并**将安装内容写入根目录**即可。

具体步骤如下：

- 创建安装盘

  - 按照m1-debian的介绍，运行一下命令创建分区。
```
# 替换成你U盘的对应设备
DEVICE=/dev/sdX
sudo parted -a optimal $DEVICE mklabel msdos
sudo parted -a optimal $DEVICE mkpart primary fat32 2048s 100%
sudo mkfs.vfat ${DEVICE}1
sudo mount ${DEVICE}1 /mnt
```
在[这里](https://ci.deepin.com/repo/deepin/deepin-ports/deepin-m1/deepin-m1-usb-installer.zip)下载安装盘压缩包，并解压到**U盘FAT分区**的**根目录**。
在Mac上安装m1n1+uboot引导。(Asahi Linux官方安装脚本选第三项UEFI environment only, m1n1+uboot+esp)
```curl https://alx.sh/ | sh ```



