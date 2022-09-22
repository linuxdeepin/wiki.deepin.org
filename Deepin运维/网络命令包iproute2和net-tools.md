---
title: iproute2 vs net-tools
description: 
published: true
date: 2022-07-23T15:10:42.749Z
tags: 
editor: markdown
dateCreated: 2022-07-22T04:35:55.227Z
---

# iproute2 vs net-tools
本文来自于 https://linoxide.com/iproute2-vs-net-tools/

Linux查询网络相关的命令工具主要有两个包，iproute2和net-tools。

iproute2在Linux基金会的百科网址：https://wiki.linuxfoundation.org/networking/iproute2
net-tools的官网：https://net-tools.sourceforge.io/


|  iproute2   | net-tools  |  功能 |
|  ----  | ----  | ---- |
| ip link show  | ifconfig -a | 显示所有接口
| ip link set down/up eth0  | ifconfig eth0 up/down | 启用 (UP)/禁用 (DOWN) 网络接口
| ip addr add 192.168.0.10/24 dev eth0 | ifconfig eth0 192.168.0.10/24 | 为网络接口分配 IPv4 地址 |
| ip addr del 192.168.0.10/24 dev eth0 | ifconfig eth0 0 | 	从网络接口中删除 IPv4 地址 |
| ip addr show dev eth0 | ifconfig eth0 | 	显示网络接口的 IPv4 地址 |
| ip -6 addr add fe80::f0b7:57ff:fe2f:5f0d/64 dev eth1 | ifconfig eth1 inet6 add fe80::f0b7:57ff:fe2f:5f0d/64 | 	将 IPv6 地址分配给网络接口 |
| ip -6 addr show dev eth0 | ifconfig eth0 | 	显示网络接口的 IPv6 地址 |
| ip link set dev eth0 address 02:42:20:d2:28:36 | ifconfig eth0 hw ether 02:42:20:d2:28:36 | 	更改网络接口的 MAC 地址 |
| ip route show | route -n | 	显示 IP 路由表 |
| ip route add default via 192.168.0.1 dev eth0 | route add default gw 192.168.0.1 eth0 | 添加默认路由 |
| ip route replace default via 192.168.0.1 dev enp0s3 | route del default gw 192.168.0.1 enp0s3 | 删除默认路由 |
| ip route add 10.24.32.0/24 via 192.168.0.1 dev enp0s3 | route add -net 10.24.32.0/24 gw 192.168.0.1 dev enp0s3 | 添加静态路由 |
| ip route del 192.168.10.0/24 | route del -net 192.168.10.0/24 | 删除静态路由 |
| ss | netstat | 显示套接字 - 监听 tcp/udp |
| arp -an  | ip neigh | 显示 ARP 表 |
| bridge  | 	brctl | 管理网桥地址和设备 |

