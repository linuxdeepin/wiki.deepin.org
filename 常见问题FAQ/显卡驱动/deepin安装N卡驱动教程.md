---
title: deepin安装N卡驱动教程
description: 
published: true
date: 2022-06-20T09:22:56.236Z
tags: n卡 显卡 驱动
editor: markdown
dateCreated: 2022-06-20T09:22:28.761Z
---

# deepin安装N卡驱动教程
**一、前言**

deepin20.6仓库内的N卡驱动已经更新到了510版本

对于大部分用户而言，已经不需要用run文件来安装驱动

（如有需要可以看我之前的教程）

https://bbs.deepin.org/zh/post/232923?offset=0&postId=1317417

通过apt安装会更加简单

（不需要手动禁用nouveau、不需要更改配置文件、不需要显卡驱动管理器）

------

**二、安装N卡驱动**

跟着下面的步骤来就行

tips：下面的指令都可以用ctrl c复制，然后在终端里面用ctrl alt v进行粘贴（可以在终端右上角菜单里自定义修改快捷键），回车执行

**1、卸载原有驱动**

sudo apt autoremove nvidia-*

**2、添加32位架构并刷新源**

sudo dpkg --add-architecture i386
sudo apt update