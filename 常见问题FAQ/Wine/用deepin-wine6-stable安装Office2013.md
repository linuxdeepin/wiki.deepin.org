---
title: 用deepin-wine6-stable安装Office2013
description: 
published: true
date: 2022-06-27T02:06:21.031Z
tags: office wine
editor: markdown
dateCreated: 2022-06-27T02:03:19.442Z
---

# wine使用教程

## 第6辑：在Deepin/UOS家庭版用deepin-wine6-stable安装Microsoft Office2013的方法

注：建立deepin-wine6-stable环境：新装的系统需要安装一款应用商店里使用deepin-wine6-stable运行的wine应用（如wine版微信、wine版QQ），并运行一下。（系统会自动建立deepin-wine6-stable环境）

## 一、下载Microsoft Office2013安装镜像并解压

以下教学所用Microsoft Office2013安装镜像（cn_office_professional_plus_2013_x86_x64_dvd_1149708.iso）从[MSDN网站下载](https://msdn.itellyou.cn/)。

![1](https://storage.deepin.org/thread/202206262305275758_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220626230515.png)

Microsoft Office2013安装镜像iso文件放在下载文件夹（~/Downloads）

右键解压

![2](https://storage.deepin.org/thread/202206262313334838_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220626201945.png)

## 二、新建容器

新建一个32位的windows7容器（容器名为Deepin-Office）

终端命令：

```
WINEARCH=win32 WINEPREFIX=~/.deepinwine/Deepin-Office deepin-wine6-stable winecfg
```

上述命令结构解析：

（1）字段1：WINEARCH=后面写win32，即表示新建一个32位的容器，如果写win64，即表示新建一个64位的容器。（目前wine对32位程序支持较好，若无特殊情况，建议新建32位容器）

（2）字段2：WINEPREFIX=是指定的容器路径

（3）字段3：deepin-wine6-stable是你使用的wine

（4）字段4：winecfg是调出wine设置

**注意：不同字段之间有一个空格（英文输入法），下同。**

![3](https://storage.deepin.org/thread/202206262307333460_%E6%88%AA%E5%9B%BE_deepin-terminal_20220626223500.png)

容器新建好后，就会在~/.deepinwine下多一个Deepin-Office的文件夹，这个文件夹就是模拟的windows系统环境，被称为wine容器

![4](https://storage.deepin.org/thread/202206262335277797_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220626231116.png)

## 三、安装Microsoft Office 2013

终端命令：

```
WINEPREFIX=~/.deepinwine/Deepin-Office deepin-wine6-stable ~/Downloads/cn_office_professional_plus_2013_x86_x64_dvd_1149708/setup.exe
```

上述命令结构解析：

（1）字段1：WINEPREFIX=是指定的容器路径

（2）字段2：deepin-wine6-stable是你使用的wine

（3）字段3：最后接你要运行的exe程序的路径

![5](https://storage.deepin.org/thread/202206262317377490_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220626223623.png)
