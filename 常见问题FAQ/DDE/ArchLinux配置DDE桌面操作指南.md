---
title: ArchLinux配置DDE桌面操作指南
description: 
published: true
date: 2022-09-08T11:06:33.609Z
tags: arch dde
editor: markdown
dateCreated: 2022-09-08T09:54:52.738Z
---

# 一、Arch Linux简介
Arch Linux 是一个独立开发的 x86-64 通用 GNU/Linux 发行版，其用途广泛，足以适应任何角色。开发侧重于简单、极简主义和代码优雅。Arch 是作为一个最小的基础系统安装的，由用户配置，通过仅安装其独特目的所需或所需的东西来组装他们自己的理想环境。官方没有提供 GUI 配置实用程序，大多数系统配置是通过编辑简单的文本文件从 shell 执行的。Arch 努力保持领先，通常提供大多数软件的最新稳定版本。

Arch Linux 使用自己的 Pacman 包管理器，它将简单的二进制包与易于使用的包构建系统结合在一起。这允许用户轻松管理和自定义包，从官方 Arch 软件到用户自己的个人包，再到来自 3rd 方来源的包。存储库系统还允许用户轻松构建和维护自己的自定义构建脚本、包和存储库，从而鼓励社区发展和贡献。

最小的 Arch 基础包集位于精简的 [core] 存储库中。此外，官方的[extra]、[community]、[testing] 仓库提供了数千个高质量的包来满足您的软件需求。Arch 还提供 Arch Linux 用户存储库 (AUR)，其中包含超过 49,000 个构建脚本，用于使用 Arch Linux makepkg 应用程序从源代码编译可安装包。

Arch Linux 使用“滚动发布”系统，允许一次性安装和永久软件升级。通常不需要将 Arch Linux 系统从一个“版本”重新安装或升级到下一个“版本”。通过发出一个命令，Arch 系统就可以保持最新状态并处于最前沿。

Arch 努力使其软件包尽可能接近原始上游软件。补丁仅在必要时应用，以确保应用程序与安装在最新 Arch 系统上的其他软件包一起正确编译和运行。

总而言之：Arch Linux 是一个多功能且简单的发行版，旨在满足有能力的 Linux® 用户的需求。它功能强大且易于管理，使其成为服务器和工作站的理想发行版。把它带到你喜欢的任何方向。

# 二、Arch Linux优缺点总结
Arch linux是朝向轻量(lightweight)以及简单(simple)的Linux发行版。其中“简单”(Simplicity)被定义为“避免不必要或复杂的修改”，也就是说，是由开发者角度定义，而非用户角度思考。

## 优势

1. 特有的包管理系统Archlinux是针对特定处理器而优化过的，能够更好地利用CPU周期以提高性能。相比Debian/Ubuntu、SUSE、RedHat/Fedora等其他发行版，Archlinux属于轻量级选手，其简单的设计让它容易被轻松扩展和配置成为任何想要的系统类型。

2. 通过二进制包管理系统pacman，仅需一个命令就能完成安装、升级等多个操作。同时也附带一个类似ports的包构建系统ABS(Arch Build System)。

## 缺点

1. 安装过程简陋，缺乏智能直观的错误处理，需要用户有一定的Linux环境常识才能正确安装使用。仅对i686、x86_64 架构优化，对于其它CPU架构支持匮乏。

2. 包管理系统pacman在升级过程缺乏对系统核心组件的回溯保护，如升级的Kernel有问题，即导致系统无法启动。

3. 系统软件缺乏严谨的测试管理机制，稳定性、可靠性不如Redhat、CentOS、Debian等发行版，难以在企业用户中推广。

# 三、Arch Linux下载
/home/uos/.config/Typora/typora-user-images/image-20220906170401619.png


