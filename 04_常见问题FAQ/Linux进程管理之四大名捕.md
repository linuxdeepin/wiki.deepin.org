---
title: Linux进程管理之四大名捕
description: 
published: true
date: 2023-03-07T02:57:30.368Z
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

![2023-3-7_41699.png](/2023-3-7_41699.png)

常用组合之三：-eFH
-F：显示完整格式的进程信息；
C：cpu utilization cpu 占用百分比
PSR：运行于哪颗 CPU 之上
-H：以层级结构显示进程的相关信息；

![2023-3-7_68691.png](/2023-3-7_68691.png)

常用组合之四：-eo, axo
o  field1, field2,…：自定义要显示的字段列表，以逗号分隔
常用的 field：pid, ni, priority, psr, pcpu, stat, comm, tty, ppid, rtprio
pid：进程的 pid 号
ni：nice 值
priority：优先级
psr：运行在那颗 cpu
pcpu：cpu 利用率
ppid：父进程的 id 号
rtprio：实时优先级
四、内功卓越的高手 “铁手”[dstat]
dstat：系统资源统计命令（动态）
dstat [-afv] [options..] [delay [count]]

![2023-3-7_29414.png](/2023-3-7_29414.png)

常用选项：
-c， –cpu：显示 cpu 相关信息；
-C #,#,…,total：显示第一个 cpu，第二个 cpu 或者总共的
-d, –disk：显示磁盘的相关信息
-D sda,sdb,…,tobal：显示指定硬盘设备，总空间
-g：显示 page 相关的速率数据；
-m：Memory 的相关统计数据
-n：Interface 的相关统计数据；
-p：显示 process 的相关统计数据；
-r：显示 io 请求的相关的统计数据；
-s：显示 swapped 的相关统计数据；

![2023-3-7_73269.png](/2023-3-7_73269.png)

–tcp：显示 tcp 套接字
–udp：显示 udp 连接
–raw：显示裸套接字
–socket：套接字
–ipc：进程间通信信息

![2023-3-7_33720.png](/2023-3-7_33720.png)

–top-cpu：显示最占用 CPU 的进程；
–top-io：最占用 io 的进程；
–top-mem：最占用内存的进程；

![2023-3-7_75688.png](/2023-3-7_75688.png)
