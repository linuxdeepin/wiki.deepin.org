---
title: Deepin-wine
description: 
published: true
date: 2022-05-07T07:47:22.065Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:31:28.645Z
---

# 如何用Deepin-wine安装运行win32的程序
## 创建容器
容器就是win32程序运行的环境，可以理解为一个极小的windows，在Linux下面实际对应一个文件目录，如QQ对应的容器目录是~/.deepinwine/Deepin-QQ。

创建容器最简单实用的方法就是将deepin维护的容器拷贝一份，如将QQ的容器拷贝一份到用户目录。cp -r ~/.deepinwine/Deepin-QQ ~/.bottle

创建一个干净的容器可以用如下命令：WINEPREFIX=~/.bottle deepin-wine winecfg 。但是这样可能会有一些字体乱码的问题。

## 运行程序
只通过deepin-wine *.exe 可以运行程序，但是默认通~/.wine的容器运行，~/.wine是wine默认生成的干净的容器，没有适配应用运行可能会有一些问题，所以最好通过上一步创建好的容器，可以每一个应用对应一个容器，不同的应用可能会需要不同的配置。

通过WINEPREFIX的环境变量可以指定容器运行程序。如WINEPREFIX=~/.bottle deepin-wine *.exe

## 简单debug
简单的分析程序运行出现的问题，可以打开deepin-wine输出日志的通道，通过WINEDEBUG环境开关。如 WINEDEBUG=+pid,+tid,+process WINEPREFIX=~/.bottle deepin-wine *.exe

# Deepin-wine应用全局快捷键设置
## 启动快捷键脚本
### 更新deepin-wine-helper
sudo apt-get update && sudo apt-get install deepin-wine-helper

更新到最新，/opt/deepinwine/tools/sendkeys.sh中有　$3 control mode , default ctrl+alt　这行注释就可以

### 确认需要设置快捷键的进程名和快捷键
如果不清楚需要设置的快捷键组合是什么，可以在设置中找到。如打开微信的快捷键是　ctrl+alt+W

进程名就是运行的exe的名字，可以用深度系统监视器查看。程序运行之后可以在监视器中找到对应的进程->右键菜单中选择属性->查看命令行的信息可以看出进程名。
如微信的进程名是:  WeChat

### 验证启动快捷键的脚本
启动快捷键是通过/opt/deepinwine/tools/sendkeys.sh脚本运行，第一个参数是快捷键的键值，目前只支持字母，第二个参数是进程名，第三个参数是控制键的组合。详细说明参考/opt/deepinwine/tools/sendkeys.sh的注释。如打开微信的快捷键就可以写成：　/opt/deepinwine/tools/sendkeys.sh w WeChat 4

程序运行的情况下，在终端运行脚本验证脚本是否有效。

## 添加自定义快捷键
在deepin的控制中心中添加自定义的快捷键。

名称：自己随意填

命令：填上面验证过的命令，如　/opt/deepinwine/tools/sendkeys.sh w WeChat 4

快捷键：输入自己方便的组合，不一定要和程序中设置的一致

已知问题，微信截图的快捷键　alt+a 没有效果，可以在微信中将截图的快捷键改为　Ctrl+a，对应的脚本命令就是　/opt/deepinwine/tools/sendkeys.sh a WeChat 2。
