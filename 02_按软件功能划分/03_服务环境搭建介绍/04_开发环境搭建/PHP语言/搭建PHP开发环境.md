---
title: 搭建PHP开发环境
description: 
published: true
date: 2022-10-25T06:01:40.658Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:32:13.485Z
---

# 简介
本词条意在说明如何快速简便地搭建php开发环境。

# 正文

XAMPP是一个把Apache网页服务器与PHP、Perl,phpMyAdmin及MariaDB集合在一起的安装包，允许用户可以在自己的电脑上轻易的建立网页服务器。  -- 百科


## 安装

下载地址： https://www.apachefriends.org

我们需要选择XAMPP for linux。此处以xampp-linux-x64-7.3.1-0-installer.run为例进行说明。

下载到本地后，打开安装包所在文件夹，在文件夹空白处右击鼠标选择在终端中打开。
然后在终端中执行以下命令：

        $ sudo ./xampp-linux-x64-7.3.1-0-installer.run
该命令执行后，xampp图形化安装程序便会启动，根据提示安装即可。

## 开启xampp

1）安装完成之后， 程序会自动运行，此时需要手动点击进入manage server界面，点击启动全部选项。

2）在Deepin系统中，XAMPP程序没有图标，故在日常使用时，需要通过命令行启动。命令行如下
    
  $ sudo ./opt/lampp/lampp start         启动
     
 $ sudo ./opt/lampp/lampp restart     重启
     
 $ sudo ./opt/lampp/lampp stop         停止

## 测试是否成功

打开浏览器，在地址栏输入localhost，能打开welcome to xampp网页

打开浏览器，在地址栏输入localhost、phpmyadmin，能打开phpmyadmin管理网页

以上两项均无问题则说明本次环境搭建成功

##如何使用该环境开发PHP程序

在确保XAMPP成功安装并已启动后，可使用命令行在指定文件夹创建php文件

   $ sudo touch /opt/lampp/htdocs/name.php

此处在opt/lampp/htdocs文件夹下创建了一个名为name的php程序

！！！需要注意的事，在opt/lampp/apache2文件夹下也有一个htdocs文件夹，在此文件夹下创建的php文件并不能在浏览器中打开。

！！！只有opt/lampp文件夹下的htdocs文件夹里的php文件能被浏览器打开

## PHP编辑器推荐

在Deepin系统中可以方便地使用Sublime Text编辑器进行php的编辑