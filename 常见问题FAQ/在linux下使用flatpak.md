---
title: 在Linux中使用flatpak
description: Flatpak是一个程序包管理实用程序
published: true
date: 2022-10-26T09:12:58.015Z
tags: 
editor: markdown
dateCreated: 2022-06-14T06:30:47.687Z
---

# 说明
Flatpak是一种构建、发布、安装和运行应用程序的工具
Flatpak的设计目标是：
* 使应用程序可以安装在任何一个发行版上
* 为应用程序提供固定的环境，实施测试和减少缺陷
* 实现应用程序和操作系统的解耦和，使应用程序可以不依赖于特定的发行版本
* 使应用程序可以自带依赖，能够使用linux发行版没有提供的依赖，避免其对特定发行版本甚至特定库的依赖
* 在沙箱中独立运行应用程序，提升安全性

```
# 安装 Flatpak
sudo apt install flatpak

# 添加 Flathub 仓库
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# 移除 Flathub 仓库
flatpak remote-delete flathub

# 镜像 Flathub 仓库
sudo flatpak remote-modify flathub --url=https://mirror.sjtu.edu.cn/flathub

# 查找
flatpak search xxx

# 安装
flatpak install xxx

# 删除
flatpak uninstall xxx

# 更新
flatpak update xxx

# 运行
flatpak  run xxx

```

### Flatpak安装包下载
https://flatpak.org/setup/

### Flathub仓库
https://www.flathub.org/apps

### 上海交大镜像
https://mirrors.sjtug.sjtu.edu.cn/docs/flathub

### Flatpak文档
https://docs.flatpak.org/zh_CN/latest/