---
title: Deepin用Create_ap建热点辅助脚本21
description: 
published: true
date: 2022-05-11T15:29:49.625Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:32:41.541Z
---

## 简介

本经验由深度论坛用户(comzhong)分享，[原地址](https://bbs.deepin.org/forum.php?mod=viewthread&tid=132223&extra=)

使用您的网卡建立虚拟wifi热点，让您不用路由器也能用WiFi，好东西必须收藏喔。

## 正文

前面发过开热点的，写了个脚本，虽然比较简陋，算是勉强能用

1. 创建一个txt文本，将如下代码复制到正文，然后把名称修改为“create_ap.sh”，右键——属性中勾选”允许以程序执行“

```bash
#!/bin/bash
temfile=tmp.txt

wan=`sudo ifconfig|grep "^wl"|cut -d':' -f1`
lan=`sudo ifconfig|grep "^en"|cut -d':' -f1`
ppp=`sudo ifconfig|grep "^ppp"|cut -d':' -f1`
echo 
clear
echo 1.网络接口查询
echo 无线网卡接口：$wan
echo 有线网卡接口\：$lan
echo 宽带接口\：$ppp
echo 
echo 按任意键继续
read -n 1

#======================================
clear
sudo apt-get install ethtool
clear
echo 2.网卡及驱动信息
echo 网卡信息：
sudo lspci -vvnn | grep Network
echo 
echo 驱动信息：
sudo ethtool -i $wan|grep "^driver"
sudo ethtool -i $wan|grep "^version"
echo 
echo 下一行有“* AP”字样说明支持AP，如果没有，考虑是不是网卡驱动不对。
sudo iw list|grep -E "^* AP$"
echo 
echo 打开下面的网址，查看AP模式支持的驱动列表里的查找网卡芯片以及驱动，查看AP项为yes的，证明支持AP。
echo http://wireless.kernel.org/en/users/Drivers
echo 
echo 同一网卡芯片可能会有几个驱动可用，但某些版本的驱动可能不支持AP，如果当前驱动不支持，可以试试网卡芯片可用的其它驱动。
echo 
echo 如果确认支持AP，请按任意键继续安装hostapd和create_ap。
echo 
echo 按任意键继续
read -n 1
#======================================
clear
echo 安装hostapd……
sudo apt-get install hostapd dnsmasq haveged iptables

clear
echo 安装git……
sudo apt-get install git

clear
echo 安装create_ap……
git clone https://github.com/oblique/create_ap
cd create_ap
sudo make install

clear
echo 3.不可用信道查询
echo 建AP时不能用这些信道,若是先连wifi，再开AP，wifi也不能是这些信道：
sudo iw list|grep "(no IR)"|cut -d' ' -f4
sudo iw list|grep "(disabled)"|cut -d' ' -f4
echo 
echo 开启Wan--\>AP命令>../ap_create.sh
echo \sudo create_ap $wan $wan Deepin_AP 123456789 \&>>../ap_create.sh
echo >>../ap_create.sh
echo 开启Lan--\>AP命令>>../ap_create.sh
echo \sudo create_ap $wan $lan Deepin_AP 123456789 \&>>../ap_create.sh
echo >>../ap_create.sh
echo 开启ppp--\>AP命令>>../ap_create.sh
echo \sudo create_ap $wan $ppp Deepin_AP 123456789 \&>>../ap_create.sh
echo >>../ap_create.sh
echo 关闭AP命令>>../ap_create.sh
echo \sudo create_ap --stop $wan>>../ap_create.sh
echo  >>../ap_create.sh
cat ../ap_create.sh

echo 现在开始使用吧！按任意键查看create_ap示例
read -n 1

echo \## 示例>../ap_create_.sh
echo \### 无密码 \(开放网络\)\:>>../ap_create_.sh
echo     create_ap wlan0 eth0 MyAccessPoint>>../ap_create_.sh
echo >>../ap_create_.sh
echo \### WPA + WPA2 密码\:>>../ap_create_.sh
echo     create_ap wlan0 eth0 MyAccessPoint MyPassPhrase>>../ap_create_.sh
echo >>../ap_create_.sh
echo \### AP 无Internet访问\:>>../ap_create_.sh
echo     create_ap -n wlan0 MyAccessPoint MyPassPhrase>>../ap_create_.sh
echo >>../ap_create_.sh
echo \### 桥接共享 Internet\:>>../ap_create_.sh
echo     create_ap -m bridge wlan0 eth0 MyAccessPoint MyPassPhrase>>../ap_create_.sh
echo >>../ap_create_.sh
echo \### 桥接共享 Internet \(预配置桥接接口\)\:>>../ap_create_.sh
echo     create_ap -m bridge wlan0 br0 MyAccessPoint MyPassPhrase>>../ap_create_.sh
echo >>../ap_create_.sh
echo \### 同一WiFi接口共享 Internet\:>>../ap_create_.sh
echo     create_ap wlan0 wlan0 MyAccessPoint MyPassPhrase>>../ap_create_.sh
echo >>../ap_create_.sh
echo \### 选择不同的 WiFi 适配器驱动>>../ap_create_.sh
echo     create_ap --driver rtl871xdrv wlan0 eth0 MyAccessPoint MyPassPhrase>>../ap_create_.sh
echo >>../ap_create_.sh
echo \### 无密码 \(开放网络\) 使用 pipe\:>>../ap_create_.sh
echo     \echo -e \"MyAccessPoint\" \| create_ap wlan0 eth0>>../ap_create_.sh
echo >>../ap_create_.sh
echo \### WPA + WPA2 密码 使用 pipe\:>>../ap_create_.sh
echo     \echo -e \"MyAccessPoint\\nMyPassPhrase\" \| create_ap wlan0 eth0>>../ap_create_.sh
echo >>../ap_create_.sh
echo \### 启用 IEEE 802.11n>>../ap_create_.sh
echo     create_ap --ieee80211n --ht_capab \'[HT40+]\' wlan0 eth0 MyAccessPoint MyPassPhrase>>../ap_create_.sh
echo >>../ap_create_.sh
echo \### Client Isolation\:>>../ap_create_.sh
echo     create_ap --isolate-clients wlan0 eth0 MyAccessPoint MyPassPhrase>>../ap_create_.sh
echo >>../ap_create_.sh
echo \## 系统服务>>../ap_create_.sh
echo Using the persistent [systemd]\(https\:\/\/wiki.archlinux.org\/index.php\/systemd#Basic_systemctl_usage\) service>>../ap_create_.sh
echo >>../ap_create_.sh
echo \### 启动系统服务\:>>../ap_create_.sh
echo     systemctl start create_ap>>../ap_create_.sh
echo >>../ap_create_.sh
echo \### 开机自启\:>>../ap_create_.sh
echo     systemctl \enable create_ap>>../ap_create_.sh
echo >>../ap_create_.sh
echo \## License>>../ap_create_.sh
echo FreeBSD>>../ap_create_.sh
clear
cat ../ap_create_.sh
echo 任意键退出
read -n 1
```

2. 双击打开，选择“在终端中运行”
 
3. 网络接口

4. 网卡信息和驱动支持，查询驱动是否支持AP，有些网卡支持AP，但是某些驱动版本不支持，在终端中点击网址打开网站，查看驱动支持情况

5. 开AP时无线网卡不能使用的信道，正常信道一般是1~13中的1、6、13，我的笔记本是12、13、14不能用，也就是假如已经连WiFi是信道13，就开不了热点，需要切换到信道1或6.

6. 在终端中使用命令开启和关闭AP。

## 网络接口 和 网卡

网络接口 和 网卡 不一样，网卡是物理硬件，网络接口是虚拟的，当然，大多数时候一个网络接口对应一个网卡。

但是比如 宽带连接 就是一个虚拟网络接口，你用宽带连接上网，流量要走宽带连接这个网络接口才能访问互联网，VPN也是虚拟网络接口。

同样 AP也是一个 虚拟网络接口 ，在Windows 7 以上的系统，Microsoft Virtual WiFi Miniport Adapter那个就是，当然不是所有系统和网卡都支持AP，xp就不支持虚拟AP网络接口，有些网卡也不支持，也有可能是网卡驱动不支持。

## 常见问题

**用了有线网络接口上网，却选择共享无线网络接口**

刚开始 用的是 

```sudo create_ap wlp8s0b1 wlp8s0b1 Deepin_AP 123456789```

Wan-->AP 是连接无线网开AP

反复出现 错误

```
WARN: Your adapter does not fully support AP virtual interface, enabling --no-virt
ERROR: You can not share your connection from the same interface if you are using --no-virt option.
````

以为是没装成功 各种百度

后来才发现 是用错了命令

应该用这个  

```
### WPA + WPA2 密码:
create_ap wlan0 eth0 MyAccessPoint MyPassPhrase
```