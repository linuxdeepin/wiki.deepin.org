---
title: Steam说明2
description: 
published: true
date: 2022-10-25T00:45:25.588Z
tags: 
editor: markdown
dateCreated: 2022-06-16T03:05:22.937Z
---

Steam
跳到导航跳到搜索
Steam 是由 Valve 公司开发一个游戏平台。

Proton 是 Steam 提供的修改版的 Wine，可以在 Linux 系统中运行部分 Windows 游戏。

Steam 平台的游戏的兼容情况可以在 Protondb 查到。


目录
1	缺失 32 位库
2	强制打开 Steamplay 支持
3	使用蒸汽平台
4	提示与技巧
4.1	Steam++
缺失 32 位库
sudo apt-get install libgl1-nvidia-glvnd-glx:i386
强制打开 Steamplay 支持
在设置中选择 Steamplay，可以强制启用 Proton。

你也可以在游戏库中右键游戏，为游戏选择特定版本的 Proton。

使用蒸汽平台
你可以通过附加命令 -steamchina 进入中国版蒸汽平台。

不过由于软件/游戏安装限制，你不能通过安装 Proton 兼容层来运行 Windows 游戏，而只能游玩原生 Linux 游戏。

祝你好运。

提示与技巧
Steam++
Steam++ 是一个 Steam 辅助软件，提供本地反代、SAM 等功能。

Github 仓库：https://github.com/SteamTools-Team/SteamTools
Gitee 仓库：https://gitee.com/rmbgame/SteamTools