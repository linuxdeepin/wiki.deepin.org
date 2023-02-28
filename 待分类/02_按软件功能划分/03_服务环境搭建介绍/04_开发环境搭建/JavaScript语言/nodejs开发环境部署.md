---
title: nodejs开发环境部署
description: Linux上Node.js开发环境部署以及一些工具介绍
published: true
date: 2023-02-28T08:28:57.675Z
tags: 开发环境搭建, 教程
editor: markdown
dateCreated: 2023-02-28T07:39:41.665Z
---

## 简介
Node.js发布于2009年5月，由Ryan Dahl开发，是一个基于Chrome V8引擎的JavaScript运行环境，使用了一个事件驱动、非阻塞式I/O模型，让JavaScript 运行在服务端的开发平台，它让JavaScript成为与PHP、Python、Perl、Ruby等服务端语言平起平坐的脚本语言。

## 安装

nodejs就是一个基于google的v8引擎的一个解释器。可以使用下面命令进行安装。

### 使用命令从源里安装
```bash
sudo apt install nodejs
```

可以用通过以下命令验证安装是否成功：
```bash
node -v
```

看到以下输出说明安装成功：
```bash
v10.21.0
```

### 使用安装包安装
从源里下载的版本相对过低，可尝试下载安装包解压安装。
可参考下面链接进行安装。

[安装包链接](https://nodejs.org/zh-cn/download/)

选择长期维护版，或者根据自己的需要选择对应版本。

选择Linux 二进制文件 根据系统架构选择，这里以x64为例。

```bash
wget https://nodejs.org/dist/v18.14.2/node-v18.14.2-linux-x64.tar.xz
```
通过wget下载安装包。

```bash
sudo tar -xvf  node-v18.14.2-linux-x64.tar.xz -C /usr/local/
```
将压缩包解压到/usr/local目录下。

```bash
sudo mv /usr/local/node-v18.14.2-linux-x64 /usr/local/node
```

```bash
vim ~/.bashrc
```
添加环境变量。
ps: 如果使用zsh则配置.zshrc，或者选择/etc/profile做全局配置

```bash
export NODE_HOME=/usr/local/node
export PATH=$NODE_HOME/bin:$NODE_HOME/node_global:$NODE_HOME/node_modles:$NODE_HOME/node_global/bin:$PATH
```

```bash
mkdir /usr/local/node/node_{global,cache}
npm config set prefix "/usr/local/node/node_global"
npm config set cache "/usr/local/node/node_cache"
```
自定义全局目录以及缓存文件夹。

```bash
npm config list 
```
通过该指令查看配置的列表。

```bash
source ~/.bashrc
```
刷新shell环境。