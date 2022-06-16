---
title: DOCK异常
description: 
published: true
date: 2022-06-16T03:07:16.646Z
tags: 
editor: markdown
dateCreated: 2022-06-16T03:07:16.646Z
---

# DOCK异常
当dock上图标一会有一会又没有, 或者dock位置不能点击,或者dock位置变到最上面
## 根因分析：
这种情况一般都是dde-session-daemon在不断重启(此时可以进入桌面)

## 判断方法：

在终端输入 : /usr/lib/deepin-daemon/dde-session-daemon 输入上述命令后, 会将错误日志打印出来, 此时大概率是deepin-desktop-schemas版本与dde-daemon不匹配

## 修改方法：

1.下载或者从其他机器拷贝与dde-daemon版本匹配的deepin-desktop-schemas, 然后进行安装
2.如果无法拿到新版本的deepin-desktop-schemas的deb包, 可以将dde-daemon降级; 通过apt policy dde-daemon, 查询dde-daemon的版本信息, 可以使用命令安装其他版本: sudo apt install dde-daemon=xxx