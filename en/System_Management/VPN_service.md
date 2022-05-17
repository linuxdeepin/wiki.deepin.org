---
title: VPN_service
description: 
published: true
date: 2022-05-17T03:28:36.438Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:40.020Z
---

## Summary

Virtual Private Network (VPN) is a communication  method commonly used in private networks of enterprises and groups. Messages on VPN are conveyed through public networks, and encrpyted tunneling protocol is used to ensure security features like confidentiality, authentication and accuraccy. This technology can be applied to unsecure network (such as Internet) for delivering reliable messages in a secure way. It is worthing noting that the delivered message itself is explicit to everyone, no matter it is encrpyted or not. Thus the VPN without encryption still suffer the risk of information leaks.

Take an example of mail delivering in daily life. Alice would like to send a letter to Bob that belongs to a different company of Alice. Both Alice and Bob know the department of each other, but the department names cannot be written directly on the letter. So Alice has to put the letter (whose content can be encrypted or not) inside another envelop that needs to be delivered to the company of Bob, and the secretary of Bob will transfer the inside letter to Bob when he or she receive the outside envelope from Alice. Bob will need to reply to Alice in the same way.

Now imagine that Alice and Bob are two machines belonging to different internal network. They send messages to each other through public network, while the content of the messages is extracted by the proxy (e.g. routers or firewalls that support VPN) and presented to Alice and Bob. Many operating system, like Windows and Linux, have the ability to establish a VPN without the employment of extra network device.

## History

Until the 90s of the twentieth century, computers are connected through expensive dedicated network or dial-up lines. The costs of network construction, according to the distance between them, can easily rise up to thousands (for 56 kbps line) or millions (for T1 connection) of dollar.

VPN can reduce the costs of network as it avoid the needs of renting lines connected to different target. Users can thus exchange private data without introducing expensive dedicated network.

## Security

Secure VPN uses encrypted tunneling protocol to block sniffing and provide confidentiality. It allows authentication of sender's identity and prevents modification of messages to provide the integrity of message. Although most VPN provide varyious degree of security, the unencrypted VPN is not "secure" or "reliable". For example, a tunnel create between to hosts using GRE protocol is a kind of VPN, but it is neither secure nor reliable. Other examples include L2TP without IPsec and PPTP without MPPE.

## Protocol

Some commonly used VPN protocols are:

- L2F
- L2TP
- PPTP
- IPsec
- SSL VPN
- Cisco VPN
- OpenVPN

Here we take PPTP as an example to demonstrate the process of configuration of VPN.

## Installation and configuration of PPTP

Execute in terminal:

    sudo apt-get install pptpd

Open the main configuration file of pptp:

    sudo gedit /etc/pptpd.conf

and modify the following lines:

    option /etc/ppp/pptpd-options
    localip 192.168.0.1
    remoteip 192.168.0.11-150

Open the option file of pptp:

    sudo gedit /etc/ppp/pptpd-options

and change the line of "ms-dns" to "8.8.8.8" or "8.8.4.4", which is the DNS server provided by Google. You may also use DNS servers provided by your ISP.

Then open the secret file for pptp:

    sudo gedit  /etc/ppp/chap-secrets

and append one line to it:

Username pptpd Password *

where "Username" and "Password" is the name and the password used during login.

Then restart pptp service for changes to take effect:

    /etc/init.d/pptpd restart

Check if pptp service is running correctly:

    sudo netstat -anp | grep pptpd

If you get following lines, then pptp server has been started successfully.

    tcp  0  0  0.0.0.0:1723 0.0.0.0:*  LISTEN  27243/pptpd
    unix  2  [ ]  DGRAM  200719  27243/pptpd

In order that clients can connect to VPN server, the firewall on the server needs to open a specific port (1723 by default):

     sudo iptables -I INPUT -p tcp --dport 1723 -j ACCEPT

Up to now, the VPN server is correctly configured and the clients can connect to it as well. But it is still impossible to access the Internet through the VPN server, so we need further configuration of NAT for client IP addresses

First view the content of file "/proc/sys/net/ipv4/ip_forward". If it is not "1", change the configurations in "/etc/sysctl.conf" to enable IPv4 forwarding:

    net.ipv4.ip_forward = 1

Then execute:

    sudo sysctl -p
    sudo /etc/init.d/procps restart

to make it takes effect.

Configure iptables:

    sudo iptables --table nat --append POSTROUTING --out-interface eth0 --jump MASQUERADE

Replace "eth0" with the name of your network card.

Now we can access the Internet through the VPN server. Some clients using Windows may not connect to websites or encounter timeouts, so we need to adjust the MTU:

    sudo iptables -I FORWARD -p tcp --syn -i ppp+ -j TCPMSS --set-mss 1356

To automatically change the MTU each time system starts up, modify the startup script:

    sudo gedit /etc/rc.local

and append following two lines:

    iptables --table nat --append POSTROUTING --out-interface eth0 --jump MASQUERADE
    iptables -I FORWARD -p tcp --syn -i ppp+ -j TCPMSS --set-mss 1356

You may also try "iptables-save" command. However, this might cause system to fail to boot, so be cautious.

Execute in terminal:

    iptables-save > /etc/iptables-rules

Then open file "/etc/network/interfaces", find the section related to "eth0" (or the name of your network card), and append one line at the end of that section:

    pre-up iptables-restore < /etc/iptables-rules

This will mkae "eth0" loaded with the configuration saved by iptables-save command.

## References

[维基百科:虚拟专用网](http://zh.wikipedia.org/wiki/%E8%99%9B%E6%93%AC%E7%A7%81%E4%BA%BA%E7%B6%B2%E8%B7%AF)

[基于UBUNTU搭建VPN服务器(已验证)](http://blog.warmcolor.net/2013/06/21/%E5%9F%BA%E4%BA%8Eubuntu%E6%90%AD%E5%BB%BAvpn%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%B7%B2%E9%AA%8C%E8%AF%81/)
