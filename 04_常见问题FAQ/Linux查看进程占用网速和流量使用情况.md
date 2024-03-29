---
title: Linux查看进程占用网速和流量使用情况
description: 
published: true
date: 2023-03-13T01:54:30.358Z
tags: vnstat iftop nethogs
editor: markdown
dateCreated: 2023-03-13T01:51:20.724Z
---

# Linux查看进程占用网速和流量使用情况
### 有三个命令vnstat、iftop、nethogs（推荐）

都需要额外安装软件 使用yum或apt-get

### 一、vnstat使用，查看接口统计报告

vnstat -i eth0 -l #实时流量情况

还有其他命令使用--help查看

![2023-3-13_67812.png](/2023-3-13_67812.png)

ctrl+c结束后，会显示监控期间的流量统计结果

![2023-3-13_12221.png](/2023-3-13_12221.png)

### 二、iftop使用，检查带宽使用情况

iftop可以用来监控网卡的实时流量（可以指定网段）、反向解析IP、显示端口信息等

**命令用法：**

-i设定监测的网卡，如：# iftop -i eth1

-B 以bytes为单位显示流量(默认是bits)，如：# iftop -B

-n使host信息默认直接都显示IP，如：# iftop -n

-N使端口信息默认直接都显示端口号，如: # iftop -N

省略其他……

**交互命令：**

按n切换显示本机的IP或主机名;

按s切换是否显示本机的host信息;

按d切换是否显示远端目标主机的host信息;

按t切换显示格式为2行/1行/只显示发送流量/只显示接收流量;

按N切换显示端口号或端口服务名称;

按S切换是否显示本机的端口信息;

按D切换是否显示远端目标主机的端口信息;

按p切换是否显示端口信息;

省略其他……

**使用截图：**

![2023-3-13_69166.png](/2023-3-13_69166.png)

### 三、nethogs使用，按进程实时统计网络带宽利用率(推荐)

**命令用法：**

1、设置5秒钟刷新一次，通过-d来指定刷新频率：nethogs -d 5

2、监视eth0网络带宽 :nethogs eth0

3、同时监视eth0和eth1接口 : nethogs eth0 eth1
**交互命令：**
以下是NetHogs的一些交互命令(键盘快捷键)

- m : 修改单位
- r : 按流量排序
- s : 按发送流量排序
- q : 退出命令提示符

**使用截图：**

![2023-3-13_3543.png](/2023-3-13_3543.png)
