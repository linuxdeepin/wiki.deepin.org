---
title: grep和find命令
description: 
published: true
date: 2022-07-05T09:50:36.417Z
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

## 