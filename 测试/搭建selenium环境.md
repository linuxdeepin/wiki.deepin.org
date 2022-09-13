---
title: 搭建selenium环境
description: 介绍在deepin下搭建selenium环境
published: true
date: 2022-09-13T06:24:24.505Z
tags: 
editor: markdown
dateCreated: 2022-09-13T06:24:24.505Z
---

# 搭建selenium
## 安装pip3
在终端中输入：`sudo apt-get install python3-pip`

## 安装selenium
在终端中输入：`sudo pip3 install selenium`

## 下载并安装最新的驱动
在终端中输入：	`LATEST=$(wget -q -O - http://chromedriver.storage.googleapis.com/LATEST_RELEASE)`
在终端中输入：`wget http://chromedriver.storage.googleapis.com/$LATEST/chromedriver_linux64.zip`

## 解压chromedriver
在终端中输入：`unzip chromedriver_linux64.zip`

## 将chromedriver放置至/usr/local/bin/
在终端中输入：`sudo -S mv chromedriver /usr/local/bin/`

## 下载谷歌浏览器
https://www.google.cn/intl/en_uk/chrome/browser-tools/

## 检查运行情况
```
ThinKinG@ThinKinG-PC:~$ python3
Python 3.7.3 (default, Mar  9 2022, 03:38:16) 
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from selenium import webdriver
>>> d = webdriver.Chrome()
>>> d.get("https://wiki.python.org/moin/DbusExamples")
>>> 
```

