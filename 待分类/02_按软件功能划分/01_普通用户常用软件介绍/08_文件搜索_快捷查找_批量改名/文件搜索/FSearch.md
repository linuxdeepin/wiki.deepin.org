---
title: FSearch
description: 一款快速的文件搜索工具
published: true
date: 2022-10-21T02:03:18.707Z
tags: 
editor: markdown
dateCreated: 2022-07-15T14:49:47.091Z
---

## 简介
FSearch 是一款快速的文件搜索工具，灵感来自于 Everything 搜索引擎。它是用 C 语言编写的，基于GTK3。

## 安装
```
sudo apt install io.github.fsearch
```

## 卸载
```
sudo apt remove --purge io.github.fsearch
```

## 仓库地址
## 常见问题
### 1.没有中文版吗？
官方仓库的版本只有英文版。
### 2.如何自定义搜索范围（添加或排除某些目录）？
进入 Edit 菜单-> Preferences -> Database 选项卡。
Include（包含）表示这些目录在搜索范围内。
Exclude（不包含）表示这些目录被排除在搜索范围外。

![fsearch1.png](/fsearch1.png)
![fsearch2.png](/fsearch2.png)
## 相关链接
FSearch Github 主页：https://github.com/cboxdoerfer/fsearch