---
title: System_service
description: 
published: true
date: 2022-06-15T07:27:10.424Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:24.483Z
---

## Summary

Service is the program that resides in memory and provides special functions for system or network. Unix-like system uses daemon processes to serve  as services, thus a "service" is sometimes used to refer to a "daemon" in system process list.

Generally, once we start up a Linux system and enter command line mode or graphical environment,  a lots of services have already being run to make the system function properly.

## Service

### Categories of services

#### According to the way of starting and managing

##### Stand alone service

The stand alone service can be started alone, and occupy system resources until it is stopped manually. Because it resides in system memory, it can quickly response to the request from client.

Common services of this kind: WWW daemon (httpd), FTP daemon (vsftpd) and so on.

##### Super daemon

The super daemon is a special daemon process used to manage other service. When there is no request from client, the super daemon keeps other services stopped; when a client request comes in, it wakes up the related service and stops this service after tasks have been finished.

The advantages: services are started and stopped on demand and under control, which is similar to the firewall on the Internet. Also, services do not occupy system resources all the time.

The disadvantages: system needs extra time to load and unload the service on demand, prolong the time of response.

Common services of this kind: telnet service.

#### According to the way of dealing request

##### Multi-threaded service

The daemon that handles multiple requests at a same time.

##### Single-threaded service

The daemon that handles only one request at a same time. The other request must be pending to the waiting list, or fail.

#### According to the working mode

##### Signal-control service

The Daemon that is managed by signals sent from system. Once a request comes in, it is immediately waken up to deal with it. Example: Unix printing system daemon (cupsd).

##### interval-control

The daemon that is waken by a regular time interval to do its work. Users need to configure the time and the tasks, so that the daemon will finish them at a specified time. Example: atd and crond.

Note: if you are interested in developing programs, use `man 3 daemon` to see details information of system daemons.

### Nomination of services

Generally, services have a "d" at the end of their names, which indicates they are realized by daemon processes.

### The way of starting

In order to provide services, daemons usually need to be started with the environment correctly configured. Also, for management of these services, their running status like PIDs are recorded in directory /var/run. To have all these things done, most Linux distributions provides pre-configured shell scripts to start these service. Just run this script, and it will:

* detect the running environment

* analyze configurations

* write process information

* lock status file

and many things so on.

The common places where these shell scripts are put:

/etc/init.d/* : holding almost all startup scripts.

/etc/sysconfig/* : configuration scripts of each system service.

/etc/xinetd.conf and /etc/xinetd.d/* : configuration files of super daemons

/etc/* : Configuration files of other services.

/var/lib/* : databases of services. For example, /var/lib/mysql is used by MySQL database.

/var/run/* : Running status of each program. It is used by shell scripts to manage the process that have already been started, either by sending signals or invoke system calls.

## Common service

* acpi-support   Support of Advanced Configuration and Power Management Interface (ACPI)
* acpid acpi    Daemon of ACPI
* apmd acpi    Extension of ACPI
* alsa    Subsystem for sound management
* alsa-utils    Utilities for ALSA
* cron    Task scheduler system
* anacron    Subsystem of cron, automatically run delayed tasks after system boot
* atd    Task scheduler system similar to cron. Recommended to disable.
* binfmt-support    Support of other binary formats.
* bluez-utiles    Support of Bluetooth devices.
* bootlogd    Recording journals during boot. Recommended to enable.
* syslog-ng    System journaling. Recommended to enable.
* klogd    Similar to sylog.
* sysklogd    Recording system log as well as kernel log.
* cupsys    Common UNIX Printing System.
* dbus    Message bus system. Very important to desktop applications.
* dns-clean    Clean DNS information when using dial-up connections.
* evms    Volumn management system for enterprise.
* fetchm    User proxy for fetching mails.
* gdm    GNOME login manager
* gpm    Mouse support in terminal.
* halt    Shutdown the system.
* hdparm    Scripts for adjusting hard disk (using configurations from /etc/hdparm.conf)
* hibernate    System hibernation.
* hotkey-setup    Support of Fn keys on laptop.
* hotplug and hotplug-net    Plug-and-play support. Recommended to leave them untouched.
* ifrename    Scripts for rename of network interfaces. Recommended to enable if you have several network card.
* inetd    A super daemon for managing other services (using configurations from /etc/inetd.conf)
* linux-restricted-modules-common    Support of restricted system module.
* lvm    Logical volume management
* makedev    Creating device files. Very import.
* mdamd    Management of disk array.
* module-init-tools    Load modules from /etc/modules. Recommended to enable.
* networking    Networking support (using configurations from /etc/network/interfaces). Very important.
* ntpdate    Synchronization of time. Recommended to disable.
* pcmcia    Support of pcmcia device.
* powernowd    Advanced power management of mobile CPU.
* ppp    Management of dial-up connections.
* ppp-dns    DNS service for dial-up connections.
* readahead    Read library files before they are used.
* reboot    Rebooting system.
* resolvconf    Automatic configuring DNS.
* rmnologin    Remove nologin.
* rsync    Rsync daemon.
* sendsigs    Send signals to processess during reboot and shutdown.
* single    Switch to single user mode.
* sshd    SSH server daemon.
* sudo    Check sudo status.
* udev    Userspace dev file system. Important.
* umountfs    Umounting file system.
* urandom    Random number generator.
* usplash    Offer an animation during startup.
* vbesave    Tools for configuration of video cards in BIOS.
* xorg-common    Set ICE socket for X service.
* adjtimex    Adjust kernel clock.
* dirmngr     Tools for managing certificate list.
* hwtools    IRQs optimization.
* libpam-devperm     Fix device file after system crashes.
* lm-sensors    Support of onboard sensors.
* mdadm-raid    RAID manager.
* screen-cleanup    A script for clearing screen during startup.
* xinetd    Super daemon for managing other services.

Note: /lib/linux-restricted-modules/ is used to store restricted modules such as closed source drivers. If you do not use any of these modules, please do not enable linux-restricted-modules-common service.

## Service management

### chkconfig

chkconfig is a command line tool for enabling and disabling system services.

Syntax:

        chkconfig [--add][--del][--list][Service]
        chkconfig [--level<Level>][Service][on/off/reset]

Options:

        --add    Add the specified system service
        --del    Delete the specified system service
        --level    Specify the level that the service is to be started in or stopped in
        --list    List all services available for chkconfig and their levels
        on/off/reset    Enable, disable or reset the specified service

See [Initialization](https://wiki.deepin.org/index.php?title=Initialization) for explanation of system runlevel.

Example:

To see status of each service in different runlevel:

         chkconfig --list

To list status of vsftpd in different runlevel:

         chkconfig --list vsftpd

To disable vsftpd in runlevel 3 and 5:

         chkconfig --level 35 vsftpd off

To enable vsftpd in runlevel 2, 3 and 5:

         chkconfig --level 235 vsftpd on

To disable unused services:

         chkconfig --level 235 cups off  ## For system that does not have a printer
         chkconfig --level 235 smb off ## For system that does not have a network connection
         chkconfig --level 235 sshd off ## For system that does not have remove login
         chkconfig --level 235 crond off ## For system that does not have regular tasks
         chkconfig --level 235 kudzu off ## For system that does not have new hardware installed

### Service

Use `service` to start, stop, restart and show status of system services.

Syntax:

    service Service Command [Options]

Options:

        Service    Name of the service to deal with; the name should be found in directory /etc/init.d/
        -status-all    Show status of all stand alone services
        --help | -h    Show helpful information

Example:

         service --status-all ## Show all services 

         sudo service httpd stop ## Stop service httpd

         sudo service httpd start ## Start service httpd

         sudo service httpd restart ## Restart service httpd

## Reference

[鸟哥私房菜:认识系统服务 (daemons)](http://vbird.dic.ksu.edu.tw/linux_basic/0560daemons.php)

[开源世界旅行手册:常见系统服务](http://www.ha97.com/book/OpenSource_Guide/ch15s04.html#id2824176)

[chkconfig命令](http://blog.csdn.net/youyu_buzai/article/details/3956845)
