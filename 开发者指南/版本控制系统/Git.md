---
title: Git
description: Version Control
published: true
date: 2022-06-12T11:58:36.698Z
tags: git
editor: markdown
dateCreated: 2022-05-05T10:13:28.481Z
---

# Git

## 简介

git是一个开源的分布式版本控制系统，可以有效、高速地处理从小到非常大的项目版本管理，和linux是“亲兄弟”，出自Linux Torvalds。

> 什么是版本控制？版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。

## 特点

- 性能优良。
- 分布式，绝大多数操作都是在本地执行
- Git使用哈希值保证数据的完整性
- 相较于SVN这样的集中式版本控制系统，Git的上手难度明显偏高

## 安装

```shell
sudo apt install -y git
# 或者安装所有模块
sudo apt install -y git-all
```

- 验证

```shell
git --version
```