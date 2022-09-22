---
title: fzf使用说明
description: 
published: true
date: 2022-06-28T08:23:24.992Z
tags: fzf
editor: markdown
dateCreated: 2022-06-28T08:23:22.831Z
---

# fzf 使用说明

## 前言

fzf 是一个命令行下对文件或者文件内容过滤的工具。它有着高效的性能，简单易用。不过要结合其他工具，才能获得最大的收益。

网上很多 fzf 的说明都是针对 zsh 这个 shell 来说明配置方法。但是我不用这个 shell，而是用系统自带的 bash shell。

## 安装

```bash
# 安装
sudo apt install fzf
```

## 配置

默认输入 fzf 可以正常执行这个搜索过滤程序，它会调用 find 作为输入，即当前目录下的文件会被搜索。

这种使用方式很无趣，比如我们想在参数位置，输入一个我们找到的文件，如： cat xxx。 想实现这个要绑定按键，用来调用 fzf。

这个绑定的操作其实有一个样板，在/usr/share/doc/fzf/examples里面：

```bash
# 拷贝按键绑定样例到用户目录
cd 
cp /usr/share/doc/fzf/examples/key-bindings.bash .

# 将脚本添加到.bashrc 自动执行
echo source key-bindings.bash >> .bashrc

# 让脚本立即生效
. bashrc
```

然后就可以用 `ctrl + t` 调出 fzf 搜索文件名了。

另外：
`alt + c` : 搜索目录，并选择进入

## 参考

1. FZF：终端下的文件查找器【猛男必备233333】: <https://www.bilibili.com/video/av80254519>
2. Bash readline 使用技巧: <http://www.kerneltravel.net/newbie/bash_readline.htm>