---
title: Krita
description: 
published: true
date: 2022-05-07T07:47:22.601Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:37:10.039Z
---

## 简介

Krita 是一款自由开源的数字绘画软件，使用 GPL 许可证发布。它主要针对手绘用途设计，具备高度可定制的笔刷系统和完善的图层功能，可通过透明度和变形蒙版来实现非破坏性编辑。它能够绘制位图图像、矢量图形和制作动画，并具备完善的色彩管理功能，能够进行 HDR 图像的编辑和调试。它的操作直观，界面干扰较少，支持多线程并能够使用 OpenGL 加速画布显示。Krita 是一款跨平台软件，支持 Windows、Linux 和 Mac OSX。

Krita 是 KDE 的子项目之一，原先是 KOffice 1.4 的一个组成部分。KOffice 后来改名为 Calligra，Krita 在 3.0 版起从 Calligra 项目分离出来，但依然是 KDE 的一部分。2013 年 Krita 社区成立了 Krita 基金会为项目提供资金支持，此后更是连续三年成功地进行了 Kickstarter 众筹，为项目募集了资金以支持两位全职开发人员的工作，让软件的开发得到了大幅推进。

“Krita”是一个瑞典语单词，它的含义是“绘画”和“粉笔”。该项目的曾用名包括“KImageShop”和“Krayon”，但它们因为商标问题已被弃用。

Krita 使用 C++ 编写，基于 KDE Frameworks 和 Qt，支持 Python 脚本扩展。

Krita 的吉祥物是 Kiki，一只由 Tyson Tan 设计的电子松鼠。

## 安装

`sudo apt-get install krita`

## 卸载

`sudo apt-get remove krita`

## Krita Lime PPA

Krita 的近年来的开发比较迅速，每个新版都会带来意义重大的改进，而 LTS 的软件仓库版本过于老旧。因此建议使用官方维护的 PPA：

`sudo add-apt-repository ppa:kritalime/ppa`

`sudo apt-get update`

## 矢量图层兼容性

Krita 4.0 版的矢量引擎已经更改。它从原先 Calligra 的自有组件换成了一个全新编写的 SVG 标准矢量引擎。Krita 4.0 和之后的版本将无法保证正常解析 3.0 系列以及之前版本保存的矢量图层。请留意。

## OpenGL 加速

Krita 使用 OpenGL 加速来实现高品质的缩放、旋转，并极大地提升绘画的响应速度。要发挥 OpenGL 加速的真正价值，你的 GPU 需要支持 OpenGL 3.0 以上。各主要 GPU 生产商开始支持 OpenGL 3.0 的产品线和发布时间如下：

- Intel：第三代 HD Graphics 系列，IvyBridge 或 Bay-Trail 架构，2012 年。代表产品：Celeron J1x00、N2x00、Celeron (G)1xx0、Pentium J2x00、N3500、Pentium (G)2xx0、Core i3/5/7-3xx0 等。
- AMD/ATI：Radeon HD 2000 系列，TeraScale 1 架构，2007 年。代表产品：Radeon HD 2400 PRO、Radeon HD 2600 PRO 等。
- Nvidia：GeForce 8 系列，Tesla 架构，2006 年。代表产品：GeForce 8400 GS、GeForce 8800 GTS / 9800 GTX / GTS 250 等。

## 仓库地址

<http://packages.deepin.com/deepin/pool/main/k/krita/>

## 相关链接

- 官方网站：<https://krita.org/>
- 维基百科页面：<https://en.wikipedia.org/wiki/Krita>
- Krita Lime PPA：<https://launchpad.net/~kritalime/+archive/ubuntu/ppa>
