---
title: sudo命令反应慢
description: 终端使用sudo命令时输入密码的提示出现的速度变慢
published: true
date: 2023-03-01T06:34:40.360Z
tags: 
editor: markdown
dateCreated: 2023-03-01T06:34:40.360Z
---

# sudo命令响应变慢
### 问题描述
用户在论坛中提出输入sudo命令后，输入密码的提示出现的速度变慢，相关的帖子链接：[sudo的反应变慢了，不知道为什么](/zh/https://bbs.deepin.org/post/253158)
### 解决办法
论坛用户[donaldsebleung](/zh/https://bbs.deepin.org/user/287133)提供的以下解决方案：
- 修改/etc/hosts文件，使用命令`sudo vi /etc/hosts`
- 文件中添加127.0.1.1 host_name（其中host_name为用户名），保存退出即可
> 借鉴文档[Network setup](/zh/https://www.debian.org/doc/manuals/debian-reference/ch05.en.html#_the_hostname_resolution)