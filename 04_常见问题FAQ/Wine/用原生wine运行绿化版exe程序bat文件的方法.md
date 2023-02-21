---
title: 5-用原生wine运行绿化版exe程序bat文件的方法
description: 
published: true
date: 2022-10-03T15:00:54.733Z
tags: wine
editor: markdown
dateCreated: 2022-06-20T09:08:38.886Z
---

之前开帖介绍过用原生wine运行便携式exe程序（绿色程序）的方法，有朋友想知道bat程序如何运行，现在再开一帖介绍一下方法。学习本帖内容前建议先看前四帖内容。

用原生wine运行绿化版exe程序（bat程序）的方法其实跟[wine使用教程4-用原生wine运行便携式exe程序（绿色软件）的方法](/zh/常见问题FAQ/Wine/用原生wine运行便携式exe程序绿色软件的方法)是差不多的，只不过把wine命令换成wineconsole而已。

本帖案例软件名称假定为AppDemo.exe，bat文件名称假定为绿化处理.bat，容器名称假定为Deepin-AppDemo

方法如下：

## 1、下载绿化版exe程序包并解压

案例软件：AppDemo.exe

## 2、创建一个全新容器

创建容器的终端命令：

```bash
WINEARCH=win32或者wine64 WINEPREFIX=容器路径 wine winecfg
```

案例：

```bash
WINEARCH=win32 WINEPREFIX=~/.deepinwine/Deepin-AppDemo wine winecfg
```

![1](https://storage.deepin.org/thread/202206182309357346_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220618230913.png)

上述命令结构解析：

1. `WINEARCH=`后面写win32，即表示新建一个32位的容器，如果写win64，即表示新建一个64位的容器。

2. `WINEPREFIX=`是指定的容器路径（此处Deepin-AppDemo就是准备新建的容器名称）。

3. `wine`即你指定的wine的执行程序，原生wine的执行程序就是wine。

（注：创建容器时如果提示安装mono，可点取消。mono模拟的.NET Framework，不是所有exe软件都需要这个。需要的时候你再手动安装即可）

![2](https://storage.deepin.org/thread/20220618180422989_%E6%88%AA%E5%9B%BE_winecfg.exe_20220618180331.png)

Windows版本要跟根据你所运行的exe软件要求选择。

特别说明：

1. 楼主已经把自己电脑里的原生wine升级到了最新版，即wine开发版7.11。
2. Deepin系统预装的原生wine版本是4.0，过于老旧，很多exe程序wine4.0都不能运行。因此建议小伙伴升级原生wine到最新版本。
3. **升级原生wine的方法见WineHQ官网：** https://wiki.winehq.org/Debian_zhcn

## 3、拷贝exe软件目录至容器c盘中

![3](https://storage.deepin.org/thread/202206182259184633_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220618225904.png)

## 4、运行绿化程序bat文件

运行bat文件的终端命令：

```bash
WINEPREFIX=容器路径 wineconsole "bat文件在容器c盘中的路径"
```

案例：

```bash
WINEPREFIX=~/.deepinwine/Deepin-AppDemo wineconsole "c:/Program Files/AppDemo/绿化处理.bat"
```

![4](https://storage.deepin.org/thread/202206182259184633_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220618225904.png)

![5](https://storage.deepin.org/thread/202206182311319243_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220618231105.png)

提醒：bat文件名不要带感叹号和其他符号，中文名或英文名即可。

## 5、测试运行exe主程序

终端命令：

```bash
WINEPREFIX=容器路径 wine "exe程序在容器C盘中的路径"
```

案例：

```bash
WINEPREFIX=~/.deepinwine/Deepin-AppDemo wine "c:/Program Files/Appdemo/AppDemo.exe"
```

## 6、安装字体

之前教程介绍过，解决字体问题有三种方法，我个人建议小白直接安装星火应用商店里的“Win字体”即可。

## 7、创建桌面图标

在桌面新建一个txt文件（如screentogif.txt），复制以下内容到txt文件里：

```ini
[Desktop Entry]
Categories=Application
Exec=sh -c 'WINEPREFIX=/home/$USER/.deepinwine/Deepin-AppDemo wine "c:/Program Files/AppDemo/AppDemo.exe"'
Icon=/usr/share/icons/Default/devices/64/media-optical.svg
MimeType=
Name=AppDemo
StartupNotify=true
Type=Application
X-Deepin-Vendor=user-custom
```

注：

Exec= ————sh -c 'WINEPREFIX=容器路径 wine "exe主程序路径在虚拟C盘里的路径"' 特别提醒:这里/home/$USER/不能写成~/

Icon= ————指图标路径，文件格式一般为png、svg、icon，图标大小64×64为宜，图标格式最好是svg。

Name= ————图标文件显示的名称

以上三项后面的内容可以根据你自己所安装的软件的实际情况更改。

上面的内容填写好后保存退出txt，右键重命名，把这个txt文件的后缀改为desktop（如appdemo.desktop）。这样就可以双击桌面图标运行该软件了。

## 8、其他配置

以上步骤为基础步骤。但除以上步骤外，有的exe软件还需要替换dll函数库，会用到winecfg、winetricks，有的软件需要安装mono或Gecko，有的软件还需要安装dxvk。这些都是比较高级的玩法，楼主能力有限，无法为大家讲解。
