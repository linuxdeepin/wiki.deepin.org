---
title: Wine介绍
description: 
published: true
date: 2022-10-21T08:05:45.772Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:44:49.163Z
---

## 简介

Wine （“Wine Is Not an Emulator” 的首字母缩写）是一个能够在多种 POSIX-compliant 操作系统（诸如 Linux，macOS 及 BSD 等）上运行 Windows 应用的兼容层。 Wine 不是像虚拟机或者模拟器一样模仿内部的 Windows 逻辑，而是將 Windows API 调用翻译成为动态的 POSIX 调用，免除了性能和其他一些行为的内存占用，让你能够干净地集合 Windows 应用到你的桌面。

## 安装

`sudo apt-get install wine`

## 卸载

`sudo apt-get remove wine`

## 仓库地址

[http://packages.deepin.com/deepin/pool/main/w/wine/](http://packages.deepin.com/deepin/pool/main/w/wine/)

## 常见问题

1. 百度盘下载文件的位置在哪里?
/home/用户名/.deepinwine/Deepin-BaiduNetDisk/drive_c/BaiduNetdiskDownload

2. 如何判断软件是否需要wine来启动?
一般软件在介绍的时候都会说明的, 有时候一款软件有linux原生和wine两个版本, 要注意区分
大多数windows平台的软件需要安装到linux平台时都需要借助wine