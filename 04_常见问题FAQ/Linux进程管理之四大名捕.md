---
title: Linux进程管理之四大名捕
description: 
published: true
date: 2023-03-07T02:56:22.694Z
tags: 进程
editor: markdown
dateCreated: 2023-03-07T02:30:34.581Z
---

# Linux进程管理之四大名捕
## 一、四大名捕
本文四大名捕由 linux 命令所出演：
无情：ps     出演
铁手：dstat  出演
追命：top    出演
冷血：htop   出演

## 二、进程相关基础知识
介绍四大名捕之前先介绍一下进程相关的基础知识，话不多说，看图。

![2023-3-7_2245.png](/2023-3-7_2245.png)

## 三、轻功暗器高手 “无情” [PS]
ps：用于显示当前进程的状态（非动态）
ps [options]:
选项有三种风格：
1、UNIX 风格，必须在选项前面加 “-”
2、BSD 风格，选项前不能加 “-”
3、GNU 风格，选项前为两个 “-”
常用组合之一：aux
a：所有与终端相关的进程
x：所有与终端无关的进程
u：以用户为中心组织进程状态信息显示

![2023-3-7_3335.png](/2023-3-7_3335.png)

CPU%：cpu 时间占用比率
MEM%：内存占用百分比
VSZ：virtual size 虚拟内存集；
RSS：Resident Size，常驻内存集；
STAT：
R：running 运行
S：interruptable sleeping 可中断睡眠
D：uninterruptable sleeping 不可中断睡眠
T：Stopped 停止
Z：zombie 僵死态
+：前台进程
l：多线程进程
N：低优先级进程
<：高优先级进程
s：session leader  进程领导者
常用组合之二：-ef
-e：显示所有进程
-f：显示完整格式的进程信息
