---
title: Shell
description: 
published: true
date: 2022-05-07T07:48:26.733Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:41:39.333Z
---

## 简介

在计算机科学中，Shell俗称壳（用来区别于核），是指“提供使用者使用界面”的软件（命令解析器）。Shell是UNIX/Linux系统的重要组成部分，是操作系统与外部最主要的接口。在UNIX/Linux下，Shell扮演了一个双重角色：

- shell管理你与操作系统之间的交互：等待你输入，向操作系统解释你的输入，并且处理各种各样的操作系统的输出结果。
- shell提供了你与操作系统之间通讯的方式。这种通讯可以以交互方式（从键盘输入，并且可以立即得到响应），或者以shell script(非交互）方式执行。shell script是放在文件中的一串shell和操作系统命令，它们可以被重复使用。本质上，shell script是命令行命令简单的组合到一个文件里面。

传统意义上的Shell指的是命令行式的Shell，以后如果不特别注明，Shell是指命令行式的Shell。

## 分类
Shell主要可分为命令Shell和图形Shell。

## 命令Shell

命令Shell全名为Command Line Interface shell ，即CLI shell。它本质上是一个命令解释器，类似于DOS下的command.com。它接收用户命令（如ls等），然后调用相应的应用程序。较为通用的shell有标准的Bourne shell (sh）和C shell (csh）。

虽然它表面上和Windows命令提示符相似，但是它却具备完成执行复杂程序的强大功能，用户不仅可以通过它执行命令、调用Linux工具，还可以把Shell作为一种编程语言，编写自己的程序。

常见的命令Shell有：

    bash / sh / ksh / csh（Unix/linux 系统）
    COMMAND.COM（MS-DOS系统）
    cmd.exe/ 命令提示字符（Windows NT 系统）
    Windows PowerShell（支持 .NET Framework 技术的 Windows NT 系统）

第一个Unix shell是由肯·汤普逊，仿效Multic上的shell所实现出来，称为sh。

**Bourne shell兼容的Shell**

- Bourne shell (sh) 史蒂夫·伯恩在贝尔实验室时编写。1978年随Version 7 Unix首次发布。
- Almquist shell (ash)
- Bourne-Again shell (Bash)
- Debian Almquist shell(dash)
- Korn shell (ksh) David Korn在贝尔实验室时编写。
- Z shell (Zsh)

**C shell兼容的Shell**

- C shell (csh) 比尔·乔伊在加州大学伯克利分校时编写。1979年随BSD首次发布。
- TENEX C shell (tcsh)

**其他类型的Shell**

- fish，第一次发布于2005年。
- rc shell (rc) Plan 9 系统的shell，由Tom Duff在贝尔实验室时编写。随后移植回 Unix 和其他的操作系统。
- es shell (es) 一个函数式编程的rc兼容shell，编写于二十世纪九十年代中期。
- scsh (Scheme Shell)

**仅存于历史的Shell**

- Thompson shell (sh) 第一个 Unix shell，由肯·汤普逊在贝尔实验室时编写。1971年至1975年随Unix第一版至第六版发布。
- PWB shell (sh) Thompson shell 的一个版本，由John Mashey和他人在贝尔实验室时改进。1976年随PWB UNIX发布。

## 图形Shell

图形Shell全名字为Graphical User Interface shell 即 GUI shell,应用最为广泛的 Windows Explorer （微软的windows系列操作系统），还有也包括广为人知的 Linux shell

图形Shell又可分为窗口管理器和桌面环境。

### 窗口管理器

窗口管理器只提供简单的窗口管理功能，一般的窗口管理器可以直接使用，有些则作为桌面环境的一部分。常见的窗口管理器有：

- Compiz：基于OpenGL的混合型窗口管理器。
- KWin：基于OpenGL的混合型窗口管理器。
- I3：平铺式的窗口管理器。
- Xmonad：平铺式的窗口管理器。
- Enlightenment：功能强大，仅次于桌面环境的窗口管理器。
- Openbox：可高度定制的下一代窗口管理器。
- Fvwm：以最小的内存换取最多的特性的窗口管理器
- Fluxbox：轻量级的窗口管理器。

### 桌面环境

桌面环境:使用一个窗口管理器管理程序窗口.并且自带一系列的组件,提供较完善的软件需求,而且还提供系统管理设置等诸多重要模块.

- 深度桌面环境：基于Qt/C++和GO开发的全新桌面环境。
- Gnome Shell：世界最为流行Gnome桌面环境。
- KDE：优雅、华丽的桌面环境。
- Unity：ubuntu专属的桌面环境。
- Xfce：轻巧优美的桌面环境。
- LXDE：极度简洁的桌面环境。
- MATE：基于Gnome2的开发的桌面环境。
- Cinnamon：基于Gnome Shell的开发的桌面环境。
- Razor-qt：基于QT开发的桌面环境。

### 两者关系

桌面环境必须使用一个窗口管理器管理程序窗口,桌面环境没有窗口管理器就不能运行.

窗口管理器可单独运行.

## 相关链接

[维基百科:Unix shell](http://zh.wikipedia.org/wiki/Unix_shell)

[百度百科:shell](http://baike.baidu.com/view/849.htm)
