---
title: 4-用原生wine运行便携式exe程序绿色软件的方法
description: 
published: true
date: 2022-10-03T15:00:13.771Z
tags: wine
editor: markdown
dateCreated: 2022-06-20T09:06:18.041Z
---

之前开帖介绍过用wine安装exe程序的方法，有朋友想知道用wine运行便携式exe程序（也称绿色软件）的方法，今天再开一帖介绍一下方法。学习本帖内容前建议先看前三帖内容。

用wine运行绿色软件的方法其实跟[wine使用教程1-用原生wine安装/运行exe程序的方法](/zh/常见问题FAQ/Wine/wine如何安装运行exe程序)是差不多的，只不过少了安装程序的步骤而已。

方法如下：

## 1、下载免安装版exe软件并解压

案例软件：[32位ScreenToGif（下载时选择x86，系统要求windows8以上）](https://www.screentogif.com/?l=zh_cn)
友情提醒：大家下载exe软件尽量从它们的官网下载，以免下载到带毒的软件。

![1](https://storage.deepin.org/thread/202206181731197107_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220618170936.png)


2、创建一个全新容器

![2](https://storage.deepin.org/thread/202206181800311878_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220618165912.png)

创建容器的终端命令：

```bash
WINEARCH=win32或者wine64 WINEPREFIX=容器路径 wine winecfg
```

案例：

```bash
WINEARCH=win32 WINEPREFIX=~/.deepinwine/Wine-ScreenToGif wine winecfg
```

上述命令结构解析：

（1）WINEARCH=后面写win32，即表示新建一个32位的容器，如果写win64，即表示新建一个64位的容器。

（2）WINEPREFIX=是指定的容器路径（此处Wine-ScreenToGif就是准备新建的容器名称）。

（3）wine即你指定的wine的执行程序，原生wine的执行程序就是wine。（如果要用deepin-wine，则这里可以换成`deepin-wine5`或者`deepin-wine6-stable`，或者`~/.deepinwine/deepin-wine5-stable/bin/wine`）

（注：创建容器时如果提示安装mono，可点取消。mono模拟的.NET Framework，不是所有exe软件都需要这个。需要的时候你再手动安装即可）

![3](https://storage.deepin.org/thread/20220618180422989_%E6%88%AA%E5%9B%BE_winecfg.exe_20220618180331.png)

案例软件ScreenToGif要求系统为windows8以上，所以在winecfg窗口里windows版本设定为windows10。

特别说明：（1）经测试，案例软件ScreenToGif.exe不能用deepin-wine5和deepin-wine6-stable运行，可以用原生wine7.10运行（楼主已将原生wine版本升级为开发版wine-devel 7.10），因此本帖用原生wine7.10做例子。（2）Deepin系统预装的原生wine版本是4.0，过于老旧，喜欢尝鲜的小伙伴建议升级原生wine到最新版本。（3）**升级原生wine的方法见WineHQ官网：** https://wiki.winehq.org/Debian_zhcn

## 3、拷贝exe软件目录至容器c盘中

容器路径：~/.deepinwine/Deepin-ScreenToGif

案例软件解压后只有一个主程序ScreenToGif.exe，我在容器的drive_c文件夹（即模拟的c盘）Program Files文件夹下新建一个ScreenToGif文件夹，用于放置ScreenToGif.exe

![4](https://storage.deepin.org/thread/202206181743316082_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220618164537.png)

## 4、测试运行

运行exe程序的终端命令：

```bash
WINEPREFIX=容器路径 wine "exe主程序在容器c盘中的路径"
```

案例：

```bash
WINEPREFIX=~/.deepinwine/Wine-ScreenToGif wine "c:/Program Files/ScreenToGif/ScreenToGif.exe"
```

![5](https://storage.deepin.org/thread/20220618180001856_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220618170608.png)

## 5、安装字体

之前教程介绍过，解决字体问题有三种方法，我个人建议小白直接安装星火应用商店里的“Win字体”即可。

## 6、创建桌面图标

在桌面新建一个txt文件（如screentogif.txt），复制以下内容到txt文件里：

```ini
[Desktop Entry]
Categories=Application
Exec=sh -c 'WINEPREFIX=/home/$USER/.deepinwine/Wine-ScreenToGif wine "c:/Program Files/ScreenToGif/ScreenToGif.exe"'
Icon=/usr/share/icons/Default/devices/64/media-optical.svg
MimeType=
Name=ScreenToGif
StartupNotify=true
Type=Application
X-Deepin-Vendor=user-custom
```

注：

Exec= ————sh -c 'WINEPREFIX=容器路径 wine "c:/exe主程序路径在虚拟C盘里的路径"' 特别提醒:这里/home/$USER/不能写成~/

Icon= ————指图标路径，文件格式一般为png、svg、icon，图标大小64×64为宜，图标格式最好是svg。

Name= ————图标文件显示的名称

以上三项后面的内容可以根据你自己所安装的软件的实际情况更改。

上面的内容填写好后保存退出txt，右键重命名，把这个txt文件的后缀改为desktop（如screentogif.desktop）。这样就可以双击桌面图标运行该软件了。

![6](https://storage.deepin.org/thread/202206181749343495_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220618172315.png)

## 5、其他配置

以上步骤为基础步骤。但除以上步骤外，有的exe软件还需要替换dll函数库，会用到winecfg、winetricks，有的软件需要安装mono或Gecko，有的软件还需要安装dxvk。这些都是比较高级的玩法，楼主能力有限，无法为大家讲解。

------

经测试，案例软件无法在wine7.10里完美运行，使用过程会崩溃闪退。本帖仅做讲解运行绿色软件的基本方法，并不深究软件崩溃的原因。

