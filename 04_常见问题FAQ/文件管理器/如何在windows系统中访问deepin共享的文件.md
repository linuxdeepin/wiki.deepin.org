---
title: 如何在windows系统中访问deepin中共享的文件
description: 在windows系统中访问deepin中共享的文件的方法之一
published: true
date: 2023-06-14T06:10:12.914Z
tags: deepin, windows, 共享文件
editor: markdown
dateCreated: 2023-06-14T03:04:41.045Z
---

# 如何在windows系统中访问deepin共享的文件
本文参考文章：[Deepin(3) 与windows共享文件夹](https://www.dandelioncloud.cn/article/details/1602475667742736386)
### 一、deepin中共享文件夹
1. 右键点击需分享的文件夹，右键菜单中选择“共享文件夹”项，如下图：
![截图_选择区域_20230614104657.jpg](/for_trans/共享文件/截图_选择区域_20230614104657.jpg)
2. 勾选“共享文件夹”，权限设置为“可读写”，需记住共享文件的用户名，通过“修改密码”设置共享密码，如下图：
![截图_选择区域_20230614104829.jpg](/for_trans/共享文件/截图_选择区域_20230614104829.jpg)
### 二、查看deepin系统中ip
Ctrl+Alt+T快捷方式打开终端，输入命令：ifconfig，查看ip，如下图：
![截图_deepin-terminal_20230614105652.jpg](/for_trans/共享文件/截图_deepin-terminal_20230614105652.jpg)
### 三、windows系统中查看deepin共享的文件
使用快捷键win+r唤出运行窗口，输入`\\+deepin系统的ip`，如下：
![wine_r.jpg](/for_trans/共享文件/wine_r.jpg)
回车后，输入用户名和密码即可查看deepin系统中共享的文件：
![ip.jpg](/for_trans/共享文件/ip.jpg)