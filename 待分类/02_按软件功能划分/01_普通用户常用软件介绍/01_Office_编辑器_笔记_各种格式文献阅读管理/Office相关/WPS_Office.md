---
title: WPS Office
description: 
published: true
date: 2023-02-22T08:59:26.180Z
tags: office
editor: markdown
dateCreated: 2022-04-21T03:44:33.547Z
---

## 简介

WPS Office是由金山软件股份有限公司自主研发的一款办公软件套件，可以实现办公最常用的文字、表格、演示等多种功能。免费提供海量的在线存储空间及文档模板、支持阅读和输出PDF文件、全面兼容Microsoft Office 97-2010格式。

## 安装

`sudo apt-get install wps-office`

## 卸载

`sudo apt-get remove wps-office`

## 仓库地址

[http://packages.deepin.com/deepin/pool/non-free/w/wps-office/](http://packages.deepin.com/deepin/pool/non-free/w/wps-office/)


## 常见问题

1. 排版错乱
这是在Deepin系统中使用WPS最常遇到的问题，现象通常为在Deepin端的WPS中查阅在Windows端编辑完成的文档，文档中的每一行只显示一半，就像叠加在一起一样。出现这种情况是因为Deepin是一个开源社区操作系统，而WPS中常用字体则大多需要商业授权，Deepin无法将其集成到系统内，Deepin端WPS缺失相应的字体，从而导致文档排版错乱。
解决方法是通过Deepin的字体管理器安装相应的字体，更方便的方法是从星火商店下载`Win字体`软件，安装该软件可以将Windows中默认字体全部安装到Deepin中。

2. 使用卡顿
这不一定是WPS的问题，但如果你使用其他软件都很流畅，可唯独使用WPS非常卡顿的话，在WPS的`设置-其他-切换窗口管理模式`中将`整合模式`改为`多组件模式`说不定能解决你的问题

## 相关链接

维基百科：