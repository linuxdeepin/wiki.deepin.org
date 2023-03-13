---
title: Linux查看进程占用网速和流量使用情况
description: 
published: true
date: 2023-03-13T01:52:33.930Z
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



ctrl+c结束后，会显示监控期间的流量统计结果
