---
title: Mobile_broadband
description: 
published: true
date: 2022-05-20T10:36:45.140Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:56:12.324Z
---

## Summary

A mobile broadband connection refers to the connection that uses a cellular network (GPRS / EDGE / 3G / 4G). This entry briefly introduces the configuration for setting up a mobile network.

Note: In later versions of deepin, the settings of mobile networks may not be accessible through the Control Center. Please try another network manager (for example, Gnome Network Manger) to do so.

## Configuration

Go to the Control Center -> Network -> Mobile broadband, and click to add a connection. The mandatory fields are:

* Country / Region

* Provider

* Username

* Password

The last two fields should be obtained from your service provider, or the manufacturer of the network card.

After save your configurations, click the name of the connection to activate it.

### Note

If the method above does not bring you a active connection, please check if the network card you brought supports Linux platform. If not, contact the seller or official support of the network card.

## Examples

### Huawei et127 (3G Modem)

Deepin provides support of Huawei et127, without manual configuration by user. However, If the modem is not detected correctly, you may need to use usb-modeswitch and wvdial (or gnome-ppp, or the dial-up program that comes with your modem). usb-modeswitch is used to switch modes between the modem and the USB disk, and wvdial / gnome-ppp is used to set parameters for dial-up).

First deactivate the wired network (if any). You can execute in terminal:

    sudo ifconfig eth0 down

where "eth0" is the name of the wired network card.

Get the local IP address of your modem:

    sudo wvdial

Then set it as the default gateway.

    sudo route add default gw 10.150.129.135

and restart networking service

    sudo /etc/init.d/networking restart

To configure the DNS service for your modem, check the current DNS configuration:

    ls -l /etc/resolv.conf

If this file is not linked to the file /run/resolvconf/resolv.conf, execute:

    sudo rm /etc/resolv.conf
    sudo ln -s /run/resolvconf/resolv.conf /etc/resolv.conf 

Edit this file:

    sudo gedit /etc/resolv.conf

and add IPs of name servers obtained from command `sudo wvdial`:

    nameserver 221.130.33.60
    nameserver 221.130.33.52

Then restart resolvconf and networking service:

    sudo /etc/init.d/resolvconf restart
    sudo /etc/init.d/networking restart

Now you should be able to reach the Internet. The next time when you need to connect to the mobile network, execute in terminal:

    sudo wvdial # For dial-up
    sudo ifconfig eth0 down # For bringing down the wired connection
    sudo route add default gw x.x.x.x # For setting up the gateway

You may also need to run

    sudo usb_modeswitch

first to make deepin accept the modem using a correct mode.

## References

[Ubuntu 11.04 怎样安装华为(Huawei)et127(中国移动G3无线上网卡)](http://zhidao.baidu.com/question/320409484.html)

[拨号上网设置](http://wiki.linuxdeepin.com/index.php/ADSL_%E6%8B%A8%E5%8F%B7%E4%B8%8A%E7%BD%91%E8%AE%BE%E7%BD%AE)
