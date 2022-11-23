---
title: KVM详解（三）——KVM创建虚拟机
description: 
published: true
date: 2022-11-23T05:48:42.996Z
tags: 
editor: markdown
dateCreated: 2022-11-23T05:47:41.817Z
---

# KVM详解（三）——KVM创建虚拟机
一、安装准备
在前文KVM详解（二）——KVM安装部署中，我们安装了KVM。今天，我们就来创建一个KVM的虚拟机。
要创建KVM的虚拟机，我们首先来为KVM虚拟机挂载一块磁盘。在前文中，我们特意加装了一块30G的磁盘，我们就用该磁盘作为KVM虚拟机的存储设备。执行命令：

```
fsdisk /dev/sdb
mkfs.xfs /dev/sdb1
mount /dev/sdb1 /var/lib/libvirt/images
```

这些命令执行结果如下：