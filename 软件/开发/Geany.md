---
title: Geany
description: 
published: true
date: 2022-05-14T02:45:09.777Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:35:08.137Z
---

# 简介

Geany是一个跨平台的开源集成开发环境，它支持基本的语法高亮、代码自动完成、调用提示、插件扩展。支持文件类型有C、CPP、Java、Python、PHP、 HTML、DocBook、Perl、LateX和Bash脚本。

# 安装

`sudo apt-get install geany`

# 卸载

`sudo apt-get remove geany`

# 仓库地址

[http://packages.deepin.com/deepin/pool/main/g/geany/](http://packages.deepin.com/deepin/pool/main/g/geany/)


# 常见问题
## 点击执行时无法调用深度终端
在首选项→工具→虚拟终端，修改终端的命令为：

`> deepin-terminal -x "/bin/sh" %c` 

点击“应用”按钮后即可。

# 相关链接