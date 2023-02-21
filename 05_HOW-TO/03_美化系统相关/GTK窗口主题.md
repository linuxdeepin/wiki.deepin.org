---
title: GTK窗口主题
description: 
published: true
date: 2022-10-18T04:51:23.565Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:51:40.214Z
---

## 前言

本小节主要介绍深度操作系统默认桌面环境下,如何设置GTK主题和窗口主题.

> 由于DTK程序并未使用传统的标题栏，其本质上是在一个无边框窗口中加入一个类似标题栏的控件实现的。因此，对标题栏主题的修改并不会影响DTK窗口的样式。其它使用非传统标题栏的应用程序同理。

## 什么是GTK主题

## 配置文件

GTK主题和窗口主题的配置文件存放目录有:

- ~/.themes    （用户自定义配置）
- /usr/share/themes    （全局配置）

## 安装

便捷安装

在网上下载并安装窗口主题的Deb文件后，打开控制中心——个性化——窗口，设置喜欢的窗口主题即可。

手动安装

打开带Root权限的文件管理器进入窗口主题的目录，终端执行：

    sudo dde-file-manager /usr/share/themes

最后打开控制中心——个性化——窗口，设置喜欢的窗口主题即可。

## 卸载

打开带Root权限的文件管理器进入窗口主题的目录，终端执行：

    sudo dde-file-manager /usr/share/themes

删除不需要的窗口主题即可。

## 相关链接

[GTK3.X主题资源下载](http://gnome-look.org/index.php?xcontentmode=167)

[GTK2/3 主题推荐](http://planet.linuxdeepin.com/2012/04/12/gtk-2-and-gtk-3-theme-for-linux-deepin/)
