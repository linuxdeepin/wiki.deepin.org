---
title: grep和find命令
description: 
published: true
date: 2022-10-26T06:56:30.779Z
tags: grep find
editor: markdown
dateCreated: 2022-07-05T09:50:34.501Z
---

# grep和find命令

## 前言

linux 中查找东西，最常用的就是 grep 和 find。grep 是处理文件内容，而 find 是查找文件。

本文简要介绍他们和一些相关工具的配合。

## grep

grep 有两种使用场景，一种是对其他命令的输出做处理，搜索管道来的数据。一种是对文件系统中的文件内容进行查找。个人极少使用第二个场景。

### 对命令输出进行处理

```bash
# 查找当前目录下的文件名字
ls | grep wps
# 查询显卡的驱动
lspci -k | grep VGA -A2
```

### 对文件进行操作

```bash
# 查看 cpu 支持什么指令集
grep -c sse /proc/cpuinfo
```

## find
find 命令可以根据给定的路径和表达式查找文件或目录, find 参数选项很多, 并且支持正则. 如不加任何参数表示查找当前路径下的所有文件和目录

语法格式：
```bash
find 参数 路径 查找和搜索范围
# 常用参数：
-name	按名称查找
-size	按大小查找
-user	按属性查找
-type	按类型查找
-iname	忽略大小写
```
示例: 在当前目录下搜索名称后缀为 .txt 的所有文件名
```
find -name ./ "*.txt
```
示例: 在当前 video 目录下搜索所有文件大小大于 1M 的文件
```
find -size ./video +1M
```










