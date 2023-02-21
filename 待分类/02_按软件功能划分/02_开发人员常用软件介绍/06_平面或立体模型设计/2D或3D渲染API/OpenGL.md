---
title: OpenGL
description: 
published: true
date: 2022-10-25T01:24:08.268Z
tags: 显卡
editor: markdown
dateCreated: 2022-09-22T05:46:13.940Z
---

# OpenGL
**OpenGL**是用于[渲染](https://baike.baidu.com/item/渲染?fromModule=lemma_inlink)[2D](https://baike.baidu.com/item/2D?fromModule=lemma_inlink)、[3D](https://baike.baidu.com/item/3D?fromModule=lemma_inlink)[矢量图形](https://baike.baidu.com/item/矢量图形?fromModule=lemma_inlink)的跨[语言](https://baike.baidu.com/item/语言?fromModule=lemma_inlink)、[跨平台](https://baike.baidu.com/item/跨平台?fromModule=lemma_inlink)的[应用程序编程接口](https://baike.baidu.com/item/应用程序编程接口?fromModule=lemma_inlink)（API）。

1、终端中输入命令：sudo apt install mesa-utils

2、终端中输入命令：glxinfo | grep -i opengl

若当前使用的为N卡，则输入命令后，截图信息如下：

![n卡.png](/for_trans/opengl/n卡.png)

若当前使用的为A卡（AMD机型），则输入命令后，截图信息显示如下：

![a卡.png](/for_trans/opengl/a卡.png)

若当前使用的为I卡（Intel机型），则输入命令后，截图信息显示如下：

![i卡.png](/for_trans/opengl/i卡.png)

若显示有误，可能是mesa驱动版本不匹配，需做适配。

