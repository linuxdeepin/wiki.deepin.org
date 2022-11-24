---
title: KVM详解（七）——KVM常用命令详解
description: 
published: true
date: 2022-11-24T13:07:56.771Z
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

![2022-11-24_3937.png](/2022-11-24_3937.png)

2、虚拟机启动与关闭执行命令：

`virsh start centos7-1`

可以打开该虚拟机。执行命令：

`virsh shutdown centos7-1`

可以关闭该虚拟机。执行命令：

`virsh destroy centos7-1`

可以强制性关闭该虚拟机，相当于强行断电，当虚拟机无法正常关闭时常使用这种方法。

3、虚拟机配置文件导出执行命令：

`virsh dumpxml centos7-2 > /tmp/centos7-2.xml`

可以将该虚拟机的配置文件导出到/tmp/目录下，并命名为centos7-2.xml，该命令执行结果如下：

