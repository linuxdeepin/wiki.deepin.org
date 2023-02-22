---
title: deepin桌面卡死如何处理
description: 桌面卡死
published: true
date: 2022-07-29T15:06:02.716Z
tags: 桌面, 卡死
editor: markdown
dateCreated: 2022-06-13T08:03:28.189Z
---

## 出现桌面卡死的情况
1. 桌面环境卡死
2. Xorg 卡死
3. 启用某个进程导致（如打开wine qq）

## 解决方法
Ctrl + Alt + T 启用终端（Terminal）；若终端无法启用，使用 Ctrl + Alt + Fn(主要针对笔记本) + F2/F3/F4/F5/F6 进入tty界面，需输入帐户、密码。

对于桌面环境卡死，执行下面命令重启桌面环境：
```
sudo systemctl restart lightdm
```
对于 Xorg 卡死，执行下面命令结束 Xorg：
```
sudo killall Xorg
```
对于某个特定进程导致桌面卡死，可以尝试使用以下方法：
输入命令top，查看各项进程CPU占用率，CPU占用100%的进程一定异常，需把该进程杀死。可执行命令：
```
kill -9 PID #PID为进程号
```
然后使用快捷键Ctrl + Alt + F1，回到图形界面即可。
当然如果以上方法无法解决，也可以直接使用重启命令：
```
reboot
```

