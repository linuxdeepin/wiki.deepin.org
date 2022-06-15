---
title: 用apt-fast加速软件包下载
description: 
published: true
date: 2022-06-15T01:55:39.748Z
tags: apt 软件包下载
editor: markdown
dateCreated: 2022-06-15T01:54:05.622Z
---

# 用apt-fast加速软件包下载
apt-fast是一款命令行工具，能够通过多线程下载软件包来提升下载应用和更新的速度。

项目链接：https://github.com/ilikenwf/apt-fast    本文主要参考安装说明中的Debian and derivates部分。

# 一 基础（新手请认真阅读）
有些用户可能平时主要通过应用商店和控制中心获取应用和更新，因此这里大致介绍一下安装软件包和更新的命令。（下面的命令中带方括号的部分需要根据实际情况自行替换）

## 1. 安装软件包

打开终端，输入以下命令并回车：

`
