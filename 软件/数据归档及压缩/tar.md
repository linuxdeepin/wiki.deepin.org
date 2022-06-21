---
title: tar
description: tar
published: true
date: 2022-06-21T10:20:12.720Z
tags: 压缩
editor: markdown
dateCreated: 2022-05-05T09:46:39.898Z
---

# Tar

## 描述

tar是一个用于储存或提取tar文件的命令行程序。 tar文件可放在磁盘中， 也可以存为普通文件。tar 的第一个参数必须是操作参数A、c、d、r、t、u、x  中的一个， 参数后面可跟着任意可选选项。tar的最后一个参数是你要处理的文件或目录的名字。 如果你指定了一个目录，  该目录的所有子目录都将被加入存档。

## 应用举例

`tar -xvf foo.tar`
&emsp;提取 foo.tar 文件并显示提取过程

`tar -xzf foo.tar.gz`
&emsp;提取用 gzip 压缩的文件 foo.tar.gz

`tar -cjf foo.tar.bz2 bar/`
&emsp;用 bzip 为目录 bar 创建一个叫做 foo.tar.bz2存档

`tar -xjf foo.tar.bz2 -C bar/`
&emsp;把用 bzip 压缩的文件 foo.tar.bz2 提取到 bar 目录

`tar -xzf foo.tar.gz blah.txt`
&emsp;把文件 blah.txt 从 foo.tar.gz 中提取出来

注意: 当压缩或提取的时候， 压缩类型选项常常是不必需的， 因为tar会根据文件的后缀自动选择压缩类型。
