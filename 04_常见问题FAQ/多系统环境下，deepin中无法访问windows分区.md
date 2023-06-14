---
title: 多系统环境下，deepin中无法访问windows分区
description: 多系统环境下，deepin系统中无法访问windows系统中文件，无权限创建文件和文件夹
published: true
date: 2023-06-14T09:36:31.666Z
tags: 多系统, 快速启动
editor: markdown
dateCreated: 2023-06-14T09:30:25.801Z
---

# 因windows系统开启快速启动，导致deepin中无法访问windows分区

### 一、问题背景
论坛中多个用户反馈出现deepin下访问Windows系统盘时文件上锁，无法在windows分区创建文件或文件夹的情况：
[https://bbs.deepin.org/post/255976](https://bbs.deepin.org/post/255976)
[https://bbs.deepin.org/post/243993](https://bbs.deepin.org/post/243993)
[https://bbs.deepin.org/post/240619](https://bbs.deepin.org/post/240619)

### 二、问题原因
有关开启“快速启动”导致deepin中无法访问windows分区的原因，请教过研发同学，回复说：可能在快速启动模式下，关机时windows会将硬盘分区写保护，所以再次进入linux后，也就无法向这些分区写入数据了。

### 三、解决方法-windows关闭快速启动
部分用户的问题可通过在windows系统中关闭“快速启动”的方式，来获取访问windows分区的权限。
windows系统中使用win+x，弹出常用菜单，选择“电源选项”：
![1.jpg](/for_trans/快速启动/1.jpg)
在设置-电源和睡眠界面选择“其它电源设置”项：
![2.jpg](/for_trans/快速启动/2.jpg)
通过“电源按钮的功能”项进入系统设置界面：
![3.jpg](/for_trans/快速启动/3.jpg)
通过“更改当前不可用的设置”项，取消“快速启动”设置：
![4.jpg](/for_trans/快速启动/4.jpg)
保存修改后关机，开机后进入deepin系统，即可在deepin系统中向windows系统分区写入数据：
![写数据.jpg](/for_trans/快速启动/写数据.jpg)