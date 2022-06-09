---
title: Pip
description: pip
published: true
date: 2022-06-09T02:17:19.465Z
tags: python
editor: markdown
dateCreated: 2022-05-05T10:08:37.502Z
---

# Pip

## 简介

pip是python包管理工具，提供python包搜索、下载、安装、卸载、更新等操作。
如果你在python官网下载了windows的python安装包，安装时会默认勾选安装pip。
在近几年更新的linux发行版中，python3已默认集成，如果没有自带pip，可以通过以下方式安装：

```shell
# apt包管理器
apt install -y python3-pip
```

- 验证是否已安装

```shell
pip3 --version
```

## 常用命令

- 这里以python的一个数据分析包`pandas`作为示例

```shell
# 查看帮助
pip3 --help

# 搜索pandas包
pip3 search pandas

# 安装pandas包。默认安装最新版
pip3 install pandas

# 安装或更新pandas包
pip3 install -U pandas

# 查看pandas包的信息
pip3 show pandas

# 列出已安装的python包
pip3 list

# 卸载pandas
pip3 uninstall pandas
```

## 使用中国大陆的镜像源

pip默认通过官方源下载安装包，而官方服务器在境外。如果用户在中国大陆内，访问速度会比较慢，建议大陆用户配置国内的pip镜像源。

- 临时使用

```shell
pip3 install pandas -i https://mirrors.aliyun.com/pypip/simple
```

- 永久使用

1. 新建目录：`mkdir -p ~/.pip`
2. 新建配置文件并编辑：`vim ~/.pip/pip.conf`

```
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```