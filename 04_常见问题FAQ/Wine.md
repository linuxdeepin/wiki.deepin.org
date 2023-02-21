---
title: Wine
description: 
published: true
date: 2022-06-16T02:20:17.255Z
tags: wine
editor: markdown
dateCreated: 2022-06-16T02:20:15.108Z
---

# Wine

[跳到导航](http://old.deepin.wiki/index.php?title=Wine#mw-head)[跳到搜索](http://old.deepin.wiki/index.php?title=Wine#searchInput)

Wine（“Wine Is Not an Emulator” 的首字母缩写）是一个能够在多种 POSIX-compliant 操作系统（诸如 Linux，macOS 及 BSD 等）上运行 Windows 应用的兼容层。Wine 不是像虚拟机或者模拟器一样模仿内部的 Windows 逻辑，而是将 Windows API 调用翻译成为动态的 POSIX 调用，免除了性能和其他一些行为的内存占用，让你能够干净地集合 Windows 应用到你的桌面。

## 目录



- 1配置
  - [1.1容器](http://old.deepin.wiki/index.php?title=Wine#.E5.AE.B9.E5.99.A8)
  - [1.2WINEPREFIX](http://old.deepin.wiki/index.php?title=Wine#WINEPREFIX)
- 2提示与技巧
  - [2.1Deepin-Wine适配知识库](http://old.deepin.wiki/index.php?title=Wine#Deepin-Wine.E9.80.82.E9.85.8D.E7.9F.A5.E8.AF.86.E5.BA.93)
  - [2.2为 Wine 程序添加快捷键](http://old.deepin.wiki/index.php?title=Wine#.E4.B8.BA_Wine_.E7.A8.8B.E5.BA.8F.E6.B7.BB.E5.8A.A0.E5.BF.AB.E6.8D.B7.E9.94.AE)
  - [2.3*.desktop 文件创建](http://old.deepin.wiki/index.php?title=Wine#.2A.desktop_.E6.96.87.E4.BB.B6.E5.88.9B.E5.BB.BA)
- 3相关工具
  - [3.1Winetricks](http://old.deepin.wiki/index.php?title=Wine#Winetricks)
  - 3.2wine 运行器
    - [3.2.1问题解决](http://old.deepin.wiki/index.php?title=Wine#.E9.97.AE.E9.A2.98.E8.A7.A3.E5.86.B3)
  - [3.3Vek](http://old.deepin.wiki/index.php?title=Wine#Vek)
- [4请参阅](http://old.deepin.wiki/index.php?title=Wine#.E8.AF.B7.E5.8F.82.E9.98.85)

## 配置

### 容器

默认情况下，Wine 将使用位于 `~/.wine/` 的默认容器。

另见：[深度论坛：[新手教程\] Deepin用wine安装的Windows软件目录在哪?](https://bbs.deepin.org/zh/post/227183)

### WINEPREFIX

设置 `WINEPREFIX` 变量可以改变使用的 Wine 容器的位置。如 `env WINEPREFIX=~/.wine/wineqq/ wine "C:\\pcqq2021.exe"` 将在位于 `~/.wine/wineqq/` 目录下的容器中启动 `C:\pcqq2021.exe` 程序。

## 提示与技巧

### Deepin-Wine适配知识库

Deepin-Wine适配知识库：https://docs.qq.com/mind/DWFBpbmpjd0RtV2Z0

### 为 Wine 程序添加快捷键

详见：[[新手教程\] wine程序添加快捷键（通用）](https://bbs.deepin.org/zh/post/219312)

### *.desktop 文件创建

参见：[深度论坛：[新手教程\] exe文件怎么打开运行](https://bbs.deepin.org/post/231651)

## 相关工具

### Winetricks

Winetricks 是一个帮助用户调整 Wine 设置、下载安装 Wine 所需的 Windows 组件的应用程序。执行命令安装：

```
sudo apt install winetricks
```

仓库地址为：https://github.com/Winetricks/winetricks

### wine 运行器

wine 运行器是 gfdgd xi 使用 Python3 的 tkinter 制作的 wine 运行工具。可以在不打开终端的情况下选取 Wine 版本、选择容器、运行应用的工具。是对如下命令的简易封装：

```
WINEPREFIX=容器路径 wine（wine的路径） 可执行文件路径
```

仓库地址为：https://gitee.com/gfdgd-xi/deep-wine-runner

#### 问题解决

如果终端执行遇到

```
Traceback (most recent call last):
  File "/usr/bin/deepin-wine-runner", line 19, in <module>
    import ttkthemes
ModuleNotFoundError: No module named 'ttkthemes'
```

执行：

```
sudo apt install python3-pip
sudo pip3 install ttkthemes
```

### Vek

参见：[Vek](http://old.deepin.wiki/index.php?title=Vek)

## 请参阅

- https://wiki.archlinux.org/title/Wine
- https://wiki.winehq.org/Main_Page