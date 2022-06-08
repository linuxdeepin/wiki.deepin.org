---
title: Virtualbox_虚拟机中网络安装在LINUX下共享的打印机方法
description: 
published: true
date: 2022-05-07T07:48:28.372Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:44:21.304Z
---

# 简介

本经验由深度论坛用户(leixiaozeng)分享

[原文地址](https://bbs.deepin.org/forum.php?mod=viewthread&tid=133743)

# 正文

今天打文档的时候，才发现，deepin下wps的字体呀，简直了，我没法用呀，还好，我机智前段时间用virtublbox装了一个WIN7，遂，打开虚拟机，两下就把文档给编辑好了，正要打印才发现，天，既然没装打印机，可是文档又急着要，没办法我把做好的文件拷在我的双系统的WINDOWS下先打印，把差事给解决了，回过神来，我就开始研究着在VIRTUALBOX下面安装打印机的方法。为了让其它朋友少一些折腾，给大家分享一下。

## 安装CUPS

默认情况下DEEPIN是安装了的，就不用安装了，没有安装的用新立得，或者命令安装 sudo apt-get install cups

## 配置共享

两个方法

1. 利用网页
    用网页打开 <https://192.168.1.3:631/admin> 当然IP地址要替换成你的主机的IP
    把其中的 Share printers connected to this system 和 Allow printing from the Internet 给勾选上

2. 利用打印机
    打开打印机设置
    点击 服务器——设置  勾选下图两个选项
    ![图片](https://storage.deepin.org/forum/201701/04/130506jyh5oa87b6i54q34.png)
    记得打印机右键的共享也要勾选上。这个我截不了图。

---

好上面的事情完成之后，那在主机上的配置就完事了，打开你的虚拟机吧。

一、虚拟机的网络吧要设置成桥接网卡

二、在虚拟机当中输入 <http://192.168.1.3:631/printers> IP地址要换成你自己的IP
  
在上面的界面选择你要连接的打印机
  
这个时候你就得到了你打印机的网络地址 <http://192.168.1.3:631/printers/HP_LaserJet_Pro_MFP_M128fn>

## 添加打印机

在控制面板里打开设备和打印机界面
  
点击 添加打印机
  
选择第二个添加网络打印机
  
不要等它搜索，是搜索不到的，点击下面的，我需要的打印机不在列表中（R）
  
选择按名称选择共享打印机，在里面复制你刚刚得到的网络地址再点下一步。
  
这里选择你的驱动进行安装，如果没有，那就自己官网下载驱动吧。安装完成之后就能直接用了。

---

其实吧，virtualbox真的麻烦如果用vmware的可以直接识别，就像U盘也是一样，vmware可以直接识别，但是virtualbox却要安装什么增强包。或许是我刚开始用virtualbox不习惯吧，显卡方面virtualbox也与vmware不一样，唉，又发牢骚了。
