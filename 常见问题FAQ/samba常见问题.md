---
title: samba常见问题
description: 
published: true
date: 2022-06-20T07:43:17.505Z
tags: 
editor: markdown
dateCreated: 2022-06-20T07:42:57.278Z
---

# samba 常见问题
## 网络邻居中不能查看设备
初步分析为访问网络领居需要samba低版本的协议，现在默认是高版本。可以手动降低协议，但不建议；
1.降低后不能访问windows的共享，协议不匹配；
2.低版本协议有严重的安全漏洞.试下在`/etc/samba/smb.conf`配置文件中的加入一行代码，如下：
`【workgroup = WORKGROUP】`
`client max protocol = NT1`

## 挂载samba后图标显示齿轮
可能的原因：
1.gvfs包没有安全完全。特别是`gvfs-fuse`、`gvfs-backends`；
2.系统没有 fuse 驱动。通过`lsmod | grep "fuse"`，命令可以查看是否安装驱动；

## samba 无法记住密码
`/efc/profile`文件中包含删除`~/.local/share/keyrings/`的脚本，这样也是无法记住密码的；

## samba 共享中文件权限问题
1.首先查看`samba`服务器中`/etc/smb.conf`文件中的配置是否是如下:
`create mode = 0766`
`force create mode = 0766`
`directory mode = 0777`
`force directory mode = 0777`

2.用户默认创建的文件夹带有`acl`控制权限,匿名访问对应当前用户是`other`的,就没有读写权限了,通过终端创建文件夹也是带了`acl`权限的;