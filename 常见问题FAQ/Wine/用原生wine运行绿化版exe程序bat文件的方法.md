---
title: 用原生wine运行绿化版exe程序bat文件的方法
description: 
published: true
date: 2022-06-20T09:08:38.886Z
tags: wine exe bat
editor: markdown
dateCreated: 2022-06-20T09:08:38.886Z
---

# 用原生wine运行绿化版exe程序（bat文件）的方法



之前开帖介绍过用原生wine运行便携式exe程序（绿色程序）的方法，有朋友想知道bat程序如何运行，现在再开一帖介绍一下方法。学习本帖内容前建议先看前四帖内容。

用原生wine运行绿化版exe程序（bat程序）的方法其实跟[wine使用教程4-用原生wine运行便携式exe程序（绿色软件）的方法](https://bbs.deepin.org/post/239212)是差不多的，只不过把wine命令换成wineconsole而已。

本帖案例软件名称假定为AppDemo.exe，bat文件名称假定为绿化处理.bat，容器名称假定为Deepin-AppDemo

方法如下：

## 1、下载绿化版exe程序包并解压

案例软件：AppDemo.exe

## 2、创建一个全新容器

创建容器的终端命令：

```
WINEARCH=win32或者wine64 WINEPREFIX=容器路径 wine winecfg
```

案例：

```
WINEARCH=win32 WINEPREFIX=~/.deepinwine/Deepin-AppDemo wine winecfg
```