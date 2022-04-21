---
title: User_and_task
description: 
published: true
date: 2022-04-21T03:57:35.528Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:32.675Z
---

[[zh:用户与任务]]


## Summary

Linux is a multi-user, multi-task operating system. It would be bettwer to understand the concept of multi-user and multi-task.

## User and task

### Roles of user

There are several roles for users in Linux system. Different roles are supposed to complete different task. It is worth noting that roles are identified by UID and GID, especially UID. For system administrator, every UID must be unique.

* Root user: real, unique in system, have the highest privilege
* Virtual user: also called fake user, distinguished from real user. They cannot login to system, but is indispensable to functionning of system.  Some virtual users are presented in system by default; however, we can also add them manually. Example: bin, daemon, adm, ftp, mail
* Normal user: real user that can login to system, but limited to access to its own files. Usually created by system administrator.

### Single user multi-tasking

This refers to that one user dealing with several tasks at one time.

For example, we use "Beinan" to login system. After entering the system, I want to open GEDIT to write a document, but in the process of writing the document, I feel less music, so open the XMMS. Come on some music. Of course, it's not enough to listen to music, MSN also have to open, want to know some of the brothers are doing now. When the user logs in, the execution of the GEDIT, XMMS and MSN, of course, there are input method fcitx.

In this way, a user, in order to complete the work, has created several tasks.

### Multi-user multi-tasking

Sometimes it is possible that a lot of users use the same system at the same time, but this does not mean all users have to do the same thing.

For example, LinuxSir.Org server, there are FTP user, system administrator, web users, regular users and so on, at the same time, there may be some brothers are visiting the Forum; some may upload software package management sub station, such as luma or Yuking brother in the management of their home system and FTP. At the same time, there may be a system administrator in the maintenance of the system; browse the home page with the use of nobody
User, we use the same, and upload software package used is FTP user; view or administrator of the system maintenance, may use is ordinary account or super root privileges account; different user has the permissions are different, to complete different tasks need different users can also be different user said, may complete the work are not the same.

It is worth noting that multi-user and multi task and not at the same time, everyone crowded to a grounding in a machine of the keyboard and display came to operate the machine, the user may through the remote login, such as the server of the remote control, as long as the user permissions anyone can up the operation or visit the.

### Security

Multi-user system is in fact more safe and convenient for system management.

In aspect of user, file permission can be applied to certain files, so that they can only be accessed by its owner. Thus the information of every user is protected. 

In aspect of server administrator, Linux, as well as other Unix-like system, is also more secure than Windows.

## Managing online user

### View user's tasks

To view the task of certain user, use `w` command:

    # W
    2:31PM UP 11 DAY ,21:18 4 USERS, LODE AVERAGE : 0.12, 0.09 , 0.08
    USER TTY FROM LOGIN@ IDLE JCPU PCPU WHAT
    ROOT TTY1 - 09:21AM 3:23 0.13S 0.08S -BASH
    GEORGE TTY2 - 09:40AM 18:00S 0.12S 0.00S TELNET
    HELLO TTY6 - 11:12AM 34.00S 0.06S 0.O6S BASH
    MARRY PTS/1 192.0.3.1102:40PM 5.20S 0.09S 0.03S FTP

The first line summarize the system information: current time (2:31 p.m.), system running time (11 days 21 hours and 18 minutes), number of users logined (4 users) and average system load.

Frome the second line to the last line, a list of information of users are given. Each line contains several fields:

* USER: login name of user. If a user has logined for several times, they will all be shown here.
* TTY: the terminal used by user
* FROM: the place from where the user logged in
* LOGIN@: means "login at", time when user logged in
* IDLE: idle time of user; counted from the last time user's task has been finished
* JCPU: total time consumed by this terminal; distinguished by terminal number;
* PCPU: time cconsumed by current task
* WHAT: current task name

### View users that are logged in

Use `who` command to see who are currently online, as well as their information.

Example:

    # who
    root tty1 - 09:21am
    reorge tty2 - 09:40am
    hello tty6 - 11:12am
    marry pts/1 :0 02:40pm

The output of "who" is quite similar to "w". If you would like to more about these user, append options like -H (header line), -I (idle time) or -T (whether this user accepts messages from other user). 

If  a user accepts message from other user, a "+" is displayed in "MESG" field. You can use `mesg` command to send him / her a message.

### View login history

To know the history of login of certain users, use `last` command.

Example:

     ROOT TTY1 09：21AM MON FRI 10 11：15 STILL LOGGED IN
    GEORGE TYY2 09：40AM MON FRI 11 11：18 -DOWN
    HELLO TTY6 11：12AM MON FRI 12 9：47 -DOWN
    MARRY PTS/1 192.0.3.11 02：40PM FRI 17 12：56 -DOWN
     ……
    WTMP BEGINS FRI DEC 5 12：53：55 2003

The output of "last" command may be quite long, so it is advised to use `last | less` to view it. Like "w" command, you can append a user name to "last: to see only information about this user.

    last george
    george tty2 - 09:40am mon fri 11 11.18 -down
    ………….
    Wtem begins fri dec 5 12:53;55 2003

In fact, `last` shows the content in /var/log/wtmp, which is stored in binary format and cannot be understood directly by human.

### "Kick out" onlinen user

Use `pkill` to force log out certain users.

Syntax:

    pkill -kill -t [TTY]

[TTY] refers to the terminal that the user are using.

After execute `pkill` command, use `w` to see if the user has been logged out.

### Log out by yourself

Example:

     gnome-session-quit --logout --force  ## Log out through gnome-session
     pkill -9 gnome-session   ## Kiill gnome-session to logout
     sudo restart lightdm  ## Restart lightdm Display manager to logout

## Source link

[Linux 用户（user）和用户组（group）管理概述](http://linux.chinaunix.net/techdoc/system/2006/03/24/929728.shtml)

[Linux系统下查看已经登录用户并踢出的方法](http://tech.ccidnet.com/art/302/20061121/956303_1.html)