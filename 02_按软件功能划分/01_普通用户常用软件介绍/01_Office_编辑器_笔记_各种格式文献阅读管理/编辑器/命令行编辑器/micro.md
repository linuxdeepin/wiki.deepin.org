---
title: Micro-Editor
description: 一个用go语言开发的优秀的终端编辑器
published: true
date: 2022-10-21T08:26:47.562Z
tags: editor
editor: markdown
dateCreated: 2022-06-28T00:18:47.152Z
---

## 简介

Micro-Editor是一个基于终端的文本编辑器，旨在易于使用和直观，同时还利用了现代终端的功能。它是一个单一的、包含电池的、静态的二进制文件，没有依赖关系；您可以立即下载并使用它！

顾名思义，micro 旨在通过易于安装和使用来成为 nano/vim 编辑器的继承者。对于喜欢在终端中工作的人或经常通过 SSH 编辑文件的人来说，它努力成为一名全职编辑器。

## 安装

`curl https://getmic.ro | bash`

## 卸载

`rm micro`

## 从源头构建

确保您拥有 Go 版本 1.16 或更高版本并且启用了 Go 模块。

`git clone https://github.com/zyedidia/micro`
`cd micro`
`make build`
`sudo mv micro /usr/local/bin # optional`
## 特征

![micro-monokai.png](/micro-monokai.png)
- 便于使用
Micro 的第一大特性是易于安装（它只是一个没有依赖关系的静态二进制文件）并且易于使用。

- 高度可定制
使用简单的 json 格式来配置您的选项并根据您的喜好重新绑定键。如果需要更多功能，可以使用 Lua 进一步配置编辑器。

- 颜色和突出显示
Micro 支持超过 75 种语言，并有 7 种默认配色方案可供选择。Micro 支持 16、256 和真彩色主题。语法文件和配色方案也很容易制作。

- 多个光标
Micro 支持 Sublime 风格的多光标，直接在终端中为您提供大量编辑功能。

- 插件系统
Micro 支持成熟的插件系统。插件是用 Lua 编写的，并且有一个插件管理器可以自动为您下载和安装插件。

- 常用键绑定
Micro 的键绑定是您对易于使用的编辑器所期望的。您还可以在bindings.json文件中重新绑定任何绑定而不会出现问题。

- 鼠标支持
Micro 完全支持鼠标。这意味着您可以单击并拖动以选择文本，双击按单词选择，三次单击以按行选择。

- 终端仿真器
在 micro 中运行真正的交互式 shell。您可以在一侧使用代码打开一个拆分，在另一侧使用 bash——所有这些都来自 micro 内部。
## 相关链接

[官方主页](https://micro-editor.github.io/)
[源码地址](https://github.com/zyedidia/micro/)