---
title: Deepin安装苹果编程语言 swift 3
description: 
published: true
date: 2022-06-09T01:13:37.195Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:42:45.993Z
---

## 简介

在Deepin安装苹果编程语言 swift 3

## 下载

- 我的环境是Deepin 15.3
- 下载swift3.0 ， 下载地址:<https://swift.org/download/#releases> 下载swift 3，我这选择Ubuntu 16.04
- 也可以使用纯命令行操作：
- 使用 wget 工具下载 如果你没有安装 则需要安装 wget
- // 下载Swift3.0
- $ wget <https://swift.org/builds/swift-3.0.2-release/ubuntu1604/swift-3.0.2-RELEASE/swift-3.0.2-RELEASE-ubuntu16.04.tar.gz>

## 安装

### 首先需要 安装 clang 及相关库

- $ sudo apt-get install clang libicu-dev

### 导入PGP密匙到你的密匙环

- $ wget -q -O - <https://swift.org/keys/all-keys.asc> |  gpg --import -

### 解压下载的文件

#### 命令行操作

- $ tar xzf swift-3.0.2-RELEASE-ubuntu16.04.tar.gz
- 重命名文件夹（只为方便操作）: $ mv swift-3.0.2-RELEASE-ubuntu16.04 swift-3.0.2
- 将文件移动到/opt目录下（可选）$ sudo mv swift-3.0.2 /opt // 如果没有操作此步，下面添加path时需要根据你自己的swift路径进行修改

#### 非命令行操作

- 略

### 配置命令path

- $ cd
- $ vim .bashrc
- // 在文件的最底部配置swift3.0 的路径(输入下面的内容)
- export PATH= /opt/swift-3.0.2/usr/bin:"${PATH}"
- 保存退出
- 到此处，Swift已经算安装好了 这个时候在终端 输入 swift --version，会输出相关版本信息，如果提示未找到命令可关闭终端，再重新打开，或者输入source .bashrc
