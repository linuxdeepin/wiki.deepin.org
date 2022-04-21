---
title: VPN服务
description: 
published: true
date: 2022-04-21T03:44:08.244Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:44:05.257Z
---

[[en:VPN_service]]


##简介

虚拟专用网（英语：Virtual Private Network，简称VPN），是一种常用于连接中、大型企业或团体与团体间的私人网络的通讯方法。虚拟私人网络的讯息透过公用的网络架构（例如：互联网）来传送内联网的网络讯息。它利用已加密的通道协议（Tunneling Protocol）来达到保密、传送端认证、信息准确性等私人信息安全效果。这种技术可以用不安全的网络（例如：互联网）来传送可靠、安全的信息。需要注意的是，加密信息与否是可以控制的。没有加密的虚拟专用网信息依然有被窃取的危险。

以日常生活的例子来比喻，虚拟专用网就像：甲公司某部门的A想寄信去乙公司某部门的B。A已知B的地址及部门，但公司与公司之间的信不能注明部门名称。于是，A请自己的秘书把指定B所属部门的信（A可以选择是否以密码与B通信）放在寄去乙公司地址的大信封中。当乙公司的秘书收到从甲公司寄到乙公司的信件后，该秘书便会把放在该大信封内的指定部门信件以公司内部邮件方式寄给B。同样地，B会以同样的方式回信给A。

在以上例子中，A及B是身处不同公司（内部网路）的计算机（或相关机器），通过一般邮寄方式（公用网络）寄信给对方，再由对方的秘书（例如：支持虚拟专用网的路由器或防火墙）以公司内部信件（内部网络）的方式寄至对方本人。请注意，在虚拟专用网中，因应网络架构，秘书及收信人可以是同一人。许多现在的操作系统，例如Windows及Linux等因应所用传输协议，已有能力不用通过其它网络设备便能达到虚拟专用网连接。

##历史

直到20世纪90年代末，计算机网络上的计算机通过非常昂贵的专线和/或拨号连线互连。视站点间的距离，花费可达数千美元（56kbps连线）或上万美元（T1）。

由于避免了租用多条各自连接互联网的专线的需要，虚拟私人网络可减少网络开支。用户可以安全地交换私密数据，这使昂贵的专线变得多余。

##安全性
安全的虚拟私人网络使用加密穿隧协议，通过阻止截听与嗅探来提供机密性，还允许发送者身份验证，以阻止身份伪造，同时通过防止信息被修改提供消息完整性。 某些虚拟私人网络不使用加密保护数据。虽然虚拟私人网络通常都会提供安全性，但未加密的虚拟私人网络严格来说是不“安全”或“可信”的。例如，一条通过GRE协议在两台主机间创建的隧道属于虚拟私人网络，但既不安全也不可信。 除以上的GRE协议例子外，本地的明文穿隧协议包括L2TP（不带IPsec时）和PPTP（不使用微软点对点加密(MPPE)时）。

##协议
常用的虚拟专用网协议有：

- L2F
- L2TP
- PPTP
- IPsec
- SSL VPN
- Cisco VPN
- OpenVPN

安装和配置

命令安装,终端执行:

    sudo apt-get install pptpd

编辑配置文件终端执行:

    sudo gedit /etc/pptpd.conf

修改为以下内容:

    option /etc/ppp/pptpd-options
    localip 192.168.0.1
    remoteip 192.168.0.11-150

继续编辑配置文件,终端执行:

    sudo gedit /etc/ppp/pptpd-options

修改里面的 ms-dns 这个项为 8.8.8.8 和 8.8.4.4。这是Google的DNS服务器,使用ISP提供的DNS也可以.

继续编辑配置文件,终端执行:

    sudo gedit  /etc/ppp/chap-secrets

此文件定义登录的用户名和密码,按下面的格式添加一行

    "用户名" pptpd "密码" *

修改配置文件后,重启pptpd配置信息才能生效,终端执行:

    /etc/init.d/pptpd restart   #重启pptp进程

检验PPTP服务器是否运行,在终端输入命令:

    sudo netstat -anp | grep pptpd

将得到类似如下结果,说明PPTP服务器运行成功.

    tcp  0  0  0.0.0.0:1723 0.0.0.0:*  LISTEN  27243/pptpd
    unix  2  [ ]  数据报  200719  27243/pptpd

为了客户端能够顺利连接到VPN服务器，还需主机防火墙开放VPN端口（默认为1723）,终端执行:

     sudo iptables -I INPUT -p tcp --dport 1723 -j ACCEPT

到目前为止,VPN服务器确实搭建完成,客户端也可以正常连接了,但是客户端却不能通过该VPN服务器上网, 我们还需要为客户端IP地址设置网络地址转换(NAT)

查看 “/proc/sys/net/ipv4/ip_forward” 文件中的值是否为”1″ 如果不是，则需在修改配置文件/etc/sysctl.conf中的相应内容如下,打开ipv4转发功能

    net.ipv4.ip_forward = 1

执行:

     sysctl -p
    /etc/init.d/procps restart

使其马上生效.

配置iptables:

    sudo iptables --table nat --append POSTROUTING --out-interface eth0 --jump MASQUERADE

注意上面的“eth0”要选择你对应的网卡

这样设置基本上可以上网了，但是在WINDOWS下面连接的时候可能会出现某些网站连接异常缓慢甚至连接超时的情况。这个时候就要设置一下最大MTU：

    sudo iptables -I FORWARD -p tcp --syn -i ppp+ -j TCPMSS --set-mss 1356

但是,只是这样 iptables 的规则会在下次重启时被清除,所以我们还需要把它保存下来, 编辑配置文件,终端执行:

    sudo gedit /etc/rc.local

在末尾另起一行加上下面的命令即可

    iptables --table nat --append POSTROUTING --out-interface eth0 --jump MASQUERADE
    iptables -I FORWARD -p tcp --syn -i ppp+ -j TCPMSS --set-mss 1356

下面还有一种方法,本人没有试验,但有网友反映,使用后无法进入系统。方法是使用 iptables-save 命令:

    iptables-save > /etc/iptables-rules

然后修改 “/etc/network/interfaces” 文件，找到 eth0 那一节，在对 eth0 的设置最末尾加上下面这句:

    pre-up iptables-restore < /etc/iptables-rules

这样当网卡 eth0 被加载的时候就会自动载入我们预先用 iptables-save 保存下的配置.

## 相关链接

[维基百科:虚拟专用网](http://zh.wikipedia.org/wiki/%E8%99%9B%E6%93%AC%E7%A7%81%E4%BA%BA%E7%B6%B2%E8%B7%AF)

[基于UBUNTU搭建VPN服务器(已验证)](http://blog.warmcolor.net/2013/06/21/%E5%9F%BA%E4%BA%8Eubuntu%E6%90%AD%E5%BB%BAvpn%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%B7%B2%E9%AA%8C%E8%AF%81/)