---
title: 硬件/USB设备
description: 
published: true
date: 2022-05-08T14:08:44.948Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:43:44.702Z
---

## 前言
USB设备为与USB接口上链接的所有设备,但此页面介绍鼠标、键盘、打印机以外的USB设置.
## 查看当前USB设备
图形查看

通知区-存储设备即可查看


命令查看

终端执行：
```
    lsusb   ##查看系统当前USB设备
    lsusb:列出所有USB设备
    语法：lsusb [参数]
    参数：
    -D 设备路径 不扫描/proc/bus/usb，而以指定的设备路径取代
    -p 内核路径 使用其他USB设备在内核的路径，默认为/proc/bus/usb
    -t 将USB设备以树状架构输出
    -v 列出较详细的运行过程
    -vv 列出完整的运行过程
```
## 相关链接
暂无