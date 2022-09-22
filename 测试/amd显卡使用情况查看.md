---
title: amd显卡使用情况查看
description: 
published: true
date: 2022-09-22T06:03:21.779Z
tags: linux, amd
editor: markdown
dateCreated: 2022-09-22T06:03:20.081Z
---

### 检查AMD显卡使用情况

原文链接：[linux amd显卡使用情况查看](/zh/https://blog.csdn.net/wenshuifuping/article/details/107929598)
使用工具radeontop

1、安装 radeontop：`sudo apt install radeontop`

2、运行radeontop

打开终端，使用命令：`sudo radeontop`，出现如下界面，最后一行VRAM为显存大小1982M，前边833M为使用大小，整体使用率为42.01%。
![amd显存.png](/for_trans/amd显存.png)