---
title: Samba_service
description: 
published: true
date: 2022-05-17T03:23:43.083Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:10.105Z
---

## Summary

Samba is made for connection between Unix-like operating system with SMB/CIFS from Microsoft Windows. It is a free software too.

The current version (v3) of Samba allows sharing of SMB folders and printers, as well as Samba network itself to be integrated into Windows Server domain to act as a domain controller and join as a member of Active Directory. In brief, Samba serves as a bridge between Windows and Unix-like system, making resource sharing more easier.

## History

The first version of Samba for Unix was created by Andrew Tridgell in Australian National University (ANU) in 1992.

## Functionalities

Samba implements a lot of services, including NetBIOS over TCP / ICP (NBT), SMB, CIFS (an enhanced version of SMB), DCE / RPC (or more precisely, MSPRC), a kind of WINS server (also called NetBIOS Name Server (NBNS))， NT domain protocol suite (including Domain Logons、Secure Accounts Manager (SAM), databases, Local Security Authority (LSA) service, NT-style printing service (SPOOLSS), NTLM, as well as Active Directory Logon service that contains a kind of lightweight Kerberos protocol and a lightweight directory access protocol (LDAP)). Some of these services are sometimes classified into NetBIOS or SMB by mistake.

Samba can also be used to share printers.

Samba can establishes network sharing for specified Unix directory (and all its subdirectories), so that Windows users are able to access these Unix directory in the way that they access common directories in Windows.

## Related software

- Samba TNG: A branch of Samba. It has significant difference in structure and implementation of key part of NT domain service.
- LinNeighborhood
- LDAP Account Manager
- Kerberos protocol
- Smb4K: SMB/CIFS Share Browser for KDE
- Smbldap-Tools: Management tool of user and group.

## Installing samba

First we need to install samba service and start it. Execute in terminal:

    sudo apt-get install samba

Then install a extension for nautilius:

    sudo apt-get install nautilus-share

This extension is to create a entry for file sharing in the context menu of nautilus.

If errors occur when creating sharing, like

    'net usershare' returned error 255: net usershare: cannot open usershare directory /var/lib/samba/usershares. 

    Error Permission denied You do not have permission to create a usershare. 

    Ask your administrator to grant you permissions to create a share.

Try to add your user name into group "sambashare":

    sudo adduser Username sambashare

If other machines are prevented from accessing files by password, set one password for them:

    sudo smbpasswd -a Username

Now access share files on another machine, and input the user name and password that you set before. You should be able to see those files.

## Check if Samba service is available

First, find the current IP address of our machine. Execute in terminal:

    ifconfig

Then open another machine in the LAN, a computer running Windows for example, then type in the address bar:

    file://xxx.xxx.xxx.xxx

where "xxx.xxx.xxx.xxx" is the IP address you get from our machine. If you can see files shared from our machine, then the Samba service has already been started.

You may also access shared files in a system inside a virtual machine, but be sure that the network mode is in LAN mode.

## References

[官方文档](http://www.samba.org/samba/docs/)

[维基百科:Samba](http://zh.wikipedia.org/zh-cn/Samba)

[IMCN:怎样在 Ubuntu 12.04 中安装和设置 Samba 实现网上邻居共享](http://imcn.me/html/y2012/10717.html)
