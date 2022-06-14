---
title: 在Linux中使用flatpak
description: Flatpak是一个程序包管理实用程序，可让您分发，安装和管理软件，而不必担心依赖项，运行时或Linux分发。由于您可以安装软件而没有任何问题，而与Linux发行版无关（无论是基于Debian的发行版还是基于Arch的发行版），因此Flatpak称为通用软件包。
published: true
date: 2022-06-14T07:11:51.386Z
tags: 
editor: markdown
dateCreated: 2022-06-14T06:30:47.687Z
---

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

---------------------------------------------
# Flatpak安装包下载
https://flatpak.org/setup/
# Flathub仓库
https://www.flathub.org/apps
# 上海交大镜像
https://mirrors.sjtug.sjtu.edu.cn/docs/flathub
# Flatpak文档
https://docs.flatpak.org/zh_CN/latest/