---
title: KVM详解（七）——KVM常用命令详解
description: 
published: true
date: 2022-11-24T13:03:36.476Z
tags: 
editor: markdown
dateCreated: 2022-11-24T13:03:36.476Z
---

# KVM详解（七）——KVM常用命令详解
本文以centos7-1、centos7-2虚拟机为例，向大家介绍常见的KVM命令如下：
1、虚拟机查看
执行命令：

`virsh list`

可以查看已经打开的虚拟机。
执行命令：

`virsh list --all`

可以查看所有虚拟机（包括已经打开的和未打开的）
执行命令：

`virsh version`

可以查看virsh的版本上述命令执行结果如下：
