---
title: 开启wayland切换通道
description: 
published: true
date: 2022-06-24T06:55:30.869Z
tags: deepin, wayland, x11
editor: markdown
dateCreated: 2022-06-24T06:55:30.869Z
---

# deepin开启wayland切换通道
本文通过配置策略工具开启wayland切换通道
1.  进入终端下载配置策略工具，命令如下：
```linux
apt download dde-dconfig-editor
````
![1.png](/for_trans/wayland/1.png)
2.  执行如下命令解压工具 deb 包：
 ```linux
 dpkg -x dde-dconfig-editor_0.0.5.1-1_amd64.deb [解压目录]
 ```
示例：
`dpkg -x dde-dconfig-editor_0.0.5.1-1_amd64.deb /home/qqq/Desktop/`
![2.png](/for_trans/wayland/2.png)
3. 通过终端进入配置策略工具配置页面：
![3.png](/for_trans/wayland/3.png)
4. 在 greeter 配置中开启 wayland 切换配置项：
![4.png](/for_trans/wayland/4.png)
5. 注销后通过登录页面右下角按钮切换 wayland：
![5.jpg](/for_trans/wayland/5.jpg)