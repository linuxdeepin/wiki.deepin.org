---
title: thinkbook_14双显卡切换
description: 
published: true
date: 2022-10-18T02:40:05.812Z
tags: 独显, 双显卡
editor: markdown
dateCreated: 2022-09-22T05:57:05.542Z
---

# thinkbook_14双显卡切换
> 本文只针对thinkbook 14双显卡机型，其它机器使用该方法进行显卡切换存在风险
{.is-warning}

[files.zip](/for_trans/切换显卡/files.zip)解压以上压缩的工具包后，

1. 进行初始化(运行脚本)：`sudo sh Initialize.sh`

2. 切换为独显命令（运行脚本）：`sudo sh NVIDIA.sh`

3. 若出现黑屏或切换不成功或切回A卡驱动，可进行还原（运行脚本）：`sudo sh Rescue.sh`

若切换显卡后，可使用[OpenGL](/zh/测试/OpenGL)中命令查看当前使用的显卡，输入命令`glxinfo | grep -i opengl`,查看OpenGL vendor string字段即可判定当前使用的显卡。