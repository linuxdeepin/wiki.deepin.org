---
title: Linux或Windows上实现端口映射
description: 
published: true
date: 2023-06-14T08:35:13.053Z
tags: 
editor: markdown
dateCreated: 2023-06-14T08:33:55.386Z
---

> 通常服务器会有许多块网卡，因此也可能会连接到不同的网络，在隔离的网络中，某些服务可能会需要进行通信，此时服务器经过配置就可以承担起了转发数据包的功能。

### 一、Windows 下实现端口映射

**1. 查询端口映射情况**

```
netsh interface portproxy show v4tov4
```

**2. 查询某一个 IP 的所有端口映射情况**

```
netsh interface portproxy show v4tov4 | find "[IP]"
例：
netsh interface portproxy show v4tov4 | find "192.168.1.1"
```

**3. 增加一个端口映射**

```
netsh interface portproxy add v4tov4 listenaddress=[外网IP] listenport=[外网端口] connectaddress=[内网IP] connectport=[内网端口]
例：
netsh interface portproxy add v4tov4 listenaddress=2.2.2.2 listenport=8080 connectaddress=192.168.1.50 connectport=80
```

**4. 删除一个端口映射**

```
netsh interface portproxy delete v4tov4 listenaddress=[外网IP] listenport=[外网端口]
例：
netsh interface portproxy delete v4tov4 listenaddress=2.2.2.2 listenport=8080
```

### 二、Linux 下端口映射

**1. 允许数据包转发**

```
echo 1 >/proc/sys/net/ipv4/ip_forward
iptables -t nat -A POSTROUTING -j MASQUERADE
iptables -A FORWARD -i [内网网卡名称] -j ACCEPT
iptables -t nat -A POSTROUTING -s [内网网段] -o [外网网卡名称] -j MASQUERADE
例：
echo 1 >/proc/sys/net/ipv4/ip_forward
iptables -t nat -A POSTROUTING -j MASQUERADE
iptables -A FORWARD -i ens33 -j ACCEPT
iptables -t nat -A POSTROUTING -s 192.168.50.0/24 -o ens37 -j MASQUERADE
```

**2. 设置端口映射**

```
iptables -t nat -A PREROUTING -p tcp -m tcp --dport [外网端口] -j DNAT --to-destination [内网地址]:[内网端口]
例：
iptables -t nat -A PREROUTING -p tcp -m tcp --dport 6080 -j DNAT --to-destination 10.0.0.100:6090
```

### 实验：将部署在内网的服务映射到外网

### 实验环境

1. VMWare Workstation Pro
2. 5 台最小化安装的 centos 7 虚拟机