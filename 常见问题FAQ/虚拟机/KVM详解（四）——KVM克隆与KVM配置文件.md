---
title: KVM详解（四）——KVM克隆与KVM配置文件
description: 
published: true
date: 2022-11-23T09:04:25.109Z
tags: 
editor: markdown
dateCreated: 2022-11-23T08:17:37.694Z
---

# KVM详解（四）——KVM克隆与KVM配置文件
## 一、KVM克隆
KVM支持虚拟机克隆，所谓虚拟机克隆，就是由一台指定的虚拟机生成另一台与其完全相同的虚拟机。
KVM虚拟机克隆命令格式为：

`virt-clone -o 【原虚拟机】 -n 【新虚拟机】 -f 【新虚拟机镜像名（含路径）`

例如，我们要把之前的centos7-1虚拟机进行克隆，如下（centos7-1.qcow2）所示：

那么就可以执行命令：

 virt-clone -o centos7-1 -n centos7-2 -f /var/lib/libvirt/images/centos7-2.img

注意，-o参数后面跟的是原虚拟机的名称，而不要跟镜像格式，即需要把后面的后缀.qcow2去掉，-n跟的是新的虚拟机名称，-f跟的是新的虚拟机的路径加上文件名。
该命令执行过程如下：

执行结果如下：



