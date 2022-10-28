---
title: Mysql
description: 
published: true
date: 2022-10-25T04:49:31.050Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:38:44.986Z
---

## 下载

打开网页 <https://dev.mysql.com/downloads/repo/apt/> 点击 Download 按钮，在弹出的界面中点击登录按钮或者 “No thanks, just start my download.” 链接。

## 运行

````
sudo dpkg -i mysql-apt-config_0.8.15-1_all.deb
````

在弹出的界面中选择对应的debian版本和mysql版本，v15.11用的是 debian 9 （Stretch），v20用的是 debian10（Buster）。

## 更新

````
sudo apt update
````

## 安装

````
sudo apt install mysql-server
````

在弹出的界面中输入mysql的root密码。
