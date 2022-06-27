---
title: 使用kvm安装和管理deepin
description: 
published: true
date: 2022-06-27T06:28:38.008Z
tags: kvm
editor: markdown
dateCreated: 2022-06-27T06:20:43.293Z
---

# 使用kvm安装和管理deepin 

## 1.概述  

我们知道：一台运行的主机，是由硬件跟软件同时存在的。硬件是基础，软件是灵魂。

所谓的硬件，就是：处理器，内存，磁盘，网卡，声卡，显卡 等

而作为灵魂的软件，其作为最基础系统软件的操作系统，英文简称OS，则是应用软件的运行平台。

操作系统，就是为使用者管理软硬件平台的。

本篇主要介绍如何使用VMware workstation来安装和管理deepin操作系统。

VMware workstation 提供主机运行的硬件基础，deepin提供主机运行的软件基础。

在虚拟机里面使用deepin的目的：

- 入门deepin。感受deepin的安装，配置，使用。
- 参与内测。体验最新的deepin的特性功能。
- 折腾deepin。进阶deepin的深层次配置和玩法。
- 开发测试。使用VMware workstation提供的快照功能，方便迭代与回滚。

这里不介绍如何下载和安装VMware workstation。

deepin最新版本下载[【点我】](https://www.deepin.org/zh/download/)，deepin历史版本下载[【点我】](https://cdimage.deepin.com/releases/)

本文假设你已经成功安装了VMware workstation 和下载了deepin最新版本。

> 本篇内容使用VMware workstation 16 pro。deepin20.4

## 2. 创建虚拟机

本节主要内容：使用VMware workstation创建一台配置自定义的虚拟机。简单来说，就是定制一台PC的硬件。

首先打开VMware workstation，点击【创建新的虚拟机】，如下图：