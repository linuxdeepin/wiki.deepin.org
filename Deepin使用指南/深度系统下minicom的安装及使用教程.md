---
title: 深度系统下minicom的安装及使用教程
description: 
published: true
date: 2022-05-17T11:36:28.594Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:50:36.421Z
---

# 简介

本经验由深度论坛用户(z0218)分享

[原文地址](https://bbs.deepin.org/forum.php?mod=viewthread&tid=134861)

Minicom先进系统（Minicom Advanced Systems）是一家Intel创投公司（Intel CapitalPortfolio），是针对带外服务器管理领域的数字和模拟KVM解决方案领商。另外，Minicom同时也是针对数字告示领域的音视频和串口信号分配系统主导厂商。

Minicom公司成立于1987年，总部位于以色列耶路撒冷，拥有一座3600平米的办公大楼，包括研发、市场、销售和物流等部门。此外Minicom在欧洲和美国具有区域总部，英国、意大利、法国和德国和中国具有销售支持队伍。在2005年，Minicom收购了Replicom公司，作为专注于IP技术的研发中心。历经20年的发展以及遍及全球50多个国家的业务，Minicom的产品已经销售到上千个数据中心，无论小公司还是大型跨国企业。 Linux下的Minicom的功能与下的超级终端功能相似，适于在通过超级终端对设备的管理以及对嵌入操作系统的升级。

# Minicom的安装

打开深度终端输入 `sudo apt install minicom && sudo minicom`

# Minicom的串行端口的设置

安装好配置`sudo minicom -s`进去把端口设置下波特率等

# Minicom的使用

## minicom界面介绍

第一次运行minicom，启动minicom要以root权限登录系统，需要进行minicom的设置，输入下了命令#minicom –s，显示的屏幕如下所示,按上下光标键进行上下移动选择，我们

要对串行端口进行设置，因此选中Serial port setup,然后回车：[configuration]

配置

│ Filenames and paths │//文件名和路径

│ File transfer protocols│//文件传输协议

│ Serial port setup │//串行端口设置

│ Modem and dialing │//调制解调器和拨号

│ Screen and keyboard │//屏幕和键盘

│ Save setup as dfl │//设置保存到

│ Save setup as.. │//储存设定为

│ Exit │//退出

│ Exit from Minicom │//退出minicom


## minicom的参数设置

选中设置串行端口，点击回车后，弹出设置的界面如下：点击‖A‖设置串行设置为/dev/ttyS0,这表示使用串口1（com1）,如果是/dev/ttyS1则表示使用串口2(com 2).按‖E‖键进

入设置‖bps/par/Bits‖（波特率）界面，如下图所示。再按‖I‖以设置波特率为115200，点‖F‖键硬件流控制设置为NO，回车最终的设置结果如下，然后回车返回到串口设置

主菜单中│A-Serial Device（串口设备）/dev/ttyS0 

│B-Lockfile Location（锁文件位置）: /var/lock 

│C-Callin Program（调入程序）

│D-Callout Program（调出程序）

│E-Bps/Par/Bits（）: 115200 8N1 

│F-Hardware Flow Control（硬件数据流控制） No 

│G-Software Flow Control（软件数据流控制） No

Change which setting? （改变这些设置）然后选中‖Save setup as dfl‖，按回车键保存刚才的设置。如下图所示：在选中‖EXit‖退出设置模式，刚才的设置保存到

‖/etc/minirc.dfl‖，接着进入初始化模式。

或可以这样设置，打开终端输入minicom后，初始化进入minicom的欢迎界面，这里提示按‖Ctrl+A‖,再按‖Z‖键进入主配置目录 

按下‖O‖键,并选择串口配置选项进行配置。接下来的配置是一样的。解析一下minicom命令摘要，命令将被执行当你按下Ctrl+D ,Key是对应的―字母‖键。“D”键：拨号目录

“S”键：发送文件，上传文件有几种方式：zmodem,ymodem、xmodem、kermit、ascii 

“P”键：通信参数。对波特率进行设置。

“L”键：捕捉开关。

“F”键：发送中断。

“T”键:终端设置。A-终端仿真：VT102终端B-Backspace键发送：DEL键C-状态一致：启动D-换行延迟（毫秒）:0 

“W”键：换行开关

“G”键：运行脚本

“R”键：接收文件

“A”键:添加一个换行符

“H”键：挂断

“M”键：初始化调制解调器

“K”键：运行kermit进行刷屏

“E”键：切换本地回显开关

“C”键:清除屏幕

“O”键：配置minicom 

“J”键:暂停minicom 

“X”键：退出和复位

“Q”键:退出没有复位

“I”键：光标模式

“Z”键：帮助屏幕

“B”键：滚动返回