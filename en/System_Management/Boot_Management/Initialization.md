---
title: Initialization
description: 
published: true
date: 2022-05-17T03:26:39.858Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:55:58.387Z
---

## Introduction

In Unix-based computer operating systems, init (short for initialization) is the first process started during booting of the computer system. It has several characteristics:

* Its process ID (PID) is always 1

* It remain existed until the system is shutted down

* It is the direct or indirect ancestor of any other process, and takes any orphaned process

Init is started by the kernel using a hard-coded filename; a kernel panic will occur if the kernel is unable to start it.

In Unix systems such as System III and System V, the design of init has diverged from the functionality provided by the init in Research Unix and its BSD derivatives. The usage in most Linux distributions employing a traditional init rather than a recent variant such as systemd is somewhat compatible with System V, while some distributions, such as Slackware, use BSD-style startup scripts; or deepin, use systemd - a new daemon system which attracts most of  the desktop users these days.

## BSD-styled init

BSD-styled init runs the initialization shell script located in /etc/rc, then launched getty on terminals under the control of /etc/ttys. There are no runlevels; the /etc/rc file determines what programs are run by init. The advantage of this system is that it is simple and easy to edit manually. However, new software added to the system may require changes to existing files that risk producing an unbootable system.

It is worth noting that modern BSD variants have long supported a site-specific /etc/rc.local file that is run in a sub-shell near the end of the boot sequence. This reduce the risk that the whole system is broke by changes in /etc/rc. Then start/stop scripts provided by third-party software can be installed to local repository /etc/rc.d (which is usually done  by ports collection/pkgsrc).

FreeBSD and NetBSD now use  rc.d as their default directory for init. Like Sys-V, any user-level  startup script is splitted into sub-scripts, and the executing order is determined by rcorder according to their dependencies.

## Sys-V styled init

Sys V Init checks the "initdefault" entry  in file /etc/inittab, then switch  to that default Runlevel. If the entry does not exists, user will be dropped to console, and need to manually select a Runlevel.
Runlevel is usually a number, ranged from 0 to 6, representing the running mode of Unix and Unix-like operating system (e.g. Linux). In deepin linux, the relation of Runlevel and the path of startup scripts is shown as following:

|  Path of startup scripts | Runlevel |
| :------: | :------: |
| /etc/rc0.d | 0 |
| /etc/rc1.d | 1 |
| /etc/rc2.d | 2 |
| /etc/rc3.d | 3 |
| /etc/rc4.d | 4 |
| /etc/rc5.d | 5 |
| /etc/rc6.d | 6 |

## Description of different Runlevel

As for deepin, Runlevel 0 ~ 6 represent following behaviors:

| Runlevel | System Behavior |
| :------: | :------ |
| 0 | All process are terminated. The machine is halted |
| 1 | Single user mode. For system maintenance. Only a few process is running, and no service is started |
| 2 | Multiple user mode. Similar to Runlevel 3, except that NFS service is not started |
| 3 | Multiple user mode. Default Runlevel of system. Allow multiple login |
| 4 | Reserved for user customization |
| 5 | Multiple user mode. X server is launched after startup process, providing a graphical interface for login |
| 6 | All process are terminated. The machine restart |

## Managing Runlevel

### View current Runlevel

The default Runlevel of deepin is 5. Use runlevel command in terminal  to show current Runlevel:

      runlevel

### Change Runlevel

Run following command in terminal:

    init [0123456Ss]

The parameter after "init" represent the Runlevel you would like to switch to.

**Notice: init 0 = shutting down; init 6 = restarting.**

### Change default Runlevel

File /etc/init/rc-sysinit.conf needs to be modified. Run in terminal:

    sudo gedit /etc/init/rc-sysinit.conf

Find the following line

    env DEFAULT_RUNLEVEL=2

And change the number into the level you prefer.

**Notice: Please do not change default Runlevel to 0, 4 or 6. Otherwire, system may not start up normally.**
