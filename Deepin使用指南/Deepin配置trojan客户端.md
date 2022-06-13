---
title: Deepin配置trojan客户端
description: 
published: true
date: 2022-05-07T07:47:22.188Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:32:58.830Z
---

# 安装trojan服务 #
sudo apt install trojan
# 将例子中的客户端配置文件移动到trojan配置服务下 #
mv /usr/share/doc/trojan/examples/client.json-example /etc/trojan/config.json
#修改配置文件#
sudo dedit /etc/trojan/config.josn
```json
  ......
  "remote_addr": "example.com",
  "password": [
    "password1"
  ],
  "ssl": {
    "verify": false
  ......
```

我填写的flase，不是最正确的做法，但是方便，具体查看<https://github.com/trojan-gfw/trojan/issues/362>

# 启动 #
systemctl start trojan.service

# 关于Trojan-Qt5图形界面客户端 #
trojan是有图形界面客户端的，<https://github.com/Trojan-Qt5/Trojan-Qt5>。
不过我当时用的版本早，使用一段时间后速度明显缓慢，不清楚现在效果如何。

# 说明 #
在deepin上使用trojan客户端，服务端的设置类似本篇。本篇由论坛用户(houyawei)分享
