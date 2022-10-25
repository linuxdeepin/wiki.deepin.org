---
title: FTP服务
description: 
published: true
date: 2022-05-08T14:02:03.277Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:34:13.000Z
---

##前言

FTP是File Transfer Protocol（文件传输协议）的英文简称，而中文简称为“文传协议”。

用于Internet上的控制文件的双向传输。同时，它也是一个应用程序（Application）。基于不同的操作系统有不同的FTP应用程序，而所有这些应用程序都遵守同一种协议以传输文件。在FTP的使用当中，用户经常遇到两个概念："下载"（Download）和"上传"（Upload）。"下载"文件就是从远程主机拷贝文件至自己的计算机上；"上传"文件就是将文件从自己的计算机中拷贝至远程主机上。

用Internet语言来说，用户可通过客户机程序向（从）远程主机上传（下载）文件。

##安装

安装vsftpd,终端执行:

    sudo apt-get install vsftpd

##卸载

卸载vsftpd,终端执行:

    sudo apt-get remove vsftpd

##配置

终端执行以下命令设置FTP状态:

    service vsftpd start  ##启动ftp
    service vsftpd stop   ##停止ftp
    service vsftpd restart   ##重启ftp

默认的配置文件

ftp的配置文件主要有三个，一般位于/etc/vsftpd/目录下：

    ftpusers    该文件用来指定那些用户不能访问ftp服务器。
    user_list   ftp登录的白名单（或者黑名单）
    vsftpd.conf   vsftpd的主配置文件

若要配置vsftpd，需要修改vsftpd的配置文件。终端执行:

    sudo gedit /etc/vsftpd.conf

有关用户登录控制的行：

    anonymous_enable=YES，允许匿名用户登录。
    no_anon_password=YES，匿名用户登录时不需要输入密码。
    local_enable=YES，允许本地用户登录。
    deny_email_enable=YES，可以创建一个文件保存某些匿名电子邮件的黑名单，以防止这些人使用Dos攻击。
    banned_email_file=/etc/vsftpd/banned_emails，保存电子邮件黑名单的目录（默认）

有关用户权限控制的行：

    write_enable=YES，开启全局上传
    local_umask=022，本地文件上传的umask设置为022，系统默认。
    anon_upload_enable=YES，允许匿名用户上传，当然要在write_enable=YES的情况下。同时必须建立一个允许ftp用户读写的目录。
    anon_mkdir_write_enable=YES，允许匿名用户创建目录
    chown_uploads=YES，匿名用户上传的文件属主转换为别的用户，一般建议为root。
    chown_username=whoever，改此处的whoever为要转换的属主，建议root
    chroot_list_enable=YES，用一个列表来限定哪些用户只能在自己目录下活动。
    chroot_list_enable=/etc/vsftpd/chroot_list，指定用户列表文件
    nopriv_user=ftpsecure，指定一个安全账户，让ftp完全隔离和没有特权的账户

有关用户连接和超时的行：

    idle_session_timeout=600，默认的超时时间
    data_connection_timeout=120，设置默认数据连接的超时时间

有关服务器日志和欢迎信息的行:

    dirmessage_enable=YES，允许为配置目录显示信息
    ftpd_banner=Welcome to blah FTP service. ftp的欢迎信息
    xferlog_enable=YES 打开日志记录功能
    xferlog_file=/var/log/xferlog 日志记录文件的位置

其他的建议不要配置。

我们可以更改以上的各个设置，然后重启ftp服务就可以实现对ftp的配置了。


##相关链接

[维基百科:FTP](http://zh.wikipedia.org/wiki/%E6%96%87%E4%BB%B6%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)

[LINUX下搭建FTP服务器](http://www.2cto.com/os/201107/98311.html)

[用LINUX架设FTP服务器（推荐阅读)](http://www.chinaunix.net/old_jh/4/269002.html)