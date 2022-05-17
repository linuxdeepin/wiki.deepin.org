---
title: User_and_group
description: 
published: true
date: 2022-05-17T02:30:50.899Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:28.419Z
---

## Summary

Each user logined to Linux acquires at least two IDs: user ID (UID) and group ID (GID). This entry gives bref introduction to management of user and group under Linuz.

## User

### Type of user

User is a set of permission to access system resources. A user can login to system by providing their account information, which includes user name and password. After login, Linux system will identifies users by checking their user identifiers (UIDs), rather than their name. There are generally three types of user:

| Type of user| UID range | Description |
| :------: | :------: | :------ |
| Super user | 0 | "root" user, has all permission to the system; disabled to login by default |
| System user | 1 ~ 999 | Limited permission is granted in order to finish specified task |
| Normal user | 1000 ~  4294967295 | Users created by super user and systemusers; only unprivileged actions are allowed |

### /etc/passwd

The /etc/passwd file is a text file with one record per line, each describing a user account. Each record consists of seven fields separated by colons. These fields, in order from left to right, are:

    User name: Password :User ID: Main group ID : User's full name : Home directory : Login shell

Description:

* User name: name used to login
* Password: in old UNIX system, this field contains encrypted password of user. For safety reason, modern Linux system stores password in /etc/shadow, lefts this field an "x" or an "*"
* User ID: an integer used by Linux kernel to indentify the user
* Main group ID: an integer used by Linux kernel to indentify the group that the user belongs to
* User's full name: string shown when user logins; if no full name is provided during creation of account, then use user name for this field
* Home directory: used as working directory after user has logined
* Login shell: used as default shell after user has logined; this field is often set to "bin/bash"

Typical examples in /etc/passwd file:

    root:x:0:0:root:/root:/bin/bash
    apache:x:48:48:Apache:/var/www:/sbin/nologin
    tomcat:x:91:91:Apache Tomcat:/usr/share/tomcat6:/bin/sh

Note: for more details about /etc/passwd, type ` man 5 passwd ` in terminal.

### /etc/shadow

/etc/shadow is used to increase the security level of passwords by restricting the access to hashed password data to super user only. This  reduce the likelihood of brute-force attacks by making the list of hashed passwords unreadable by unprivileged users.

The /etc/passwd file is also a text file with one record per line, each describing a user account. Each record consists of nine fields separated by colons. These fields, in order from left to right, are:

    User name : Encrypted password : Days of last password change : Days until change allowed : Days before change required : Days warning for expiration : Days before account inactive : Days when account expires : Reserved field

Description:

* User name: identical to user name in /etc/passwd for the same user; it must not be empty
* Encrypted password: hashed password; it must not be empty. If this field is "x", then this user cannot login to the system.
* Days of last password change: days from 1970-01-01 to the last change of password. You can change password using passwd, then see changes in this field
* Days until change allowed: the number of days the user will have to wait before it will be allowed to change her password again.
* Days before change required: the number of days after which the user will have  to change her password.
* Days warning for expiration: The number of days before a password is going to expire  during which the user should be warned.
* Days before account inactive: The number of days after a password has expired during which the password should still be accepted.
* Days when account expires: days from 1970-01-01 to the expiration of the account.
* Reserved field: reserved for further usage

Examples:

    beinan:$1$VE.Mq2Xf$2c9Qi7EQ9JP8GKF8gH7PB1:13072:0:99999:7:::
    linuxsir:$1$IPDvUhXP$8R6J/VtPXvLyXxhLWPrnt/:13072:0:99999:7::13108:

Note: for more details about /etc/shadow, type ` man 5 shadow ` in terminal.

### User management

#### Add user

To add user "hilbert", execute in terminal:

    sudo useradd hilbert   

Following record will be added in /etc/passwd:

    hilbert:*:1001:1001::/home/hilbert:/bin/sh

Note that the password field is "*". This account is disabled until a password is assigned.

You can also add extra parameters to "useradd" command. For example, in order to set the main group of "hilbert" to "faculty", the secondary group to "famous", and to create home directory when it does not exists, you can execute:

    sudo  useradd -c "David Hilbert" -d /home/math/hilbert -g faculty -G famous -m -s /bin/sh hilbert

Following record will be added in /etc/passwd:

    sudo  hilbert:x:1001:1001:David Hilbert:/home/math/hilbert:/bin/sh

The UID assigned is higher than the largest existed UID in system. And for /etc/shadow, following record will be added:

    sudo hilbert:!:15597:0:99999:7:::

useradd will also append "hilbert" to group "faculty" and "famous" in /etc/group file, create directory /home/math/hilbert and initialize home directory according to the content of  /etc/skel.

Use `useradd -D` to see the default setting of useradd. Combine -D flag with other options to adjust this default values.

Another way to create user is to use "adduser" command:

    sudo adduser hilbert   

This will launche a wizard for creating user. No password or login shell will be assigned.

Note: useradd is an ELF executable, which creates  a group of the same name as the user name, but it does not create home directory under /home, either not assign new password to user account. adduser, on the other hand, is a interactive perl script, which provides user settings by interaction.

#### Summary

adduser is more suitable for new user, as it does not requires verbose option, simply questions & anwsers, though it is time-consuming. useradd is suitable for advanced users, as the majority of requirements can be achieved by one command.

#### Modify user

Use `usermod` to change the information of user account.

Syntax:

    usermod [-LU][-c <comment>][-d <login directory>][-e <expiration date>][-f <days before disabled>][-g <main group>][-G <group>][-l <new user name>][-s <shell>][-u <user ID>][user name]

Example:

    sudo usermod -e 2015-07-04 hilbert ## Set account "hilbert" to expire on 2015-07-04
    sudo usermod -G staff newuser2 ## Add "newuser2" to group "staff"
    sudo usermod -l newuser1 newuser ## Change the name of "newuser" to "newuser1"
    sudo usermod -L newuser1 ## Lock account "newuser1"
    sudo usermod -U newuser1 ## Unlock account "newuser1"

Note:

* You cannot change user name of the online user
* When changing user ID using usermod, make sure that this user is not executing program on this computer
* You may also need to manually modified some configuration, such as crontab or NIS.

#### Delete user

Use userdel to delete user account. For example, to remove account "hilbert", execute in terminal:

    sudo userdel hilbert    ## Delete records of "hilbert" in passwd, shadow and group; do not remove hilbert's home directory
    sudo userdel -r hilbert ##  Delete records of "hilbert" in passwd, shadow and group; also remove hilbert's home directory

## Group

### Introduction

In order to facilitate sharing of file and resources, "group" is  introduced to Linux system. Each Linux user belongs to a main group, and each group has an group identifier (GID). All information about groups is store in /etc/group.

When creating user, a group of the same name is also created, and includes this user by default. Thus, every user has its own group. Users can also join other groups to access certain resources.

If a file belongs to one group, then it can be accessed by all members of this group.

### /etc/group

Group information is stored in /etc/group, which controls the grouping of users. The fields of each records are:

    Group name : Password : Group ID : Members

Description:

* Group Name: name of the group; unique in /etc/group
* Password: encrypted password of the group; generally empty or "x" for Linux system
* Group ID: group identifier
* Memebers: list of users that  belong to this group, seperated by ","; empty if this groups is owned by user  whose UID equals to GID

Typical examples in /etc/group file:

    more /etc/group 
     root:x:0:
    daemon:x:1:
  
### /etc/gshadow

Group passwords are stored in /etc/gshadow, which can only be accessed by privileged users. Group password is rarely used in Linux system.

The fields of each records are:

    Group name : Encrypted password : Group ID : Members

Typical examples in /etc/group file:

    more /etc/group
    root:x:0:
    adm:x:4:cxbii

### Group management

#### Add group

Use `groupadd` to add group

Syntax:

    groupadd [-g Group ID [-o]] [-r] [-f] Group name

Example:

    sudo groupadd cjh ## Create group "cjh"
    sudo groupadd -g 344  cjh ## Create group "cjh", and generate in /etc/group an item whose GID is 344

#### Delete group

Use `groupdel` to delete group

Example:

    sudo  groupdel group cjh ## Remove group cjh

#### Set group password

Use `gpasswd` to set group password:

    sudo gpasswd group_name

#### Cancel group password

Use `gpasswd` to cancel group password:

    sudo gpasswd-r group_name

Note; after cancelling group password, only group memebers can use newgrp to switch to that group.

#### Modify group members

Use `gpasswd` to modify group members"

Syntax:

    gpasswd[-a User][-d User][-A User,...][-M User,...][-r][-R] Group name

Example:

    sudo gpasswd-a cxbii root ## Add "cxbii" to group "rooot"
    sudo gpasswd-d cxbii root ## Remove "cxbii" from group "rooot"

## Switching identity

Root user can do everything  we want, so it is reallly dangerous to login using root account. However, some tasks can only be done within root permission. This leads to the tools for switching identity: `su` and `sudo`.

Note: unless you clearly know what you are doing, please do not try following command! These command may cause irrevocable damage to your system!

### su

su (Switch user) lets a normal user have permission of other user temporarily. For normal user, password of the target user is required; for super user, no password is required.

Syntax：

    su [-fmp] [-c Command] [-s Shell] [--help] [--version] [-] [User [Arguments]]

Example:

     su -c ls root # Change identity to root, and change back after executing command "ls"
     su root -f # Change identity to root and execute new shell with "-f" option
     su - clsung # Change identity to "clsung" and switch working directory to clsung's home directory

Note: `su` change identity to root only; `su -` change both identity and shell environment.

**PROs and CONs**

PROs: Provide solution to identity switching

CONs: password needs to be providing when switching to specified user. If there are 10 users who need to switch to root, they all need to know root password. This increase the risk of password leaking, If one of these users does wrong thing, then the whole system might be in danger.

### sudo

sudo is frequently used in Linux to granti super user's privilege to normal user. It is good for control and audit root privileges, allow system administor to allocate its tasks to other users without sharing its password. It also allows control of permissions according to users'real needs, thus minimizing the privilege.

Syntax:

    sudo -K | -L | -V | -h | -k | -l | -vsudo [-HPSb] [-a Authorization Type] [-c Class|-] [-p Prompt] [-u User name | #UID] {-e File [...] | -i | -s | Command} 

For more details for sudo, type `man 8 sudo` in terminal.

**PROs and CONs**

PROs: provides solution to identity switching, while keeping root password intact;

CONs: privilege gained is valid only in 5 minutes. If no command is executed in the next 5 minutes since last command, then users need to re-run sudo to gain privilege again.

**/etc/sudoers**

The configuration of sudo is saved in /etc/sudoers. It is recommanded to use command `visudo` provided by sudo to modify this configuration file, as it while verify the changes to this file. If it finds anything incorrent, it will ask you to correct it before exit editing.

The default configuration of sudo shows below:

    # sudoers file.
    #
    # This file MUST be edited with the 'visudo' command as root.
    #
    # See the sudoers man page for the details on how to write a sudoers file.
    #
    # Host alias specification
    # User alias specification
    # Cmnd alias specification
    # Defaults specification
    # User privilege specification
    root ALL=(ALL) ALL
    # Uncomment to allow people in group wheel to run all commands
    # %wheel ALL=(ALL) ALL
    # Same thing without a password
    # %wheel ALL=(ALL) NOPASSWD: ALL
    # Samples
    # %users ALL=/sbin/mount /cdrom,/sbin/umount /cdrom
    # %users localhost=/sbin/shutdown -h now

To grants normal user "support" of all privileges owned by root, append following line to the file:

     support ALL=(ALL) ALL

Then save your changes. Next time when user "support" logins, it can execute

    sudo su -

then provide its own password to switch to root identity.

## Frequently asked question

### I forgot my password, and I cannot login to system

Solution:

1. For normal installation, press Shift key during boot until GRUB Boot menu is shown. For Wubi installation, move cursor to deepin system in Windows Boot Loader menu, press Shift + Enter until the GRUB boot menu is shown.

2. Move cursor to deepin system (usually the first line), then press "E".

3. Press arrow keys untili the cursor is on the line "linux /boot/....".

4. Press "End" key to move the cursor to the end of the line, press "Space", then type the folloing content:

    rw init=/bin/bash

5. Press Ctrl + X to boot system, until a terminal is shown. Then execute:

    passwd YourUserName

For example, your user name is sam, then type `passwd sam` and press "Enter" key. Type your password twice, then "Enter".

6. Press Ctrl+Alt+Del to reboot, and login with your new password.

Note: This method is applicable for all Linux system using GRUB2 as its boot loader.

### Errors when elevate privilege using "sudo"

    xxx is not in the sudoer file. This incident will be reported.

This may be caused by incorrect configuration for sudo. Use "su" to switch to root, then execute:

    sudo gedit /etc/sudoers

Delete the original content, then copy the following text into editor, then save your changes.

    Defaults env_reset
    Defaults mail_badpass
    Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

    # Host alias specification
    
    # User alias specification
    
    # Cmnd alias specification

    # User privilege specification
    root ALL=(ALL:ALL) ALL
    # Members of the admin group may gain root privileges
    %admin ALL=(ALL:ALL) ALL
    
    # Allow members of group sudo to execute any command
    %sudo ALL=(ALL) ALL
    xxx ALL=(ALL) ALL  # Replace "xxx" here by the account that has sudo problem

### Why not root for normal user by default?

Root user has very powerful privileges. It is dangerous too! Thus it cannot act as a normal user, and most of display managers disable root login. If you really want to have root privilege for a short time, please use "sudo" command.

### No reponse when typing password in terminal

Linux puts password security in a high priority. When users type password in terminal, nothing is shown on the screen, including placeholders. Just type the character you want, then press "Enter"!

## Useful links

[linux之用户/etc/passwd和/etc/shadow文件](http://liuzhigong.blog.163.com/blog/static/178272375201152511312662/)

[Linux密码文件passwd和shadow分析](http://hi.baidu.com/stdyy/item/b466f8c3453cc62cef466538)

[Linux命令：usermod命令详解！](http://qingwang.blog.51cto.com/505009/237661)

[Linux添加用户组和删除用户组等指令](http://www.zdh1909.com/html/linux/11811.html)

[鸟哥私房菜](http://vbird.dic.ksu.edu.tw/linux_basic/0410accountmanager.php#account)

[sudo及其配置文件sudoers](http://wwdhks.blog.51cto.com/839773/812915)
