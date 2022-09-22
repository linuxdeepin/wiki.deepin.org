---
title: Deepin_FAQ
description: 
published: true
date: 2022-07-13T11:25:50.080Z
tags: 
editor: markdown
dateCreated: 2022-06-17T07:31:46.428Z
---

# Frequently Asked Questions

1. View the system version information:

    ```
    $ cat /etc/os-release
    ```
    For example, the command and output are as follows:
    ```
    $ cat /etc/os-release
    PRETTY_NAME="Deepin 20.6"
    NAME="Deepin"
    VERSION_ID="20.6"
    VERSION="20.6"
    VERSION_CODENAME="apricot"
    ID=Deepin
    HOME_URL="https://www.deepin.org/"
    BUG_REPORT_URL="https://bbs.deepin.org/"    
    ```

2. View the CPU information:

    ```
    $ lscpu
    ```

3. View the memory information:

    ```
    $ free -h
    ```

4. View the disk information:

    ```
    $ sudo fdisk -l
    ```

5. View the real-time system resource information.

    ```
    $ top
    ```
    
6. View the kernel version infornation:
   ```
   $ uname -r
   ```
   
7. View current username:
   ```
   $ whoami
   ```
  
8. How to change current user password ?
   ```
   $ passwd
   ```

9. View the ip address:
   ```
   $ ifconfig
   ```

10. View the system local and Keyboard Layout status:
    ```
      $ localectl status 
      System Locale: LANG=zh_CN.UTF-8
                     LANGUAGE=zh_CN
          VC Keymap: n/a
         X11 Layout: cn
          X11 Model: pc105
    ```
   
11. View the system date、time and timezone status:
    ```
       $ timedatectl status
                   Local time: 三 2022-07-06 17:25:24 CST
               Universal time: 三 2022-07-06 09:25:24 UTC
                     RTC time: 三 2022-07-06 17:25:24
                    Time zone: Asia/Beijing (CST, +0800)
    System clock synchronized: yes
                  NTP service: active
              RTC in local TZ: yes
    ```
    
12. View the hostname:
    ```
    $ hostname
    ```

13. How to restart the system?
    ```
    $ reboot
    ```
14. How to shut down the system ?
    ```
    $ poweroff
    ```
    
15. How to login the system as root ?
    ```
    $ sudo -i
     [sudo] password for Current_User: 
     Verification successful
     root@Hostname:~# 
    ```
    
16. How to upgrade the system ?
    ```
    $ sudo apt update && sudo apt dist-upgrade
    ```
