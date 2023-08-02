---
title: vmkfstools命令
description: 
published: true
date: 2023-08-02T06:30:17.023Z
tags: 
editor: markdown
dateCreated: 2023-08-02T06:30:17.023Z
---

# vmkfstools命令 – 虚拟磁盘工具


vmkfstools被比做虚拟磁盘中的瑞士军刀，可用于复制、转换、重命名、输入、输出和调整虚拟磁盘文件的大小。

语法格式：
```
vmkfstools [参数]
```
## 常用参数：
```
-i	原vmdk磁盘名
-d	目标磁盘的格式
```
## 参考实例

更改虚拟磁盘(vmdk)大小：
```
[root@linuxcool ~]# vmkfstools -X 40g converter-one.vmdk
```
精简置备转换至厚置备置零：
```
[root@linuxcool ~]# vmkfstools --inflatedisk /vmfs/volumes/DatastoreName/VMName/VMName.vmdk
```
厚置备延迟置零转换至厚置备置零：
``
[root@linuxcool ~]# vmkfstools --eagerzero /vmfs/volumes/DatastoreName/VMName/VMName.vmdk
```
登陆ESXI 调整虚拟磁盘大小：
```
[root@linuxcool ~]# vmkfstools -X 40g converter-two.vmdk
```
创建虚拟磁盘文件：
```
[root@linuxcool ~]# vmkfstools -C --createfs vmfs3
```
