---
title: Dock去掉图标
description: 
published: true
date: 2023-07-15T07:57:34.879Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:33:20.621Z
---

## 去掉Dock上不能移除驻留的图标
> 解决deepin15.4无法去除dock上的关机音量网络时间等图标问题
## 解决方案
- 在/usr/lib/dde-dock/plugins目录下有几个链接库
- 将这几个链接库重命名就能去掉dock上的图标

  
  ```
  mv libsound.so libsound.so.bak #把声音图标去掉
  ```

## 名称对应
- libdatetime.so---------------> 日期时间
- libdde-trash-plugin.so--------->垃圾箱
- libnetwork.so------------------>网络连接
- libsound.so--------------------->声音
- libshutdown.so ---------------->关机电源
- libdde-disk-mount-plugins.so------->你插入硬盘时的挂载图标
- libsystem-tray.so--------------->系统托盘