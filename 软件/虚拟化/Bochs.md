---
title: Bochs
description: 一款开源的x86模拟器和调试软件
published: true
date: 2022-08-04T08:31:45.627Z
tags: 
editor: markdown
dateCreated: 2022-08-04T08:31:45.627Z
---

## 简介
Bochs（发音：box）是以GNU宽通用公共许可证发放的开放源代码的x86、x86-64 IBM PC兼容机模拟器和调试工具，支持处理器（包括保护模式）、内存、硬盘、显示器、以太网、BIOS、IBM PC兼容机的常见硬件外设的仿真。
Bochs主要用于操作系统开发（当模拟操作系统崩溃，它不崩溃主机操作系统，所以可以调试仿真操作系统）和在主机操作系统运行其他来宾操作系统。它也可以用来运行不兼容的旧的软件（如电脑游戏）。
它的优点在于能够模拟跟主机不同的机种，例如在SPARC系统里模拟x86，但缺点是它的速度却慢得多。

## 安装
```
sudo apt install bochs
```

## 卸载
```
sudo apt remove --purge bochs
```

## 仓库地址
http://packages.deepin.com/deepin/pool/main/b/bochs/

## 相关链接
维基百科：https://zh.wikipedia.org/zh-cn/Bochs
Bochs 官方网站：https://bochs.sourceforge.io/