---
title: 使用KVM虚拟机并配置桥接网络
description: 
published: true
date: 2022-06-23T03:44:59.989Z
tags: 
editor: markdown
dateCreated: 2022-06-23T03:44:57.861Z
---

# 使用KVM虚拟机并配置桥接网络
在deepin下使用虚拟机其实有很多的解决方案。比如VirtualBox。主要是想尝试下使用其他的虚拟机软件。在这里我们来使用kvm。在deepin下使用kvm其实很方便，有一个现成的kvm图形管理器叫"virt-manager"。可以像其他的虚拟机如VirtualBox，VMWare Workstation一样管理虚拟机。
## 使用KVM虚拟机
首先我们来安装所需要的软件包。
```
sudo apt-get install qemu-kvm libvirt-clients libvirt-daemon-system
```
安装完这些包以后，就需要将当前的用户添加到libvirt用户组。这样做的目的是为了，使当前用户可以直接管理虚拟机而不需要提权
```
sudo adduser uos libvirt    #把uos替换成自己的用户名
```
使用命令查看自己的用户是否可以管理虚拟机。
```
sudo virsh list --all
```
然后就可以安装图形管理工具来管理虚拟机了。管理kvm虚拟机，主要是通过使用一个叫做virt-manager的图形界面工具实现的。使用apt-get安装virt-manager
```
sudo apt-get install virt-manager
```
到这里，就可以通过图形界面工具管理虚拟机了。安装完virt-manager之后应该可以在应用程序菜单找到它。接下来创建虚拟机

## 配置桥接网络
接下来讲，如何将kvm虚拟机桥接到物理网络。

1.首先我们需要安装开启桥接接口所需的工具的软件包
```
sudo apt-get install bridge-utils
```
2.安装完以后，就可以使用brctl命令创建桥接接口并管理桥接接口
```
sudo brctl addbr br0   #创建一个桥接接口，名字叫br0
sudo brctl show        #输出系统上的所有桥接接口
```
3.然后将虚拟机的物理网卡接口加入到刚刚创建的br0桥接接口中
```
sudo brctl addif br0 enp0s25 #enp0s25替换成自己的网络接口的名称
```
4.通过执行命令sudo brctl show就可以看到enp0s25,已经加入br0中了
```
bridge name    bridge id            STP enabled    interfaces
br0            8000.f0def11b0be2    no             enp0s25
```
5.删除物理网卡接口的ip地址，把物理网卡接口的ip地址配置到桥接接口上，并开启桥接接口。然后添加默认网关
```
sudo ip addr del dev enp0s25 192.168.1.8/24 #把接口替换成自己的接口，ip地址替换成自己的ip地址
sudo ip addr add 192.168.1.8/24 dev br0     #把ip地址替换成自己的ip地址
sudo ip link set up br0
sudo route add default gw 192.168.1.1    #把网关地址替换成自己的网关
```
6.这时候在virt-manager中配置虚拟机的网络，将网络设置给刚刚创建的桥接接口，虚拟机就处于桥接模式了
![虚拟网络接口.png](/虚拟网络接口.png)
```
iptables -A FORWARD -p all -i br0 -j ACCEPT	#开启转发
```
恢复原来的状态，只需要将桥接接口关闭，然后从桥接接口中删除物理网卡接口，即可
```
sudo ip link set br0 down
sudo brctl delif br0 enp0s25
sudo ip link set enp0s25 down
sudo ip link set up enp0s25 #重启物理网卡
```
桥接网卡配置信息为:
```
auto lo
iface lo inet loopback

auto br0
iface br0 inet static
bridge_ports enp5s0f0
address 10.8.10.12
netmask 255.255.255.0
gateway 10.8.10.1
bridge_stp off
bridge_fd 0
bridge_maxwait 0
```
