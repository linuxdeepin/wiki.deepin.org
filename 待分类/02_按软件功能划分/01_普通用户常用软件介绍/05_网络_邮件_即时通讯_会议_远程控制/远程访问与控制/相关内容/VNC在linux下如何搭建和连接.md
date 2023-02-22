---
title: VNC在linux下的搭建和连接
description: 
published: true
date: 2023-02-22T02:53:34.874Z
tags: linux, vnc, 搭建, 连接
editor: markdown
dateCreated: 2023-02-21T10:39:54.286Z
---

# VNC在linux下的搭建和连接
### 服务端

1. 服务器上安装x11vnc，构建vnc的服务端：
`sudo apt-get install x11vnc`

1. 设置并存储vnc连接密码为deepin：
`sudo x11vnc -storepasswd deepin /etc/x11vnc.pass`

1. 启动 x11vnc服务端：
`sudo x11vnc -display :0 -auth /var/run/lightdm/root/:0 -forever -bg -o /var/log/x11vnc.log -rfbauth /etc/x11vnc.pass -shared -noxdamage -xrandr "resize" -rfbport 5900`

### 客户端

- 客户端安装并启动remmina：
`sudo apt-get install remmina -y`
`remmina`
- 选择VNC服务，直接输入服务端的IP，输入前面服务端设置的连接密码进行连接即可。
