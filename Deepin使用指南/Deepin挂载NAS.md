---
title: Deepin挂载NAS
description: 深度系统下挂载NAS盘的方法
published: true
date: 2022-06-17T06:01:06.496Z
tags: 
editor: markdown
dateCreated: 2022-06-16T09:49:53.520Z
---

# Deepin挂载NAS

## 使用NFS
（待补充）

## 使用CIFS
本文参考：[https://www.raobee.com/archives/308/](https://www.raobee.com/archives/308/)


### 一、安装前置所需软件

`nfs-common`，`cifs-utils`是必须的前置软件（在Ubuntu发行版下）

```shell
sudo apt install -y nfs-common cifs-utils
```

### 二、编辑自动挂载文件并挂载

编辑`/etc/fstab`文件

``` shell
sudo deepin-deitor /etc/fstab
```

添加如下内容:
//your_nas_ip/dir /mnt/NAS/dir cifs rw,dir_mode=0777,file_mode=0777,vers=2.0,username=yourusername,password=yourpassword 0 0

``` shell
#NAS auto mount
//your_nas_ip/dir /mnt/NAS/dir cifs rw,dir_mode=0777,file_mode=0777,vers=2.0,username=yourusername,password=yourpassword 0 0
```

其中，`your_nas_ip`代表你的NAS的访问地址， `dir`代表NAS下的分享挂载点，`/mnt/mountdir`代表本设备要挂载到的路径，`yourusername`为访问的用户名，`yourpassword`为访问用户的密码，如果设置是匿名访问则不需要username与password两项设置并改为`guest`

*附：[关于`/etc/fstab`文件的语法详解](https://blog.csdn.net/youmatterhsp/article/details/83933158)*

编辑并保存完毕后，执行

``` shell
sudo mount -a
```

如果没有任何报错与提示信息，则成功挂载~

---------

### Deepin20下实例

我想挂载整个NAS，于是我在`/mnt` 下建立了 `NAS`文件夹

赋予其权限

```shell
sudo chmod -R 777 /mnt/NAS
```

在`/etc/fstab`文件中添加自动挂载内容

> \# NAS auto mount
> //192.168.104.44/homes /mnt/NAS/homes cifs rw,dir_mode=0777,file_mode=0777,vers=2.0,username=GBwater,password=我是密码 0 0

最后执行挂载指令

``` shell
sudo mount -a
```

最后在文件管理器上右键`/mnt/NAS/homes/GBwater`，选择`添加书签`，即可将其添加到文件管理器的侧边栏，方便访问

