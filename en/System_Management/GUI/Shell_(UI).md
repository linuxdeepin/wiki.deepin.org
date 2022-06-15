---
title: Shell_(UI)
description: 
published: true
date: 2022-05-17T04:25:10.752Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:14.150Z
---

## Summary

In computer science, shell refers to the software that provides interface to operate for users. Shell is also an important part of Unix/Linux system, as it is the main interface between the operating system and the external environment. Shell plays two roles in Unix/Linux:

* Managing of interactions between OS and users: waiting for input, inteprete the input and output the result.

* Providing methods of communications, which can be either interactive (like keyboard input) or non-interactive (like the shell script, a commbinations of commands).

## Classification

There are generally two kinds of shell: command shell and graphical shell. The former is often implied when no other indication is given.

### Command shell

Command shell is also called command line interface (CLI) shell. It is in fact a command intepretor, receives commands from users, then executes the relative program. Some popular shells are

* Standard Bourne shell (sh) / C shell (csh) / ksh / bash for Unix / Linux

* Command.com for MS-DOS

* cmd.exe for Windows NT

* PowerShell for Windows NT that supports .NET Framework

The Unix / Linux shell may look like traditional command line in Windows, however it can be used to fulfill powerful and complex functions. Users can also use shell language to write their onw prorams.

### History

The first Unix shell, called "sh", is written by Ken Thompson in the inspiration from shell of Multic.

The Borune shell (sh) is written by Steve Bern in Bell Laboratory, published with Unix Version 7 in 1978. After that, a lot of command shell has emerged with the compatibility to Bourne shell:

* Almquist shell (ash)
* Bourne-Again shell (Bash)
* Debian Almquist shell (dash)
* Korn shell (ksh) by David Korn in Bell Laboratory
* Z shell (Zsh)

The C shell (csh) is written by Bill Joey in UCLA, published with BSD in 1979. The TENEX C shell (tcsh) is a shell that is compatible to csh.

Some other kinds of shell:

* fish: first published in 2005
* rc shell (rc): shell used in 9 project system, written by Tom Duff in Bell Laboratory, and further ported to Unix and other operating system
* es shell (es): a rc-compatible shell that supports functional programming, written in 90s in the 20th centry
* scsh (Scheme Shell)

Some old shells that are no longer used:

* Thompson shell (sh): The first Unix Shell, written by Ken Thompson in Bell Laboratory. Published in the first to six version of Unix from 1971 to 1975
* PWB shell (sh): A version of Thompson shell enhanced by John Mashey and others in Bell Laboratory, powered with PWB Unix in 1976.

## Graphical shell

Graphical shell is also called graphical user interface (GUI) shell. The most known GUI shells are Windows Desktop (Windows Explorer) and Linux shells.

Graphical shell consists of window manager and desktop environment.

### Window manager (WM)

The window manager offers functions of managing windows, which can be independent from or integrated into a desktop environment. Some popular window manager in Linux are:

* Compiz: Mixed window manager based on OpenGL
* KWin: Mixed window manager based on OpenGL
* I3: Windows manager using tile mode
* Xmonad: Windows manager using tile mode
* Enlightenment: Powerful window manager that offers many functions of desktop environment
* Openbox: Highly customizable window manager of the next generation
* Fvwm: Full featured window manager with minimum memory usage
* Fluxbox: Lightweight window manager

### Desktop environment (DE)

A desktop environment is a series of component that has a window manager for managing windows, meets full software requirements and provides modules like system settings. Some popular desktop environments are:

* Deepin desktop environment (DDE): a nouveau desktop environment developed using Qt/C++ and Golang
* Gnome Shell: The most popular desktop environment based on Gtk
* KDE: Elegant and full featured desktop environment
* Unity: Desktop environment developed for Ubuntu
* Xfce: Light and practicle desktop environment
* LXDE: Lightweight dekstop environment
* MATE: Desktop environment derived from Gnome 2
* Cinnamon：Desktop environment based on Gnome Shell
* Razor-qt: Another desktop environment developed using QT

### The relationship of WM and DE

A window manager can run independently, while a desktop environment must have a window manger to function normally.

## References

[维基百科:Unix shell](http://zh.wikipedia.org/wiki/Unix_shell)

[百度百科:shell](http://baike.baidu.com/view/849.htm)
