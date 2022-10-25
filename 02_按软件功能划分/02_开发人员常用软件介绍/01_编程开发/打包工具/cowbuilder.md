---
title: 使用 cowbuilder chroot 来构建包
description: 
published: true
date: 2022-07-18T05:59:47.799Z
tags: 
editor: markdown
dateCreated: 2022-07-18T05:15:15.715Z
---

# 用法
### 初始化
###### 创建一个基础镜像
```
sudo cowbuilder --create
```
新创建的镜像将在  `/var/cache/pbuilder/base.cow/` 下

### 常用操作
###### 更新基础镜像
```
sudo cowbuilder --update
```
###### 构建软件包
```
sudo cowbuilder --build somepackage.dsc
```
### tips
###### 一次为多个发行版构建包
cowbuilder 可以非常方便的一次为多个发行版构建你的软件包，如果你有一台 amd64 的机器，你可以很简单的为 stable sid 发行版构建 i386 和 amd64 架构的软件包
首先你要创建一个放置基础镜像的目录，比如 `/var/cache/pbuilder/$DIST-$ARCH/base.cow` 
```
mkdir /var/cache/pbuilder/buster-i386
```
然后创建基础镜像
```
sudo cowbuilder --create --basepath /var/cache/pbuilder/buster-i386/base.cow --distribution buster --debootstrapopts --arch --debootstrapopts i386
```
在家目录创建一个 .pbuilderrc 文件，可以从 [Ubuntu pbuilder howto](https://wiki.ubuntu.com/PbuilderHowto) 获取一个，然后在 Multiple pbuilders 部分, 注释以下行
```
BASEPATH="/var/cache/pbuilder/$NAME/base.cow/
```
让你的 cowbuilder chroot 保持最新
```
sudo HOME=$HOME DIST=sid cowbuilder --update
```
这里的 HOME 环境变量是必要的，因为 sudo 出于安全原因会剥离环境变量， 否则 cowbuilder 将找不到刚刚创建的 .pbuilderrc

接下来就可以开始构建软件包了，比如 buster 的 nano 包。
```
dget -x http://ftp.de.debian.org/debian/pool/main/n/nano/nano_2.7.4-1.dsc
# 比如给 buster i386 架构打包
sudo DIST=buster ARCH=i386 cowbuilder --build nano_2.7.4-1.dsc
```
构建的结果将在 `/var/cache/pbuilder/buster-i386/result` 目录下

###### 使用 eatmydata 加速
```
DIST=sid cowbuilder --login --save
apt-get install eatmydata
```
如果你的 pbuiler版本 >= 0.225, 只需要在你的 .pbuilderrc 中加入:
```
EXTRAPACKAGES=eatmydata
EATMYDATA=yes
```
如果你的 eatmydata 版本 <=26-2.1, 在 /etc/pbuilderrc 加入 :
```
export LD_PRELOAD=${LD_PRELOAD:+$LDPRELOAD:}/usr/lib/libeatmydata/libeatmydata.so
```
如果 eatmydata 版本 >=82-2 则加入:
```
export LD_PRELOAD=${LD_PRELOAD:+$LDPRELOAD:}libeatmydata.so
```


