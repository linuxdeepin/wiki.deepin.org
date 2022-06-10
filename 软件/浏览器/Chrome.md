---
title: Chrome
description: 
published: true
date: 2022-06-10T06:08:41.673Z
tags: 浏览器, chrome
editor: markdown
dateCreated: 2022-04-21T03:30:37.500Z
---

## 简介

谷歌浏览器是一个由Google公司开发的网页浏览器，具有稳定、快速、安全、简洁、插件扩展等特点，其中包括稳定版(Stable)、开发版(Dev)、测试版(Beta)以及其他版本。Stable主要是为追求稳定的普通用户使用，一般更新最慢。

## 安装

`sudo apt-get install chrome-dde`  （stable）

`sudo apt-get install google-chrome-beta`  （beta）

`sudo apt-get install google-chrome-unstable`  （unstable）

## 卸载

`sudo apt-get remove chrome-dde`

`sudo apt-get remove google-chrome-beta`  （beta）

`sudo apt-get remove google-chrome-unstable`  （unstable）

## 仓库地址

[http://packages.deepin.com/deepin/pool/main/c/chrome-dde/](http://packages.deepin.com/deepin/pool/main/c/chrome-dde/)

[http://packages.deepin.com/deepin/pool/non-free/g/google-chrome-beta](http://packages.deepin.com/deepin/pool/non-free/g/google-chrome-beta)

[http://packages.deepin.com/deepin/pool/non-free/g/google-chrome-unstable](http://packages.deepin.com/deepin/pool/non-free/g/google-chrome-unstable)


## 常见问题


### 如何安装Flash插件

`sudo apt-get install libflashplugin-pepper`

###手动更新flashplayer

参见：https://bbs.deepin.org/forum.php?mod=viewthread&tid=143255&extra=

### 如何去掉密钥环提示

密钥环是linux系统用于安全保存程序私密数据的模块，可以用于加密保存密码、证书、密钥等安全数据。chrome的密钥环用于保存本地访问站点密码或缓存从google服务器同步下来的访问站点的密码。
Deepin系统的chrome会默认会把密码放在登录密钥环里，之所以会提示解锁登录密钥环是因为你的登陆密钥环被锁定了，只要把你的登陆密钥环解锁就可以了。

 安装seahorse

`sudo apt-get install seahorse`
 
运行seahorse

`seahorse`
 
解锁密码或修改密码环密码
密码->登录(Login)->右键解锁

另外一种情况
如果是chrome的安全数据没有存放于登录密钥环，那么有可能是创建了一个新叫“默认密钥环”的密钥环来存储。如想不每次打开电脑都输入的话就应该把它的密码设为空或者在登录密钥环上创建一个项目指向“默认密钥环”。

密码->默认密钥环->修改密码

## 相关链接