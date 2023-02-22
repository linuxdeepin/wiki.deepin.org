---
title: neovim
description: 21世纪新的vim
published: true
date: 2023-02-22T02:52:45.367Z
tags: neovim, vim
editor: markdown
dateCreated: 2023-02-21T10:38:10.560Z
---

# Neovim
对vim的架构进行了大幅改动，支持lua和python来编写插件，视觉也更美观。
vim兼容性强，neovim则是更轻盈，也更适合扩展。

## 安装
``` shell
apt install neovim
```
## 卸载
``` shell
apt remove neovim
```

## 使用
### 打开文件
``` shell
nvim 文件				# TUI启动
nvim-qt 文件		# GUI启动
```

### 配置文件
兼容vim的配置语法。
默认的用户配置文件位置如下：
> 1. ~/.neovimrc
> 2. ~/.config/nvim/init.vim


其他操作基本是继承了vim，只在细节上有所差别。