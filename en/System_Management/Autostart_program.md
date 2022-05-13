---
title: Autostart_program
description: 
published: true
date: 2022-05-07T02:28:24.939Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:53:58.615Z
---

## Summary

This entry gives bref introduction to setting and managing of autostart program in Linux.

## Using graphical interface

Open launcher, right click on the program you would like to autostart, and choose "Add to startup".

## Managing configuration files

### Autostart programs when starting system

Generally, init process is started after Linux kernel is loaded. It then initializes hardware and drivers, and continues to start other processes according to configuration files. These configurations are usually stored in following directories:

    /etc/rc
    /etc/rc.d
    /etc/rcX.d

where X is 0, 1, 2, 3, 4, 5, 6 or S.

The script files in these directories can be executed to start other programes. For example, appending "xinit" or "startx" to file "/etc/rc.d/rc.local" (the script that is last executed during startup) will cause desktop environment to show.

### Add autostart command in ~/.config/autostart

Take proxy tool XX-Net as example. Suppose its starting script is located in "~/Documents/XX-Net-3.3.1/start". We nned to add XX-Net.desktop under "~/.config/autostart":

```
[Desktop Entry]
Type=Application
Exec="/home/wy/Documents/XX-Net-3.3.1/start"
Hidden=false
NoDisplay=false
X-GNOME-Autostart-enabled=true
Name[en_IN]=XX-Net
Name=XX-Net
Comment[en_IN]=XX-Net
Comment=XX-Net
```

The next time when user logs in, the command after  "Exec=" will be executed automatically.

### Run program when login

When user logs in, bash will first execute global login script made by system administrator, "/etc/profile".

Then search for one of the following files in user's home directory:

    /.bash_profile
    /.bash_login
    /.profile

And run the first one it founds. So it is easy to run certain program during login, by adding commands to the files above.

### Run program when logout

When user logs out, bash will first execute the personal login script, "~/.bash_logout".

For example, in order to run command "tar" for backing up all ".c" files, append command

    tar －cvzf c.source.tgz *.c

to file ~/.bash_logout.

### Run programs regularly

Linux uses a daemon process called crond to regularly check the content of files in /var/spool/cron, then executes the commands in them on the specified time. Users can use command "crontab" to create, modify or remove these files.

For example, create a file called "cronFile" with content "00 9 23 Jan * HappyBirthday", then run command

    crontab cronFile

After that, system will run program "HappyBirthday" at 9:00 in every January 23.

### Run program once at a certain time

Like crond, at runs commands at a certain time, but only once.

Syntax:

     at [ -f File ] Time

"File" contains the commands wanted to execute. Users may also input commands directly from keyboard:

     $ at 12:00
     at> mailto Roger -s ″Have a lunch″ < plan.txt
     at> (Ctrl-D)
     Job 1 at 2000-11-09 12:00

After that, system will automatically send an email to Roger with title "Have a lunch" and body text from plan.txt, at 12:00 in November 9, 2000.

## Reference

[ubuntu 开机启动时自动运行程序](http://m.oschina.net/blog/38766)
