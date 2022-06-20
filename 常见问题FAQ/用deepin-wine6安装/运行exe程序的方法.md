---
title: 用deepin-wine6安装/运行exe程序的方法
description: 
published: true
date: 2022-06-20T08:34:34.911Z
tags: wine6 exe
editor: markdown
dateCreated: 2022-06-20T08:34:34.911Z
---

# wine使用教程  

## 第2辑：在Deepin/UOS家庭版用deepin-wine6-stable安装/运行exe程序的方法
注：

1、建立deepin-wine6-stable环境：新装的系统需要安装一款应用商店里使用deepin-wine6-stable运行的wine应用（如wine版微信、wine版QQ），并运行一下。（系统会自动建立deepin-wine6-stable环境）

2、以下教学以32位7-Zip的安装程序7z2107.exe（版本21.7.0.0）为案例软件，该exe程序我放在系统下载（Downloads）文件夹，即/home/$USER/Downloads/7z2107.exe，或者写作~/Downloads/7z2107.exe

多数情况下，/home/$USER与~可以相互替代。

一、安装exe程序
安装程序到一个新建的容器

终端命令：

WINEARCH=win32或者wine64 WINEPREFIX=容器路径 deepin-wine6-stable 需安装/运行的exe软件的路径
案例：

WINEARCH=win32 WINEPREFIX=~/.deepinwine/Wine-7zip deepin-wine6-stable ~/Downloads/7z2107.exe
（注：如果提示安装mono，可点取消。mono模拟的.NET Framework，不是所有exe软件都需要这个。需要的时候你再手动安装即可）

上述命令结构解析：

（1）WINEARCH=后面写win32，即表示新建一个32位的容器，如果写win64，即表示新建一个64位的容器。

（2）WINEPREFIX=是指定的容器路径（此处Wine-7zip就是容器名称，如果容器Wine-7zip不存在，默认会新建这个容器）。

（3）如果要用deepin-wine5，中间可以写成deepin-wine5；如果要用deepin-wine5-stable，中间可以写成~/.deepinwine/deepin-wine5-stable/bin/wine

（4）最后接的是你要安装的exe安装程序的所在路径。

二、运行已安装到容器的exe主程序
终端命令：

WINEPREFIX=容器路径 deepin-wine6-stable “c:/exe主程序在虚拟C盘（即drive_c）里的路径”
案例：

WINEPREFIX=~/.deepinwine/Wine-7zip deepin-wine6-stable "c:/Program Files/7-Zip/7zFM.exe"
三、winecfg设置
可设置windows版本、替换dll函数、窗口修饰、显示分辨率等。

1、修改windows版本

默认的windows版本是windows7，有的exe安装时提示系统版本太低的话，就需要利用winecfg修改为windows10。有的exe软件在windows xp表现更好，就需要用winecfg修改为windows xp。

打开winecfg的终端命令：

WINEPREFIX=容器路径 /opt/deepin-wine6-stable/bin/winecfg
案例：

WINEPREFIX=~/.deepinwine/Wine-7zip /opt/deepin-wine6-stable/bin/winecfg
上述命令结构解析：

WINEPREFIX=是指定的容器路径，后面打一个空格，然后输入winecfg所在路径 /opt/deepin-wine6-stable/bin/winecfg

案例软件7-Zip无需修改windows版本即可正常运行。

2、函数顶替

有的exe软件，无需新增函数顶替。

有的exe软件，新增以下几个函数顶替基本上就能正常运行了：atl100、mlang、msls31、riched20、usp10

有的exe软件还需要添加msvcp60、riched32等函数

案例软件7-Zip无需新增顶替函数即可正常运行。

四、字体设置
由于linux系统默认是没有windows常用字体（如Arial、微软雅黑、宋体），所以用wine安装的exe软件大概率会出现字体乱码、字体呈现方块、字体显示不出来等问题。此时，需要设置字体，方法有三种：

方法1：直接安装“Win字体”应用（可以与方法2并用）
到星火应用商店里下载安装“Win字体”。安装好后，再调出winecfg（方法如前述），字体选项下勾选“允许加载系统字体”，建议顺便把“允许加载Windows Fonts目录下的字体”也勾上。

方法2：复制字体到虚拟C盘的字体文件夹（可以与方法1并用）
将exe软件需要用的字体文件（如宋体的文件为simsun.ttf）复制粘贴到容器的字体文件夹，路径通常为：

~/.deepinwine/容器名称/drive_c/windows/Fonts

案例软件Fonts文件夹路径：~/.deepinwine/Wine-7zip/drive_c/windows/Fonts

调出winecfg（方法如前述），字体选项下勾选“允许加载Windows Fonts目录下的字体”，建议顺便把“允许加载系统字体”也勾上。

方法3：修改字体注册表（尽量不要与方法1和方法2并用）
不安装字体，而是在注册表里把需要的字体替换为你系统已有字体。

打开注册表的命令：

WINEPREFIX=容器路径 /opt/deepin-wine6-stable/bin/regedit
案例：

WINEPREFIX=~/.deepinwine/Wine-7zip /opt/deepin-wine6-stable/bin/regedit

WINEPREFIX=~/.deepinwine/Wine-7zip /opt/deepin-wine6-stable/bin/regedit
找到HKEY_CURRENT_USER\Software\Wine\Fonts\Replacements，如果没有Replacements，新建一个项，项名为Replacements。

然后在Replacements下添加字符串值。如要把宋体（SimSun）替换为思源宋体（Noto Serif CJK SC），则字符串名称为SimSun，值为Noto Serif CJK SC