---
title: dpkg
description: 软件包打包
published: true
date: 2022-10-25T04:41:18.692Z
tags: 打包
editor: markdown
dateCreated: 2022-05-05T10:18:40.049Z
---

# 打包deb
打包debain软件
## 打包
首先认识结构，一个软件包有/DEBIAN，/etc，/usr，/opt等目录，接下来依次讲解每个目录的作用
/DEBIAN
## 打包命令
    	dpkg -b 目录 -xxx.deb
## 安装并测试软件包
打包完成后，你需要安装并测试你的软件包，可以使用命令安装，或者使用深度包管理器

    	dpkg -i xxx.deb
如果没有顺利安装，可能是存在依赖问题，请查看软件包的/DEBIAN/control中填写的依赖