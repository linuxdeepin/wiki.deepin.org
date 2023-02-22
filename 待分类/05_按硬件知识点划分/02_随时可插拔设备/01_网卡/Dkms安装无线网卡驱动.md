---
title: Dkms安装无线网卡驱动
description: 
published: true
date: 2022-10-19T07:36:07.969Z
tags: 
editor: markdown
dateCreated: 2022-06-28T08:27:10.566Z
---

# 背景
>  Linux模块和内核是有依赖关系的，如果遇到因为发行版更新造成的内核版本的变动，之前编译的模块是无法继续使用的，我们只能手动再编译一遍。
> 这样重复的操作有些繁琐且是反生产力的，而对于没有内核编程经验的使用者来说可能会造成一些困扰，使用者搞不清楚为什么更新系统之后，原来用的好好的驱动程序突然就不能用了。
> 这就是DKMS项目的意义所在。DKMS全称是Dynamic Kernel Module Support，它可以帮我们维护内核外的这些驱动程序，在内核版本变动之后可以自动重新生成新的模块。
 
# 步骤
以RTL8812BU软件包为例。

# 准备工作
```
1.解压压缩包，fixed-ok文件夹里的是arm架构的，RTL8812BU-master里支持x86架构。

2.找到对应的文件夹，对应下一层的dkms.conf文件里的前两行，修改对应的文件名。

PACKAGE_NAME="rtl88x2bu"
PACKAGE_VERSION="5.6.1"
将文件名改为rtl88x2bu-5.6.1

3.把文件夹复制到/usr/src
$sudo cp -r rtl88x2bu-5.6.1 /usr/src

4.安装dkms
$sudo apt-get install dkms
```
# 使用dkms
```
1.添加模块
执行“sudo dkms add rtl88x2bu/5.6.1”来添加rtl88x2bu-5.6.1

2.查看添加状态
dkms status
rtl88x2bu, 5.6.1: added  此时模块处于added状态

3.编译模块
执行：sudo dkms build rtl8812xu/5.6.1
看到build completed即build成功：
Kernel preparation unnecessary for this kernel.  Skipping...
Building module:
cleaning build area...
make -j16 KERNELRELEASE=4.19.0-amd64-desktop KVER=4.19.0-amd64-desktop src=/usr/src/rtl88x2bu-5.6.1.......
cleaning build area...

DKMS: build completed.   模块处于built状态

4.安装模块
最后执行：sudo dkms install rtl8812xu/5.6.1 
来安装88x2bu.ko：
DKMS: install completed.  模块处于installed状态，即已安装
```

# 基于DKMS制作驱动程序的DEB安装包
```
1.简介
作为Linux开发者，有时候用户会要求我们提供驱动的DEB安装包，基于DKMS来制作DEB安装包是一个很好的选择。对开发者来说这样的DEB包制作起来比较简单，对于用户来说使用起来也省去许多烦恼。

2.安装所需工具
sudo apt-get install dh-make libdigest-md5-file-perl

3.制作deb包
模块必须处于built状态！
sudo dkms mkdeb rtl88x2bu/5.6.1
```

# 应用
如果电脑本来就装有相应网卡，或插入外部网卡。
即可在“设备管理器“里的“网络适配器”中看到新加入的接口信息。从而实现无线上网。