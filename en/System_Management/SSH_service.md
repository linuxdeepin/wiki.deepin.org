---
title: SSH_service
description: 
published: true
date: 2022-05-07T02:30:56.087Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:06.283Z
---

## Summary

SSH is short for Secure Shell, designed by Network Working Group of IETF. It is a secure protocol established on application layer and transport layer of a network, providing security for remote login session and other network services. SSH can be applied to eliminate information disclosure issue during remote management of machines.

At first, SSH is only a Unix program, but quickly adapted to other platforms because of its ability to compensate the lack of network security when it is correctly used. Almost all Unix-like platform, including HP-UX, Linux, AIX, Solaris, Digital Unix, Irix support SSH.

This entry give introduce briefly the installation and configuration of SSH service.

## Installation

SSH consists of SSH clicent (openssh-client) and SSH server (openssh-server).

If you just want to log in to other machine using SSH, only openssh-client is neened. deepin has it installed by default; if not, execute in terminal:

    sudo apt-get install openssh-client

If you would like to provide SSH service on a server, execute in terminal:

    sudo apt-get install openssh-server

## Uninstallation

Execute in terminal:

    sudo apt-get remove openssh-client
    sudo apt-get remove openssh-server

## Configuration

Make sure that SSH server has been started:

    ps -e |grep ssh

A process named "sshd" in the output indicates that SSH server has been started successfully. If no one presents, execute:

    sudo /etc/init.d/ssh start 

or

    service ssh start

The configuration files of SSH server is /etc/ssh/sshd_config, where you can define the port, indentity files and other parameters. The default port for SSH service is 22, you may want to change it to another value, saying 222.

Then restart SSH service:

    sudo /etc/init.d/ssh stop
    sudo /etc/init.d/ssh start

Now you can log in to the server using SSH:

    ssh Username@192.168.1.112

where "Username" is the user name on machine "192.168.1.112". You may need to provide the password for this user during login.

## Frequently asked question

### View and manage logged-in user

Use `w`  and `pkill` command to see active user in the system. See [User and task](https://wiki.deepin.org/index.php?title=User_and_task) for details in user management.

## References

[官方网站](http://www.openssh.org/)

[Linux下查看已登录用户以及pkill强制活动用户退出命令](http://wangye.org/blog/archives/343/)
