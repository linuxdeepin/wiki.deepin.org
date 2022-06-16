---
title: Systemd
description: 
published: true
date: 2022-06-16T03:02:15.607Z
tags: systemd
editor: markdown
dateCreated: 2022-06-16T03:02:15.607Z
---

# Steam

[跳到导航](http://old.deepin.wiki/index.php?title=Steam#mw-head)[跳到搜索](http://old.deepin.wiki/index.php?title=Steam#searchInput)

Steam 是由 Valve 公司开发一个游戏平台。

Proton 是 Steam 提供的修改版的 [Wine](http://old.deepin.wiki/index.php?title=Wine)，可以在 Linux 系统中运行部分 Windows 游戏。

Steam 平台的游戏的兼容情况可以在 [Protondb](https://protondb.com/) 查到。

## 目录



- [1缺失 32 位库](http://old.deepin.wiki/index.php?title=Steam#.E7.BC.BA.E5.A4.B1_32_.E4.BD.8D.E5.BA.93)
- [2强制打开 Steamplay 支持](http://old.deepin.wiki/index.php?title=Steam#.E5.BC.BA.E5.88.B6.E6.89.93.E5.BC.80_Steamplay_.E6.94.AF.E6.8C.81)
- [3使用蒸汽平台](http://old.deepin.wiki/index.php?title=Steam#.E4.BD.BF.E7.94.A8.E8.92.B8.E6.B1.BD.E5.B9.B3.E5.8F.B0)
- 4提示与技巧
  - [4.1Steam++](http://old.deepin.wiki/index.php?title=Steam#Steam.2B.2B)

## 缺失 32 位库

```
sudo apt-get install libgl1-nvidia-glvnd-glx:i386
```

## 强制打开 Steamplay 支持

在设置中选择 Steamplay，可以强制启用 Proton。

你也可以在游戏库中右键游戏，为游戏选择特定版本的 Proton。

## 使用蒸汽平台

你可以通过附加命令 `-steamchina` 进入中国版蒸汽平台。

不过由于软件/游戏安装限制，你不能通过安装 Proton 兼容层来运行 Windows 游戏，而只能游玩原生 Linux 游戏。

祝你好运。

## 提示与技巧

### Steam++

Steam++ 是一个 Steam 辅助软件，提供本地反代、SAM 等功能。

Github 仓库：https://github.com/SteamTools-Team/SteamTools
Gitee 仓库：https://gitee.com/rmbgame/SteamTools