---
title: rfkill命令
description: 
published: true
date: 2023-08-02T06:15:22.571Z
tags: 
editor: markdown
dateCreated: 2023-08-02T06:14:59.596Z
---

# rfkill命令 – 管理蓝牙和WIFI设备
rfkill命令来自于英文词组“radio frequency kill”的缩写，其功能是管理系统中的蓝牙和WIFI设备。rfkill命令是一个内核级别的管理工具，可以打开或关闭系统中的蓝牙和WIFI功能。

语法格式：
```
rfkill [参数] 设备名
```
常用参数：
```
list	列出可用设备
block	关闭设备
unblock	打开设备
```
参考实例

显示系统中已有的WIFI和蓝牙设备信息：
```
[root@linuxcool ~]# rfkill list
0: phy0: Wireless LAN
Soft blocked: no
Hard blocked: no
2: hci0: Bluetooth
Soft blocked: yes
Hard blocked: no
```
关闭指定编码的设备：
```
[root@linuxcool ~]# rfkill block 0
```
打开指定编码的设备：
```
[root@linuxcool ~]# rfkill unblock 0
```