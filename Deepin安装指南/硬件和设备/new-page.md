---
title: 双显卡笔记本安装N卡闭源驱动教程
description: 小白友好向
published: true
date: 2022-05-26T03:15:11.899Z
tags: 
editor: markdown
dateCreated: 2022-05-26T03:06:59.320Z
---

## 简介
感谢论坛用户bxkdhao提供的内容！
时间：2021年2月6日
原帖地址：https://bbs.deepin.org/zh/post/226463

## 详情

本人电脑硬件为神舟战神G7M-CT7NA 英特尔酷睿i7-9750H 

 

**1、在星火商店安装双显卡快速切换插件。**（注销后才能看到图标,但是不用注销，直接第二步)

星火商店网址 https://spark-app.store
![双显卡1.png](/图片存储/双显卡1.png)
 
**2、使用以下代码一键安装440驱动、显卡设置**
sudo apt install nvidia-driver nvidia-settings 
![双显卡2.png](/图片存储/双显卡2.png)
回车 确认

**3：在任务栏切换成nvidia显卡，自动注销后（注意保存资料），查看启动器中是否成功安装“NVIDIA X 服务器设置”；,或在psensor中查看是否有独显温度信息。**
![双显卡3.png](/图片存储/双显卡3.png)
 ![双显卡4.png](/图片存储/双显卡4.png)

**更高阶玩法：**

一个可以尝试的Nvidia Prime方案

https://bbs.deepin.org/zh/post/191741
