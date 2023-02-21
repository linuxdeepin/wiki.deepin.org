---
title: VPN服务
description: 
published: true
date: 2022-10-25T01:21:13.095Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:44:05.257Z
---

安装和配置

命令安装,终端执行:
```
    sudo apt-get install pptpd
```
编辑配置文件终端执行:
```
    sudo gedit /etc/pptpd.conf
```
修改为以下内容:
```
    option /etc/ppp/pptpd-options
    localip 192.168.0.1
    remoteip 192.168.0.11-150
```
继续编辑配置文件,终端执行:
```
    sudo gedit /etc/ppp/pptpd-options
```
修改里面的 ms-dns 这个项为 8.8.8.8 和 8.8.4.4。这是Google的DNS服务器,使用ISP提供的DNS也可以.

继续编辑配置文件,终端执行:
```
    sudo gedit  /etc/ppp/chap-secrets
```
此文件定义登录的用户名和密码,按下面的格式添加一行
```
    "用户名" pptpd "密码" *
```
修改配置文件后,重启pptpd配置信息才能生效,终端执行:
```
    /etc/init.d/pptpd restart   #重启pptp进程
```
检验PPTP服务器是否运行,在终端输入命令:
```
    sudo netstat -anp | grep pptpd
```
将得到类似如下结果,说明PPTP服务器运行成功.
```
    tcp  0  0  0.0.0.0:1723 0.0.0.0:*  LISTEN  27243/pptpd
    unix  2  [ ]  数据报  200719  27243/pptpd
```
为了客户端能够顺利连接到VPN服务器，还需主机防火墙开放VPN端口（默认为1723）,终端执行:
```
     sudo iptables -I INPUT -p tcp --dport 1723 -j ACCEPT
```
到目前为止,VPN服务器确实搭建完成,客户端也可以正常连接了,但是客户端却不能通过该VPN服务器上网, 我们还需要为客户端IP地址设置网络地址转换(NAT)

查看 “/proc/sys/net/ipv4/ip_forward” 文件中的值是否为”1″ 如果不是，则需在修改配置文件/etc/sysctl.conf中的相应内容如下,打开ipv4转发功能
```
    net.ipv4.ip_forward = 1
```
执行:
```
     sysctl -p
    /etc/init.d/procps restart
```
使其马上生效.

配置iptables:
```
    sudo iptables --table nat --append POSTROUTING --out-interface eth0 --jump MASQUERADE
```
注意上面的“eth0”要选择你对应的网卡

这样设置基本上可以上网了，但是在WINDOWS下面连接的时候可能会出现某些网站连接异常缓慢甚至连接超时的情况。这个时候就要设置一下最大MTU：
```
    sudo iptables -I FORWARD -p tcp --syn -i ppp+ -j TCPMSS --set-mss 1356
```
但是,只是这样 iptables 的规则会在下次重启时被清除,所以我们还需要把它保存下来, 编辑配置文件,终端执行:
```
    sudo gedit /etc/rc.local
```
在末尾另起一行加上下面的命令即可
```
    iptables --table nat --append POSTROUTING --out-interface eth0 --jump MASQUERADE
    iptables -I FORWARD -p tcp --syn -i ppp+ -j TCPMSS --set-mss 1356
```
下面还有一种方法,本人没有试验,但有网友反映,使用后无法进入系统。方法是使用 iptables-save 命令:
```
    iptables-save > /etc/iptables-rules
```
然后修改 “/etc/network/interfaces” 文件，找到 eth0 那一节，在对 eth0 的设置最末尾加上下面这句:
```
    pre-up iptables-restore < /etc/iptables-rules
```
这样当网卡 eth0 被加载的时候就会自动载入我们预先用 iptables-save 保存下的配置.

## 相关链接

[维基百科:虚拟专用网](http://zh.wikipedia.org/wiki/%E8%99%9B%E6%93%AC%E7%A7%81%E4%BA%BA%E7%B6%B2%E8%B7%AF)

[基于UBUNTU搭建VPN服务器(已验证)](http://blog.warmcolor.net/2013/06/21/%E5%9F%BA%E4%BA%8Eubuntu%E6%90%AD%E5%BB%BAvpn%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%B7%B2%E9%AA%8C%E8%AF%81/)