---
title: 系统无法扫描WiFi列表，该如何解决？
description: 该问题一般是未安装网卡驱动导致的
published: true
date: 2022-06-28T06:05:43.597Z
tags: 无法扫描wifi, 网卡驱动
editor: markdown
dateCreated: 2022-06-28T06:00:26.369Z
---

# 系统无法扫描WiFi列表，该如何解决？
1. 在终端中执行命令重启无线网卡服务
```linux
systemctl restart network-manager.service
```
![1.jpg](/for_trans/wifi列表/1.jpg)
2. 如果发现是没有网卡驱动，就需要安装无线网卡驱动，执行命令查看无线网卡型号。
```linux
lspci | grep -i ethernet 
```
![2.jpg](/for_trans/wifi列表/2.jpg)
3. 如果查到是Realtek无线网卡，可以访问[https://github.com/lwfinger/rtlwifi_new](https://github.com/lwfinger/rtlwifi_new)网站获取对应型号的驱动并下载。如果是其他型号的无线网卡，则需要自己去查找对应的驱动。
4. 解压缩下载的驱动文件，并找到rtlwifi_new-master的文件夹。
5. 进入rtlwifi_new-master的文件夹，单击右键并选择【在终端中打开】，然后输入以下命令。
```linux
make
sudo make install
sudo modprobe <无线网卡型号>
```
6. 重启系统。

本篇wiki的原文链接为[https://www.deepinos.org/d/102-deepin](https://www.deepinos.org/d/102-deepin)
同时可参考论坛帖子[https://bbs.deepin.org/forum.php?mod=viewthread&tid=153154](https://bbs.deepin.org/forum.php?mod=viewthread&tid=153154)