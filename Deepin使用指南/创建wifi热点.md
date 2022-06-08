---
title: 创建wifi热点
description: 
published: true
date: 2022-05-07T07:49:27.609Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:46:49.270Z
---

## 采用github上的create_ap项目，此方法简单省事。

### 如何安装

如果安装了git,克隆安装create_ap

    git clone https://github.com/oblique/create_ap 
    cd create_ap 
    sudo make install 

如果提示没有安装git

    sudo apt-get install git

### 如何使用

此程序依赖hostapd,iptables,dnsmasq，如果没有安装，请先安装

    sudo apt-get install hostapd iptables dnsmasq

如果你的无线网卡名是wlan0,有线网卡名是eth0,你想创建的热点是myhost,密码是12345678

    sudo create_ap wlan0 eth0 myhost 12345678 #开启热点但是依赖终端，和下条命令二选其一
    sudo nohup create_ap wlan0 eth0 myhost 12345678 &  #开启热点并后台运行，可以关闭终端

如何查看网卡名

    ifconfig #冒号前的就是你的网卡名
    cat /proc/net/dev | awk '{if($2>0 && NR > 2) print substr($1, 0, index($1, ":") - 1)}' #这条命令会列出你的所有网卡名

### 如何卸载

如果你把项目文件删了，请再去下载一份，或者到[这里](https://github.com/oblique/create_ap/blob/master/Makefile)复制一份Makefile文件

    make uninstall

## 使用ap-hotspot,此方法需要添加ppa源

### 如何安装

首先添加ap-hotspot的ppa源

    sudo add-apt-repository ppa:nilarimogard/webupd8

如果提示未找到命令add-apt-repository,先安装python软件配置,然后再添加ppa

    sudo apt-get install python-software-properties software-properties-common

添加完ppa源之后

    sudo apt-get update
    sudo apt-get install ap-hotspot

### 如何使用

检查接口并配置热点

    sudo ap-hotspot configure #这里会要求你输入热点名ssid,和密码

开启热点

    sudo ap-hotspot start

### 如何卸载

终端执行命令

    sudo apt-get autoremove ap-hotspot #卸载ap-hotspot
    sudo add-apt-repository -r ppa:nilarimogard/webupd8 #删除ap-hotspot的ppa源
    #删除python软件配置(这一步不建议执行，以后可能用的到ppa)
    sudo apt-get autoremove python-software-properties software-properties-common
