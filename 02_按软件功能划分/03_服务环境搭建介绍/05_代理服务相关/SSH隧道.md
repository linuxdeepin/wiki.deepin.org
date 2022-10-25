---
title: SSH隧道
description: 使用SSH隧道进行代理
published: true
date: 2022-06-16T13:10:26.134Z
tags: socks5, proxy, ssh, 代理
editor: markdown
dateCreated: 2022-06-16T13:09:22.414Z
---

# SSH动态转发代理

### 启动代理服务器
master@neko:~$`ssh -D 1080 root@99.99.99.99` #该命令将会连接服务器并在本机创建端口为1080的socks5代理服务器。

#### 终端测试：
本机IP：1.1.1.1
服务器IP：99.99.99.99

---

master@neko:~$`curl ip.sb`
1.1.1.1

master@neko:~$`export all_proxy=socks5h://0:1080` #启用代理(仅限当前终端窗口)

master@neko:~$`curl ip.sb`
99.99.99.99
#### 浏览器测试：
浏览器安装`Proxy SwitchyOmega`插件
配置代理协议为：socks5
服务器地址：127.0.0.1
端口:1080

插件设置为启用状态，访问`https://ip.sb`此时显示ip应为99.99.99.99