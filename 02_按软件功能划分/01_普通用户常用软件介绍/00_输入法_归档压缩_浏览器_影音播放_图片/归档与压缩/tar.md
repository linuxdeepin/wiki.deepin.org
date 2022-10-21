---
title: tar
description: tar
published: true
date: 2022-06-21T10:34:26.089Z
tags: 压缩
editor: markdown
dateCreated: 2022-05-05T09:46:39.898Z
---

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

## 参数说明

下列参数中必须有至少一个被使用：

`-A`, `--catenate`, `--concatenate`
&emsp;将一存档与已有的存档合并

`-c`, `--create`
&emsp;创建一个新的存档

`-d`, `--diff`, `--compare`
&emsp;比较存档与相应的未存档文件的不同之处

`-r`, `--append`
&emsp;将文件附加到存档结尾

`-t`, `--list`
&emsp;列出存档中文件的目录

`-u`, `--update`
&emsp;仅将较新的文件附加到存档中

`-x`, `--extract`, `--get`
&emsp;从存档提取文件

`--delete`
&emsp;把指定文件从存档中删除（不要用于磁带！）
      
## 常用选项

`-C`, `--directory 目录`
&emsp;提取存档到指定目录

`-f`, `--file [主机名:]文件`
&emsp;指定存档或设备中的文件 (默认是 "-"， 表示 标准输入/输出)

`-j`, `--bzip2`
&emsp;用 bzip2 处理存档; 用于 .bz2 文件

`-J`, `--xz`
&emsp;用 xz 处理存档; 用于 .xz 文件

`-p`, `--preserve-permissions`
&emsp;提取所有保护信息

`-v`, `--verbose`
&emsp;显示文件处理过程

`-z`, `--gzip`, `--ungzip`
&emsp;用 gzip 处理存档; 用于 .gz 文件
