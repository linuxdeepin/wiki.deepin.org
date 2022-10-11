---
title: KVM虚拟机使用问题
description: 介绍KVM虚拟机使用方法
published: true
date: 2022-10-11T01:26:33.860Z
tags: 
editor: markdown
dateCreated: 2022-06-23T06:45:39.928Z
---

# KVM虚拟机使用问题
## 安装KVM虚拟机
在终端中输入：`sudo apt-get install qemu-system-x86 libvirt-clients libvirt-daemon-system`;

安装完这些包以后，就需要将当前的用户添加到libvirt用户组。这样做的目的是为了，使当前用户可以直接管理虚拟机而不需要提权，在终端中输入：`sudo adduser deepin libvirt `  `#把deepin替换成自己的用户名`;

使用命令查看自己的用户是否可以管理虚拟机:`sudo virsh list --all`;

然后就可以安装图形管理工具来管理虚拟机了。管理kvm虚拟机，主要是通过使用一个叫做virt-manager的图形界面工具实现的。使用apt-get安装virt-manage:`sudo apt-get install virt-manager`;


## 配置桥接网络
1. 首先我们需要安装开启桥接接口所需的工具的软件包:`sudo apt-get install bridge-utils`;

2. 安装完以后，就可以使用brctl命令创建桥接接口并管理桥接接口:
`sudo brctl addbr br0   #创建一个桥接接口，名字叫br0`
`sudo brctl show        #输出系统上的所有桥接接口`
使用命令ip addr show应该就可以看到我们刚刚创建的那个桥接接口:
`4: br0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000 link/ether ac:1f:6b:ec:0f:1b brd ff:ff:ff:ff:ff:ff`

3. 然后将虚拟机的物理网卡接口加入到刚刚创建的br0桥接接口中:
`sudo brctl addif br0 enp0s25 #enp0s25替换成自己的网络接口的名称`

4. 删除物理网卡接口的ip地址，把物理网卡接口的ip地址配置到桥接接口上，并开启桥接接口。然后添加默认网关
`sudo ip addr del dev enp0s25 192.168.1.8/24`  #把接口替换成自己的接口，ip地址替换成自己的ip地址
`sudo ip addr add 192.168.1.8/24 dev br0`  #把ip地址替换成自己的ip地址
`sudo ip link set up br0`
`sudo route add default gw 192.168.1.1`  #把网关地址替换成自己的网关

5.在virt-manager中配置虚拟机的网络，将网络设置给刚刚创建的桥接接口，虚拟机就处于桥接模式了
`iptables -A FORWARD -p all -i br0 -j ACCEPT`	#开启转发

恢复原来的状态，只需要将桥接接口关闭，然后从桥接接口中删除物理网卡接口，即可
`sudo ip link set br0 down`
`sudo brctl delif br0 enp0s25`
`sudo ip link set enp0s25 down`
`sudo ip link set up enp0s25`

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

    
