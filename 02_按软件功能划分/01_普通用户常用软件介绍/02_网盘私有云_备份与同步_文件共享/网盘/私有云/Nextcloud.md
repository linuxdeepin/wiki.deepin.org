---
title: Nextcloud
description: 用于创建网络硬盘的客户端－服务器软件
published: true
date: 2022-10-21T06:53:53.811Z
tags: 
editor: markdown
dateCreated: 2022-07-13T15:28:29.683Z
---

## 简介
Nextcloud是一套用于创建网络硬盘的客户端－服务器软件。其功能与百度网盘相近，但Nextcloud是自由及开放源代码软件，每个人都可以在私人服务器上部署Nextcloud。
与百度网盘、阿里云盘等专有服务相比，Nextcloud的开放架构让用户可以利用应用程序的方式在服务器上新增额外的功能，并让用户可以完全掌控自己的数据。

![nextcloud.png](/nextcloud.png)

## 安装（客户端）
```
sudo apt install nextcloud-desktop
```

## 卸载（客户端）
```
sudo apt remove --purge nextcloud-desktop
```

## 常见问题
### 服务器端 Nextcloud 配置
请参考 https://nextcloud.com/install/#instructions-server
若使用 Docker 方式部署（推荐），请参考 https://github.com/nextcloud/all-in-one

## 仓库地址（客户端）
http://packages.deepin.com/deepin/pool/main/n/nextcloud-desktop/

## 相关链接
维基百科：https://zh.wikipedia.org/zh-cn/Nextcloud
Nextcloud 官网：https://nextcloud.com/
