---
title: DHCP服务
description: 
published: true
date: 2022-05-08T14:18:38.418Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:31:20.874Z
---

## 简介

动态主机设置协议（Dynamic Host Configuration Protocol, DHCP）是一个局域网的网络协议，使用UDP协议工作，主要有两个用途：

- 给内部网络或网络服务供应商自动分配IP地址给用户
- 给内部网络管理员作为对所有电脑作中央管理的手段

## 适用性

DHCP用一台或一组DHCP服务器来管理网络参数的分配，这种方案具有容错性。即使在一个仅拥有少量机器的网络中，DHCP仍然是有用的，因为一台机器可以几乎不造成任何影响地被增加到本地网络中。

甚至对于那些很少改变地址的服务器来说，DHCP仍然被建议用来设置它们的地址。如果服务器需要被重新分配地址（RFC2071）的时候，就可以在尽可能少的地方去做这些改动。对于一些设备，如路由器和防火墙，则不应使用DHCP。把TFTP或SSH服务器放在同一台运行DHCP的机器上也是有用的，目的是为了集中管理。

DHCP也可用于直接为服务器和桌面计算机分配地址，并且通过一个PPP代理，也可为拨号及宽带主机，以及住宅NAT网关和路由器分配地址。DHCP一般不适用于使用在无边际路由器和DNS服务器上。

## 历史

DHCP于1993年10月成为标准协议，其前身是BOOTP协议。当前的DHCP定义可以在RFC 2131中找到，而基于IPv6的建议标准（DHCPv6）可以在 RFC 3315中找到。

## 原理

动态主机设置协议（DHCP）是一种使网络管理员能够集中管理和自动分配IP网络地址的通信协议。在IP网络中，每个连接Internet的设备都需要分配唯一的IP地址。DHCP使网络管理员能从中心结点监控和分配IP地址。当某台计算机移到网络中的其它位置时，能自动收到新的IP地址。

DHCP使用了租约的概念，或称为计算机IP地址的有效期。租用时间是不定的，主要取决于用户在某地联接Internet需要多久，这对于教育行业和其它用户频繁改变的环境是很实用的。通过较短的租期，DHCP能够在一个计算机比可用IP地址多的环境中动态地重新配置网络。DHCP支持为计算机分配静态地址，如需要永久性IP地址的Web服务器。

DHCP和另一个网络IP管理协议BOOTP类似。目前两种配置管理协议都得到了普遍使用，其中DHCP更为先进。某些操作系统，如Windows NT/2000，都带有DHCP服务器。DHCP或BOOTP客户端是装在计算机中的一个程序，这样就可以对其进行配置操作。

## 协议结构
```
<table class="block1">

    <tbody>
        <tr>
            <td>8 bits</td>
           <td>16 bits</td>
           <td>24 bits</td>
            <td>32 bits</td>
        </tr>
        <tr>
            <td>Op</td>
           <td>Htype</td>
           <td>Hlen</td>
            <td>Hops</td>
        </tr>
        <tr>
            <td colspan="4">Xid</td>
        </tr>
        <tr>
            <td colspan="2">Secs</td>
           <td colspan="2">Flags</td>
        </tr>
        <tr>
            <td colspan="4">Ciaddr</td>
        </tr>
        <tr>
            <td colspan="4">Yiaddr</td>
        </tr>
        <tr>
            <td colspan="4">Siaddr</td>
        </tr>
        <tr>
            <td colspan="4" >Giaddr</td>
        </tr>
        <tr>
            <td colspan="4">Chaddr (16 bytes)</td>
        </tr>
        <tr>
            <td colspan="4">Sname (64 bytes)</td>
        </tr>
        <tr>
            <td colspan="4">File (128 bytes)</td>
        </tr>
           <tr>
            <td colspan="4">Option (variable)</td>
        </tr>
    </tbody>

 </table>
```
- Op – 消息操作代码，既可以是引导请求（BOOTREQUEST）也可以是引导答复（BOOTREPLY）
- Htype – 硬件地址类型
- Hlen – 硬件地址长度
- Xid –处理ID
- Secs –从获取到IP地址或者续约过程开始到现在所消耗的时间
- Flags –标记
- Ciaddr –客户机 IP地址
- Yiaddr –“你的”（客户机）IP 地址
- Siaddr –在 bootstrap 中使用的下一台服务器的IP地址
- Giaddr –用于导入的接替代理IP地址
- Chaddr –客户机硬件
- Sname –任意服务器主机名称，空终止符
- File –DHCP 发现协议中的引导文件名、空终止符、属名或者空，DHCP供应协议中的受限目录路径名
- Options –可选参数字段。参考定义选择列表中的选择文件

## 技术细节

DHCP统一使用两个IANA分配的端口作为BOOTP：服务器端使用67/udp，客户端使用68/udp。

DHCP运行分为四个基本过程，分别为请求IP租约、提供IP租约、选择IP租约和确认IP租约。

客户在获得了一个IP地址以后，就可以发送一个ARP请求来避免由于DHCP服务器地址池重叠而引发的IP冲突。

### DHCP 发现 (DISCOVER)
客户在物理子网上发送广播来寻找可用的服务器。网络管理员可以配置一个本地路由来转发DHCP包给另一个子网上的DHCP服务器。该客户实现生成一个目的地址为255.255.255.255或者一个子网广播地址的UDP包。

客户也可以申请它使用的最后一个IP地址（在下面的例子里为192.168.1.100）。如果该客户所在的网络中此IP仍然可用，服务器就可以准许该申请。否则，就要看该服务器是授权的还是非授权的。 授权服务器会拒绝请求，使得客户立刻申请一个新的IP。非授权服务器仅仅忽略掉请求，导致一个客户端请求的超时，于是客户端就会放弃此请求而去申请一个新的IP地址。

### DHCP提供 (OFFER)
当DHCP服务器收到一个来自客户的IP租约请求时，它会提供一个IP租约。DHCP为客户保留一个IP地址，然后通过网络广播一个DHCPOFFER消息给客户。该消息包含客户的MAC地址、服务器提供的IP地址、子网掩码、租期以及提供IP的DHCP服务器的IP。

服务器基于在CHADDR字段指定的客户硬件地址来检查配置。这里的服务器，192.168.1.1，将IP地址指定于YIADDR字段。

### DHCP请求 (REQUEST)
当客户PC收到一个IP租约提供时，它必须告诉所有其他的DHCP服务器它已经接受了一个租约提供。因此，该客户会发送一个DHCPREQUEST消息，其中包含提供租约的服务器的IP。当其他DHCP服务器收到了该消息后，它们会收回所有可能已提供给客户的租约。然后它们把曾经给客户保留的那个地址重新放回到可用地址池中，这样，它们就可以为其他计算机分配这个地址。任意数量的DHCP服务器都可以响应同一个IP租约请求，但是每一个客户网卡只能接受一个租约提供。

### DHCP确认 (Acknowledge，ACK)
当DHCP服务器收到来自客户的REQUEST消息后，它就开始了配置过程的最后阶段。这个响应阶段包括發送一个DHCPACK包给客户。这个包包含租期和客户可能请求的其他所有配置信息。这时候，TCP/IP配置过程就完成了。

该服务器响应请求并发送响应给客户。整个系统期望客户来根据选项来配置其网卡。

### DHCP释放
客户端向DHCP服务器发送一个请求以释放DHCP资源，并注销其IP地址。鉴于客户端更多的时候并不清楚何时用户会将其从网络中移除，此协议不会託管“DHCP释放的发送”。

### 客户端配置参数
DHCP伺服器會提供一些選擇性的配置項目供DHCP客戶端設定。在RFC 2132文件裡面有提到這個詳細的內容。 你也可以參考Internet Assigned Numbers Authority（IANA）——DHCP and BOOTP PARAMETERS。

### 設定選項
DHCP Option 60可以被DHCP客戶端用來做為辨識供應商及DHCP客戶端這邊的相容性識別。可以參考RFC 2132, Section 9.13的內容。 DHCP的協定裡頭有提供預設路由的選項，Option60則是供應商的識別ID。基於這個選項，你可以在CPE方提供給STB方一些特定的選擇。這樣做最大的好處是在使用option60的時候你不用去定義橋接或路由的埠號。橋接是基於option60的MAC位址，如此一來switch可以連到STB上面，如同在PC及STB上面擁有同一個介面。

Option 60這個訊息會是一個長度會變動的字串也有可能依供應商提供的八進位數字的一個集合。DHCP客戶端通常會用來溝通的一個方式是在送出DHCP要求的時候按硬體或韌體的型別來設定這個資訊，這個資訊會被稱之為供應商Class識別（VCI Vendor Class Identifier）/(Option 60)。這個方式可能因DHCP Server之間的不同而會在兩種CMs或兩種modems之間進行DHCP request時造成差異。有些set-top的Boxes也會設定VCI去通知DHCP Server有關硬體和裝置的功能性資訊。所以結論是，這個選項的資訊會給予DHCP Server在做DHCP回應時必要附加訊息上面的提示。

### 安装与配置

命令安装,终端执行:
```
         sudo apt-get install 3- dhcp3-server
```
装结束后，先配置dhcp3监听的网卡，先备份，后配置，命令如下：
```
          sudo cp /etc/default/isc-dhcp-server /etc/default/isc-dhcp-server.bk
         sudo gedit /etc/default/isc-dhcp-server
```
打开后修改监听的网卡，我的是eth0,大家视情况而定，可以用ifconfig -a查看自己的网卡
```
          INTERFACES="eht0"
```
完成后我们开始修改dhcp的主配置文件，配置地址池、租期、dns和网关，同样先备份
```
          sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bk
         sudo gedit /etc/dhcp/dhcpd.conf
```
打开后添加如下配置
```
         option domain-name "XXX.com";
         option routers 192.168.4.1;
         subnet 192.168.4.0 netmask 255.255.255.0 {
         range 192.168.4.10 192.168.4.100;

          #地址池
         option domain-name-servers 192.168.4.1;

          #默认网关
         option broadcast-address 192.168.4.255;
         #广播地址
         }
```
同时修改默认的租期，同时用#注释掉默认的域和DNS，域、DNS、租期可以在上面找到
```
         default-lease-time 6000;
         max-lease-time 72000;
         #option domain-name “example.org”;
         #option domain-name-servers ns1.example.org, ns2.example.org;
```
完成后重启dhcp服务
```
          sudo /etc/init.d/isc-dhcp-server restart
```
## 相关链接

[维基百科:DHCP](http://zh.wikipedia.org/wiki/DHCP)

[Ubuntu 12.04下的DHCP服务器的配置](http://www.slightme.info/archives/59)
