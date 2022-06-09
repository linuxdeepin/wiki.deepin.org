---
title: Firefox
description: 
published: true
date: 2022-06-09T05:32:37.690Z
tags: firefox, 浏览器
editor: markdown
dateCreated: 2022-04-21T03:34:19.832Z
---

## 简介

火狐浏览器是一个安全高效的浏览器，它具有速度快、隐私保护、丰富的插件资源、不同设备之间同步数据、分页浏览、个性化定制等特性。

## 安装

`sudo apt-get install firefox-dde` (深度桌面版)

`sudo apt-get install firefox`           

`sudo apt-get install firefox-esr`   (延长支持版)

## 卸载

`sudo apt-get remove firefox-dde`(深度桌面版)

`sudo apt-get remove firefox`          

`sudo apt-get remove firefox-esr`  (延长支持版)

## 仓库地址

[http://packages.deepin.com/deepin/pool/main/f/firefox-dde/](https://packages.deepin.com/deepin/pool/main/f/firefox-dde/) (深度桌面版)

[http://packages.deepin.com/deepin/pool/main/f/firefox-dde/](https://packages.deepin.com/deepin/pool/main/f/firefox/)

[http://packages.deepin.com/deepin/pool/main/f/firefox-dde/](https://packages.deepin.com/deepin/pool/main/f/firefox-esr)     (延长支持版)

## 常见问题

### 如何安装Flash插件

`sudo apt-get install libflashplugin`


### 如何安装中文语言包

`sudo apt-get install firefox-l10n-zh-cn`


###手动更新flashplayer插件

参见：https://bbs.deepin.org/forum.php?mod=viewthread&tid=143255&extra=

###更新flashplayer插件

https://get2.adobe.com/cn/flashplayer/otherversions/

下载 flash_player_npapi_linux.x86_64.tar.gz 

 tar.gz 插件包安装说明:

解压 tar.gz 插件包到合适的本地文件夹，你会看到：

/libflashplayer.so 

/usr/ 

在终端中进入上述本地文件夹，使用下列命令

    sudo cp libflashplayer.so /usr/lib/mozilla/plugin

    sudo cp -r usr/* /usr


###Firefox 使用PPAPI版 flashplayer

https://get2.adobe.com/cn/flashplayer/otherversions/  下载   flash_player_ppapi_linux.x86_64.tar.gz 

 tar.gz 插件包安装说明:

解压 tar.gz 插件包到合适的本地文件夹，你会看到：

/libpepflashplayer.so

在终端中进入上述本地文件夹，使用下列命令

    sudo mkdir /usr/lib/adobe-flashplugin

    sudo cp libpepflashplayer.so /usr/lib/adobe-flashplugin


推荐 freshplayerplugin -- 使 linux 下 firefox 能够使用 ppapi flash
https://www.v2ex.com/t/153629

因为mozilla 没打算支持ppapi的flash，于是有牛人做了一个ppapi to npapi 的转换器，于是firefox就可以在linux下直接使用最新版的flash啦！

项目地址 https://github.com/i-rinat/freshplayerplugin

###Firefox所有扩展突然被禁用
老版的firefox由于 AMO（Firefox 扩展中心）中间签名证书过期，可能导致 Firefox 认为这些扩展是未签名的，所以会被禁用.更新到最新版本就能恢复.


## 相关链接
[【已修复】Firefox所有扩展突然被禁用](https://mozilla.com.cn/thread-413298-1-1.html)
[火狐官网](https://www.firefox.com.cn/)
[火狐官方论坛](http://mozilla.com.cn/forum.php)