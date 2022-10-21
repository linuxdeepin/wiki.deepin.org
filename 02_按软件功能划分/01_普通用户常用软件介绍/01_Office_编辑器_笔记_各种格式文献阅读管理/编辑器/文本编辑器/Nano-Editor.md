---
title: Nano-Editor
description: GNU nano 是一款终端下使用的经典工具，旨在免费替代 Pico 文本编辑器。
published: true
date: 2022-06-28T01:12:11.673Z
tags: nano-editor
editor: markdown
dateCreated: 2022-06-28T01:12:09.393Z
---

## 简介

在一开始的时候...

多年来，Pine 一直是 Unix 系统上用于阅读电子邮件的程序。Pico 文本编辑器是程序的一部分，用于编写他或她的邮件消息。由于 Pico 和 Pine 组织良好、易于使用的界面，许多 Unix 初学者蜂拥而至。随着 90 年代中后期 GNU/Linux 的普及，许多大学生对 Pine 和 Pico 的优势（和劣势）非常熟悉。

然后是Debian...

Debian GNU/Linux发行版以其在分发真正“自由”软件（即没有重新分发限制的软件）方面的严格标准而闻名，它不会包含 Pine 或 Pico 的二进制包。许多人有一个严重的困境：他们喜欢这些程序，但当时可用的版本并不是GNU意义上的真正自由软件。

事件...

1999 年末，Chris Allegretta（我们的英雄）再次自言自语地抱怨 Pico 分发的许可证不够完美，它附带的 1000 个 makefile 以及如何仅仅进行一些小的改进就可以使它成为最好的世界编辑（TM）。作为从 Slackware 到 Debian 的转换者，他想念一个包含 Pine 和 Pico 的简单二进制包，并且已经厌倦了自己下载它们。

最后，内部突然出现了一些东西，克里斯在一个周末连续几个小时像疯子一样编码和破解，以制作一个（几乎无法使用的）Pico 克隆，当时称为 TIP（Tip Is't Pico）。没有文件名就无法调用该程序，无法保存文件，没有帮助文本显示，拼写检查等等。但随着时间的推移，它得到了改进，并且在一些伟大的编码员的帮助下，它成熟到了（希望是）今天的稳定状态。

2001 年 2 月，nano 被 Richard Stallman 宣布为官方 GNU 程序。nano 也在 2001 年 3 月 22 日发布了它的第一个产品版本。

2000 年 1 月 10 日，由于与另一个名为“tip”的程序的命名空间冲突，TIP 正式更名为 nano。最初的 'tip' 程序“建立到远程主机的全双工终端连接”，并包含在许多较旧的 Unix 系统（和较新的系统，如 Solaris）中。最初并没有注意到冲突，因为大多数 GNU/Linux 发行版（开发 nano 的地方）都没有包含“tip”实用程序。

## 安装
实际上在Debian/Deepin核心系统中已经包含了该编辑器，如果没有，你可以用下列命令安装
`sudo apt install nano`

也可以从官方网站下载deb包进行安装
` dpkg -i nano_x.y-1*.deb`

目前nano的最新版本是6.3，而Deepin中的版本是3.2-3
## Deepin仓库地址
https://packages.deepin.com/deepin/pool/main/n/nano/[](https://packages.deepin.com/deepin/pool/main/n/nano/)

## 从源头构建
如果你是下载了源码，则需要通过编译的方式安装:

`tar -xvf nano-x.y.tar.gz`

解压后，可能你需要自行编辑设置，那么你可以这样做：

`cd nano-x.y/`
`./configure`
`make`
`make install`    (当然了，前提是你需要获得root权限)
## 卸载

`sudo apt remove nano`

## 特征

![nano-5.7.png](/nano-5.7.png)
### 便于使用
Nano编辑器可以在任何终端中使用，大部分的unix、linux发行版都将其作为默认安装的核心组件之一。

### 高度可定制
 你可以通过编辑 ~/.nanorc 文件设置配色方案或快捷键绑定。 

### 默认编辑器
你需要让 nano 成为你的 $EDITOR。如果你想保存它，你应该在你的.bashrc中加入这样的一行，如果你使用 bash （或者.zshrc如果你相信 zsh）：

`export EDITOR=/usr/local/bin/nano`

或者，如果您使用 tcsh，请将其放入您的.cshrc文件中：

`setenv EDITOR /usr/local/bin/nano`


## 相关链接

[官方主页](https://www.nano-editor.org/)
[源码地址](https://mirrors.sjtug.sjtu.edu.cn/gnu/nano/)