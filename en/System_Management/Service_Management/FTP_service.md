---
title: FTP_service
description: 
published: true
date: 2022-05-17T03:18:56.628Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:55:23.410Z
---

## Summary

FTP is short for File Transfer Protocol. It is used for control bidirectional transmission of files on the Internet. It can also refer to the application that use FTP to transfer files. Users often encounter two concepts when using FTP: download and upload. The former refers to copying files from any remote server to local disk, and the later refers to copying files from local disk to any remote server.

## Installation

To install vsftpd for providing FTP service, execute in terminal:

    sudo apt-get install vsftpd

## Uninstallation

To uninstall vsftpd, execute in terminal:

    sudo apt-get remove vsftpd

## Configuration

To set the state of FTP service:

    service vsftpd start  ## Start FTP server
    service vsftpd stop   ## Stop FTP server
    service vsftpd restart   ## Restart FTP server

There are three configuration files used by FTP service, all located in directory /etc/vsftpd:

    ftpusers    Used for specifying the users that are forbidden to access FTP server
    user_list   Whitelist or blacklist of accounts
    vsftpd.conf   Main configuration file of vsftpd

To modify the configuration of vsftpd, execute in terminal:

    sudo gedit /etc/vsftpd.conf

Lines that are related to controls of user login:

    anonymous_enable=YES  ## Allow anonymous login
    no_anon_password=YES  ## Allow anonymous login without password
    local_enable=YES  ## Allow local users to log in
    deny_email_enable=YES  ## Create blacklist file for email addresses for preventing Dos attack
    banned_email_file=/etc/vsftpd/banned_emails  ## Directory used for save blacklist files of email addresses

Lines that are related to controls of user permission:

    write_enable=YES  ## Enable global upload
    local_umask=022  ## Set umask to 022 for all uploaded files
    anon_upload_enable=YES  ## Allow anonymous user to upload. This should be used with "write_enable=YES",  as well as a writable directory created for ftp users
    anon_mkdir_write_enable=YES  ## Allow anonymous user to create directories
    chown_uploads=YES  ## Change owner of uploaded files to other user, usually root
    chown_username=whoever  ## The owner of all uploaded files
    chroot_list_enable=YES  ## Users restricted in their own directories
    chroot_list_enable=/etc/vsftpd/chroot_list  ## File that contains the users restricted in their own directories
    nopriv_user=ftpsecure  ## A secure account specified for vsftpd to run as

Lines that are related to controls of connections and timeouts:

    idle_session_timeout=600  ## Default timeout for login session
    data_connection_timeout=120  ## Default timeout for data connection

Lines that are related to controls of server journals and welcome message:

    dirmessage_enable=YES  ##  Show messages when users entering a new directory
    ftpd_banner=Welcome to XXX FTP service.  ## A welcome message shown to users
    xferlog_enable=YES ## Enable login
    xferlog_file=/var/log/xferlog  ## Place to store log file

It is recommended to leave other lines untouched.

Please restart vsftpd service to make changes take effect.

## References

[维基百科:FTP](http://zh.wikipedia.org/wiki/%E6%96%87%E4%BB%B6%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)

[LINUX下搭建FTP服务器](http://www.2cto.com/os/201107/98311.html)

[用LINUX架设FTP服务器（推荐阅读)](http://www.chinaunix.net/old_jh/4/269002.html)
