---
title: Deepin上使用惠普打印机
description: 
published: true
date: 2022-05-07T07:47:22.158Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:32:19.614Z
---

## 简介

本经验由深度官方人员分享。

## 正文

如何在Deepin系统上使用打印机？
其实只需要简单配置一下就可以。具体方法如下：

1. 首先在启动菜单里找到打印设置
   如果图标太多，可以搜索【打印】就可以了。

2. 打开打印设置，点击添加按钮

3. 如果第一次连接网络打印机，可以通过查找网络打印机，输入打印机的ip地址进行查找。如果查找到，就会在列表里显示该打印机的型号。这里我已经添加了HP Laser Jet Professional M1213nf型号的打印机。

4. 选择该打印机，在右边选择使用HPLIP进行连接

5. 正常情况下，可以识别该打印机，点击应用即可添加打印机。

6. 提示是否打印一个测试页，如果要测试是否可以用，请打印一个测试页。

7. 在配置过程中，会发现打印测试页失败的情况。Deepin系统会默认安装hplip的组件，但是缺少hp-plugin。所以需要安装此hp-plugin。

方法是：

打开终端（DeepinTerminal），输入`hp-plugin`。在终端的输出中会显示版本号。

```
deepin@deepin-PC:~/Downloads$ hp-plugin 

HP Linux Imaging and Printing System (ver. 3.16.10)
Plugin Download and Install Utility ver. 2.1

Copyright (c) 2001-15 HP Development Company, LP
This software comes with ABSOLUTELY NO WARRANTY.
This is free software, and you are welcome to distribute it
under certain conditions. See COPYING file for more details.
```

如果使用hp-plugin这个命令在线安装，往往会因为网络问题，下载失败。建议手动下载plugin。

下载地址：
<https://www.openprinting.org/download/printdriver/auxfiles/HP/plugins/>
该网站会有很多版本提供下载。根据上面看到的版本，我们需要下载3.16.10的版本（就是上面括号中给出的提示ver.3.16.X，请注意根据这个提示下载对应版本）。主要下载的文件为：
hplip-3.16.10-plugin.run
hplip-3.16.10-plugin.run.asc
用系统默认的谷歌浏览器打开下载页面的后，具体的下载方法是找到正确的文件后右键---链接另存为  然后选择下载后保存的位置

下载完成后，可以使用`hp-plugin`进行离线安装。安装完成后之后再测试是否可以打印测试页。
离线安装是指，在终端中，执行hp-plugin命令，根据提示选择要使用本地文件。或者在终端中执行`hp-setup`命令，根据提示选择你下载的plugin文件路径。
注意执行上述命令是需要root密码，但是deepin默认的root密码是没有的，因此你必须重新开一个新的终端，先设置root密码之后才能继续，设置root密码的具体操作是 `sudo passwd` 提示你输入当前账户密码，正确输入后就提示你输入unix密码 这个就是root 密码，输入后请务必记得root密码哦 ，之后在回到安装驱动页面，输入root密码即可正常安装
