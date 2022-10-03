---
title: 浅谈DDE桌面运行机制
description: 
published: true
date: 2022-10-03T18:21:17.803Z
tags: dde桌面环境
editor: markdown
dateCreated: 2022-06-27T08:36:23.655Z
---

# 浅谈DDE桌面运行机制  


## 1.桌面环境的理解  

  
1. 桌面环境是一些组件的组合体，为你提供图形用户界面，通俗点讲，就是在内核和显示服务器上，写一批程序让用户登录后就可以像在 Windows 中一样使用鼠标和键盘来使用 Linux系统

2. 如果没有桌面环境，你就只能用命令与你的 Linux 系统进行交互
 
3. 桌面环境决定了你的 Linux 系统的样子以及你与它的交互方式

4. 常见桌面环境有：DDE，KDE，GNOME，Xfce、Unity、Cinnamon、MATE、Lxde等

## 2. DDE桌面架构

![桌面架构.png](/桌面架构.png)

![systemd.png](/systemd.png)

![sys.png](/sys.png)

![lightdm.png](/lightdm.png)

![wayland.png](/wayland.png)

![dde.png](/dde.png)

## 3.DDE桌面启动流程

![启动流程.png](/启动流程.png)

![启动流程2.png](/启动流程2.png)

## 4.DDE桌面组件运行机制

DDE桌面组件认识，任务栏和启动器组件程序的加载和启动

![dde前端组件.png](/dde前端组件.png)

![dde后端组件.png](/dde后端组件.png)

### **任务栏应用程序加载**

![任务栏.png](/任务栏.png)


固定区域，托盘区域和插件区域的代码都在dock项目里，功能由dock直接维护

应用程序的加载主要依赖后台服务: com.deepin.daemon.Dock，其中右键菜单，打开，激活等功能都是调用后台服务接口实现

打开d-feet，在Session Bus中搜索daemon.dock

![任务栏应用程序加载.png](/任务栏应用程序加载.png)

### **任务栏应用程序启动**

以启动“控制中心”为例，我们先来看看有几种方法：

1. 在任务栏左键点击或右键打开

2. 桌面打开终端后，在终端中输入命令：

`dde-control-center -s`

3. 桌面打开终端后，在终端中输入命令：
`
dbus-send --session --type=method_call --print-reply --dest=com.deepin.dde.ControlCenter /com/deepin/dde/ControlCenter com.deepin.dde.ControlCenter.Show`  

  
4. 打开d-feet，搜索ControlCenter，打开com.deepin.dde.ControlCenter

![启动.png](/启动.png)

启动器软件列表加载

启动器软件列表的加载主要依赖后台服务: com.deepin.daemon.Launcher

所有安装的软件都会有一个.desktop文件，记录了软件基本的信息，该文件在安装后会
放在目录/usr/share/applications下，后台服务通过扫描该目录下所有的.desktop文件，
来提供安装软件列表给启动器展示


![ls.png](/ls.png)

通过pstree查看，可发现所有由Launcher启动的程序，父进程都是startdde
可以得知道启动器都是由startdde调用接口启动软件的

任务栏点击启动器，launcher的父进程总是为systemd，并不是startdde?

因为在代码中，启动launcher的方式就是直接通过launcher注册的dbus服务，调用show函数，类似与dbus-send的方式

## 5.日志查看

### 1. 组件日志查看
```
Lightdm: /var/log/lightdm/lightdm.log
lightdm-deepin-greeter: /var/log/lightdm/seat0-greeter.log
startdde: journalctl –b /usr/bin/startdde
dde-session-daemon: journalctl –b /usr/bin/dde-session-daemon
dde-dock: ~/.cache/deepin/dde-dock/dde-dock.log
dde-control-center: ~/.cache/deepin/dde-control-center/dde-control-center.log
dde-launcher: ~/.cache/deepin/dde-launcher/dde-launcher.log
dde-lock: ~/.cache/deepin/dde-lock/dde-lock.log
dde-shutdown: ~/.cache/deepin/dde-shutdown/dde-shutdown.log
dde-kwin:~/.kwin.log



```
