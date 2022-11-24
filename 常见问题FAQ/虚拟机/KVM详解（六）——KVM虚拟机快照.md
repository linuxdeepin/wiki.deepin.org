---
title: KVM详解（六）——KVM虚拟机快照
description: 
published: true
date: 2022-11-24T12:55:21.538Z
tags: 
editor: markdown
dateCreated: 2022-11-24T12:45:21.199Z
---

# KVM详解（六）——KVM虚拟机快照
## 一、KVM快照简介
KVM支持对虚拟机创建快照，但是前提是该虚拟机镜像不可以是raw格式，而应该是qcow2格式。但是，如果使用LVM，则可以对raw格式进行快照。这确实是一个很好的解决方案，但是其实现确实依靠LVM自身的快照功能实现的，而不是依靠KVM。有关LVM原理、作用以及实操请参考文章：LVM原理详解及实战。
KVM的虚拟机在创建快照后，就相当于对该虚拟机定位了一个状态，将来我们可以将该虚拟机恢复到该状态。下面，我们就来介绍一些KVM的快照创建、恢复和删除相关操作。

## 二、KVM快照创建
在KVM快照创建前，我们先保证虚拟机镜像为qcow2格式，如下所示：

![2022-11-24_99381.png](/2022-11-24_99381.png)

KVM的快照创建命令格式如下：

virsh snapshot-create 【虚拟机名称】

例如，我们要给虚拟机centos7-1.qcow2创建快照，则可以执行命令：

virsh snapshot-create centos7-1.qcow2

KVM虚拟机快照查看命令格式如下：

virsh snapshot-list 【虚拟机名称】

或者是：

qemu-img info 【虚拟机名称】

要查看我们创建的快照，可以执行命令：

virsh snapshot-list centos7-1.qcow2

上述命令执行结果如下：

![2022-11-24_76745.png](/2022-11-24_76745.png)


可以看出，我们成功的为KVM虚拟机创建了快照。但是，在这种创建方式中，快照的名称由KVM随机指定分配。如果我们想自己指定虚拟机的快照名称，则可以执行命令：

virsh snapshot-create-as 【虚拟机名】 【快照名】

命令示例如下：

virsh snapshot-create-as centos7-1.qcow2 snapshot-2

上述命令可以为centos7-1.qcow2创建名为snapshot-2的快照，该命令执行结果如下：

