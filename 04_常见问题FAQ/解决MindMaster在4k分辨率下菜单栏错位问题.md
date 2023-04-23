---
title: 解决MindMaster在4k分辨率下菜单栏错位问题
description: 
published: true
date: 2023-03-01T13:53:19.268Z
tags: 
editor: markdown
dateCreated: 2023-03-01T13:52:51.214Z
---

# 前言

一直在用的一个脑图软件，之前用的网页版，得知它支持linux，于是试了一下，结果发现菜单栏字体错位严重。反复安装卸载，今天终于解决。

## 表现如下：

![2023-3-1_8395.png](/2023-3-1_8395.png)

经过观察，1080p是正常的。4k分辨率如果不缩放也是正常的。4k分辨率下，设置界面设置不缩放，然后启动界面添加qt应用缩放2倍(export QT_SCALE_FACTOR=2.00)也是正常的。但是当设置4k分辨率，全局200%缩放时，应用不正常。

## 解决方法

添加QT自动缩放参数 export QT_AUTO_SCREEN_SCALE_FACTOR=1使得MindMaster可以在QT下面正确缩放。具体操作如下：

## 编辑启动脚本

```sudo vim /opt/MindMaster-10/mindmaster.sh```

再倒数第二行添加自动缩放参数 

```export QT_AUTO_SCREEN_SCALE_FACTOR=1，```

添加后如下所示：

`
export LD_LIBRARY_PATH='/opt/MindMaster-10/lib/'
export QT_AUTO_SCREEN_SCALE_FACTOR=1
/opt/MindMaster-10/MindMaster "$@"
`

按下，Esc键，按wq保存即可

## 结果展示

![2023-3-1_19364.png](/2023-3-1_19364.png)