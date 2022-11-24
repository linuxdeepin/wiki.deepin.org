---
title: KVM详解（七）——KVM常用命令详解
description: 
published: true
date: 2022-11-24T13:13:39.938Z
tags: 
editor: markdown
dateCreated: 2022-11-24T13:03:36.476Z
---

# KVM详解（七）——KVM常用命令详解
本文以centos7-1、centos7-2虚拟机为例，向大家介绍常见的KVM命令如下：
## 1、虚拟机查看
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

## 2、虚拟机启动与关闭执行命令：

`virsh start centos7-1`

可以打开该虚拟机。执行命令：

`virsh shutdown centos7-1`

可以关闭该虚拟机。执行命令：

`virsh destroy centos7-1`

可以强制性关闭该虚拟机，相当于强行断电，当虚拟机无法正常关闭时常使用这种方法。

## 3、虚拟机配置文件导出执行命令：

`virsh dumpxml centos7-2 > /tmp/centos7-2.xml`

可以将该虚拟机的配置文件导出到/tmp/目录下，并命名为centos7-2.xml，该命令执行结果如下：

![2022-11-24_40506.png](/2022-11-24_40506.png)

## 4、虚拟机挂起与恢复执行命令：

`virsh suspend centos7-1`

可以挂起该虚拟机。执行命令：

`virsh resume centos7-1`

可以恢复该虚拟机。

## 5、虚拟机会话链接执行命令：

`virsh console centos7-1`

可以与centos7-1打开一个控制台窗口终端。

## 6、虚拟机克隆

有关虚拟机克隆等相关理论和命令，请参考以下文章：KVM详解（四）——KVM克隆与KVM配置文件

## 7、虚拟机撤销与恢复执行命令：

`virsh undefine centos7-2`

可以将该虚拟机从虚拟机管理器中移除，该虚拟机的配置文件会随之删除，但是该虚拟机磁盘文件并没有删除，因此该虚拟机还可以恢复，但是前提是该虚拟机的配置文件已经进行了导出和备份。
执行命令：

`virsh define /tmp/centos7-2.xml`

即可对删除的虚拟机进行恢复。

## 8、虚拟机自启动与自启动撤销执行命令：

`virsh autostart centos7-1`

可以使得该虚拟机在开始时自启动执行命令：

`virsh autostart --disable centos7-1`

可以取消掉该虚拟机开机时自启动的设置。

## 9、虚拟机镜像格式转换

有关虚拟机镜像格式的原理及转换命令，请参考以下文章：KVM详解（五）——KVM虚拟机镜像格式

## 10、虚拟机快照

有关虚拟机快照的创建、查看、恢复和删除等操作，请参考以下文章：
KVM详解（六）——KVM虚拟机快照