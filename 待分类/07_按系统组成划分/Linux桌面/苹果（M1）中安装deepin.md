---
title: 苹果（M1）中安装deepin
description: 本文介绍苹果（M1）中安装deepin
published: true
date: 2023-07-05T05:37:59.505Z
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
```curl https://ci.deepin.com/repo/deepin/deepin-ports/deepin-m1/deepin.install | sh```
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


