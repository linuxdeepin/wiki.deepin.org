---
title: Linux或Windows上实现端口映射
description: 
published: true
date: 2023-06-14T08:37:17.263Z
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

### 实验拓扑

**内网**和**外网**是相对`Server4`来说的。
`Server1`和`Server2`为内网环境的两台服务器；
`Server3`为外网环境下的一台服务器；
`Server4`为一台双网卡主机，分别连接`192.168.50.0/24`和`172.16.2.0/24`两个网络。

### 配置实验环境

#### 1. Server1,2,3 上搭建 HTTP 服务

用 Python 在`Server1`上搭建一个简单的 HTTP 服务

![2023-6-14_96149.png](/2023-6-14_96149.png)

用 Python 在`Server1`上搭建一个简单的 HTTP 服务

```
cd ~
echo "server1" > index.html
python -m SimpleHTTPServer 8080
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/9aPYe0E1fb0OtXb4EYicMSkhMSkMlibiaBZj4h87b3ZZJXyq3YAEYnh5Zib0cIney2Sh1Goec6wF4OEK5XSfCOg8kw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

`Server2、Server3`同理

### 对照实验

在`client`上访问`Server1`的资源

```
curl http://192.168.50.11:8080/index.html
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/9aPYe0E1fb0OtXb4EYicMSkhMSkMlibiaBZXlGrqqS19zRWXt59A5oI3VibOOINbviaZr35yFD3OgHpdTcjlPfK4m0A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

在`client`上访问`Server2`的资源

```
curl http://192.168.50.12:8080/index.htm
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/9aPYe0E1fb0OtXb4EYicMSkhMSkMlibiaBZ9rpqNXjkRkAgWfEIpoPll0OHqNUwyhTa3nx2WIZxaK2YwxCYN0FsKg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

在`client`上访问`Server3`的资源

```
curl http://172.16.2.11:8080/index.html
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/9aPYe0E1fb0OtXb4EYicMSkhMSkMlibiaBZhaZh8GlYGY5wpp5ic4Yj9MdG9AqF8TSbI3aB05r4euZN5R8CgFKBZgg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

> 可以看到，外网的`client`是无法访问内网`Server1`,`Server2`的资源的。

### **在**`Server4`**上配置端口映射**

**临时配置**


