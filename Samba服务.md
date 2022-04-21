---
title: Samba服务
description: 
published: true
date: 2022-04-21T03:41:26.874Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:41:23.991Z
---

[[en:Samba_service]]


##简介

Samba，是种用来让UNIX系列的操作系统与微软Windows操作系统的SMB/CIFS（Server Message Block/Common Internet File System）网络协定做连结的自由软件。

目前的版本（v3）不仅可存取及分享SMB的资料夹及打印机，本身还可以整合入Windows Server的网域，扮演为网域控制站（Domain Controller）以及加入Active Directory成员。简而言之，此软件在Windows与UNIX系列OS之间搭起一座桥梁，让两者的资源可互通有无。

##历史

安德鲁·垂鸠（Andrew Tridgell）于1992年在澳洲国立大学（ANU）开发了第一版的Samba Unix软件。

##功能

Samba是许多服务以及协议的实现，其包括TCP/IP上的NetBIOS（NBT）、SMB、CIFS（SMB的增强版本）、 DCE/RPC或者更具体来说MSRPC（網絡鄰居协议套件）、一种 WINS服务器（也被称作NetBIOS Name Server（NBNS））、NT 域协议套件（包括NT Domain Logons、Secure Accounts Manager（SAM）数据库、Local Security Authority（LSA）服务、NT-style打印服务（SPOOLSS）、NTLM以及近来出现的包括一种改进的Kerberos协议与改进的轻型目录访问协议（LDAP）在内的Active Directory Logon服务）。以上这些服务以及协议经常被错误地归类为NetBIOS或者SMB。

Samba也能够用于共享打印机。

Samba能够为选定的Unix目录（包括所有子目录）建立网络共享。该功能使得Windows用户可以像访问普通Windows下的文件夹那样来通过网络访问这些Unix目录。

##相关软件
- Samba TNG：Samba的一个分支，其在NT域服务关键部分的结构及实现具有明显的不同。
- LinNeighborhood
- LDAP Account Manager
- Kerberos protocol
- Smb4K：KDE 环境下的 SMB/CIFS 共享资源浏览器。
- Smbldap-Tools：用户和群组管理工具


### 安装Samba

1.安装Samba服务并设置Samba服务

我们需要做的第一件事是安装Samba，终端输入:

    sudo apt-get install samba

2、再安装nautilus扩展

    sudo apt-get install nautilus-share

这个扩展的作用，在nautilus文件管理器里，需要共享的文件夹上右键->本地网络共享

注释:红色框内的选项最好不要选择,因为这样很危险!

这时直接创建共享会出现错误

    'net usershare' returned error 255: net usershare: cannot open usershare directory /var/lib/samba/usershares. 

    Error Permission denied You do not have permission to create a usershare. 

    Ask your administrator to grant you permissions to create a share.

3、解决办法，把你的用户名加入sambashare组 sudo adduser 你的用户名 sambashare

上面“你的用户名”用你实际的用户名代替

4、这时创建的共享另一台机器上还不能访问，必须先设置密码

    sudo smbpasswd -a 你的用户名

5、最后在另一台电脑上访问你的共享

会提示输入用户名和密码，输入第三、四步的用户名和密码即可

### 检查Samba服务是否正常启用.

查看我们当前电脑的IP地址,终端输入:

    ifconfig

打开一台局域网内的电脑,以Windows系统为例（虚拟机也可以,但网络模式必须是局域网模式） 我的电脑,地址栏输入

    file://xxx.xxx.xxx.xxx  ##xxx.xxx.xxx.xxx为上面查到的IP地址

如果能看到主机内的共享文件，说明Samba服务成功启用。更多设置请看相关链接

##相关链接

[官方文档](http://www.samba.org/samba/docs/)

[维基百科:Samba](http://zh.wikipedia.org/zh-cn/Samba)

[IMCN:怎样在 Ubuntu 12.04 中安装和设置 Samba 实现网上邻居共享](http://imcn.me/html/y2012/10717.html)