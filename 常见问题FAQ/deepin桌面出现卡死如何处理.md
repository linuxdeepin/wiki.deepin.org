---
title: deepin桌面卡死如何处理
description: 桌面卡死
published: true
date: 2022-06-13T08:11:27.292Z
tags: 卡死, 桌面
editor: markdown
dateCreated: 2022-06-13T08:03:28.189Z
---


## 出现桌面卡死的情况
1. 出现文件错误
2. 授予root权限后执行错误，一直处于执行状态
3. 启用某个进程导致（如打开wine qq）

## 解决方法
Ctrl + Alt + T启用终端（Terminal）；若终端无法启用，使用Ctrl + Alt + Fn(主要针对笔记本) + F2/F3/F4/F5/F6进入tty界面，需输入帐户、密码。可以直接使用重启命令：
- `reboot`         
- `shutdown -t now`

但若存在部分文档未保存，直接重启就会导致数据丢失。可以尝试使用以下方法：输入命令top，查看各项进程占用CPU比率，CPU占用100%的进程一定异常，需把该进程杀死。可以使用的命令：
- `kill -9 + PID    #PID为进程识别号`
- `sudo systemctl restart lightdm    #重新加载图形界面`

然后使用快捷键Ctrl + Alt + F1，回到图形界面即可。
