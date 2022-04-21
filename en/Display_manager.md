---
title: Display_manager
description: 
published: true
date: 2022-04-21T03:55:19.401Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:55:16.766Z
---

[[zh:登录管理器]]

## Introduction

Display manager, also known as login manager, provides a interface for users to input their user name and password. When account authetication is successful, it starts a session (window manager or desktop environment), such as GNOME shell, KDE Plasma or Deepin desktop environment.

A graphics server can only be managed by one display manager in one time. This, however ,does not prevent several display managers to be installed in one system.

## Types of display manager

There are many kinds of display manager, for example:

- LightDM: default display manager used by deepinn and Ubuntu
- GDM：default display manager used by Gnome desktop environment
- SDDM: default display manager used by KDE Plasma desktop

## Settings

### Switching display manager

deepin offers LightDM as its default display manager. If you want to change it to another one, please follow the instruction below.

### The first method

When installing display manager, a dialog will be shown to let you choose preferred display manager. Press TAB to choose one you would like to use, then Enter.

### The second method

Launch the terminal, then execute:

    sudo dpkg-reconfigure gdm  # You can change  "gdm" to any other display manager you like

After that, you will see the same dialog shown to you in the installation process, where you can change the default display manager.

### Problem of display manager

#### System startup error: Checking Battery State ...

If you cannot login through graphical interface, please press Ctrl-Alt -F1 to enter tty1, login with your user name and password, then execute:

    sudo apt-get install gdm
    sudo dpkg-reconfigure gdm　

Use TAB to choose "LightDM", then press Enter. Press Alt-F7 to resume to graphical interface.

## References

[维基百科：显示管理器](http://zh.wikipedia.org/wiki/X%E6%98%BE%E7%A4%BA%E7%AE%A1%E7%90%86%E5%99%A8)

[Debian 维基“显示管理器”页面](http://wiki.debian.org/DisplayManager)

[Deepin BUG管理:0000650: 关于登录的两个疑问](http://www.linuxdeepin.com/mantis/view.php?id=650)

[Ubuntu 12.04启动错误：Checking Battery State ...](http://www.linuxidc.com/Linux/2013-02/80128.htm)