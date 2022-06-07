---
title: Linux桌面演进
description: 
published: true
date: 2022-06-07T02:06:21.665Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:37:39.141Z
---

## 前言

当我们坐在 Debian、Fedora、Suse 等发行版本最新版本前，看着桌面操作的各种华丽效果，享受着各种易用的图形界面应用给我们带来的便利，是否对我们面前的这个操作系统如何而来有过思考？

我们是否考虑到在感恩节的时候，对隐藏在这个操作系统背后努力奉献时间的人们抱有感恩之心呢？Linux Kernel，一路走来，风雨兼程，岁月有痕。


## 历程

### 1991 年：生于毫末

在故事的开头，要介绍下 Unix。Unix 由 Ken Thompson 和 Dennis Ritchie（已离世）于 1969 年开发。此后，整个 80 年代，基于 Unix 的大量项目应运而生。而后，RMS 发起 GNU 项目，BSD 诞生，Andrew S Tanenbaum 教授开发了用于教学的 MINIX（Mini-Unix）。

1991 年，年轻的芬兰学生 Linus Torvalds 将其开发的内核带到这个世界。关于 Linux 开始的开始，有很多传说，如 Linus 在玩 MINIX 时不小心擦除了分区上的数据，这惹怒了 Linus，Fuck，自己搞一个操作系统~

另一个传说：是他在改进 MINIX 功能时，不小心开发了自己的内核。

如论事实如何，最终 Linux 带给了这个世界难以想象的变革。 此时，Manchester 计算机中心使用一块组合的 boot/root 磁盘，创建了第一个 Linux 发行版本，名为 MCC Interim Linux。

### 1992~1994：发行版本大佬创世


在不长的时间，1992~1994 年间，我们看到了最具有影响力的现在 Linux 桌面发行版本的创世：Slackware，Red Hat 和 Debian。此时，Linux 内核版本也升到了 0.95 ——第一个可以运行 X 窗口系统的内核版本。

Slackware 是第一批采用新内核的发行版本之一。Slackware 开始是以“Softlanding Linux System”（SLS）形式开始的，由 Peter MacDonald 创建于 1992 年。

SLS 走在了时代的前面，它不仅是第一个使用了 0.99 内核版本，也同时采用了 TCP/IP 栈和 X 窗口系统。SLS 不久的时间，它由 Patrick Volkerding 的 Slackware 取代，Slackware 成为寿命最长的 Linux 发行版本。

SLS 不仅孕育了 Slackware。因其糟糕的交互，其他的用户默默离开，开始创建自己的 Linux 发行版本新分支。1993 年，lan Murdock 发布了 Debian Linux。Debian 这个名称由他的女朋友名字 Debra Lynn 和自己的名字组合而成。

随着 Slackware 的演进，一些商业公司开始出现。1994 年，Software und System-Entwicklung 公司创建，可能 S.U.S.E 更为大家所熟知。

1994 年 11 月 3 日，Red Hat 商业 Linux 成立。创建人 Marc Ewing，RedHat 是他根据大学时戴的一顶帽子命名。

1994 年 3 月 14 日，Linux 1.0.0 发布，代码共计 176, 250 行。



### 1995~1999：Gnome 和 KDE 来临

在这个阶段，一些优秀的发行版本从上述“大头”发行版本中分离出来，Linux 这个大家庭的分支越来越繁茂。1996 年，也发生了著名的“企鹅袭人”事件:）


这里要提下 Jurix Linux。它：是第一个包含了脚本安装器的发行版本，是完全支持 NFS 的发行版本之一，是第一个使用 EXT2 的系统。其更重要的是，成为了 SUSE Linux 的基础系统。

该阶段，基于 Red Hat 的 Linux 系统，如 Caldera，Mandrake，TurboLinux，YellowDog 和 Red Flag（红旗）出世了。Linux 内核版本也从 1.2.0 长到了 2.2 。2.0 版本内核的一些重要功能奠定了 Linux 作为 IT 行业服务器系统的基石，如支持 SMP、更好的内存管理、支持更多类型的处理器等。2.2 版本的内核对 SMP 支持进行了改进，同时也支持 了 PowerPC 架构，支持对 NTFS 的只读功能。

基于 Debian 系列的发行版本，虽然不如 Red Hat 这样的对手活跃，但因在其服务器方面的易操作性也形成了自己的特色。桌面上的友好，也使 Debian 系成为人们追逐的对象。

在这 5 年里，最重要的事情应该是 KDE 和 Gnome 的诞生。KDE 于 1996 年 Tübingen 大学的 Matthias Ettrich 创建，该项目不仅是编写一套常用应用，更是创建一整套桌面环境。

KDE 1.0 在 1998 年发布，Mandrake 第一个采用。2000 年，2.0 发布。

Miguel de Icaza 和 Federico Mena 宣布开发基于 GTK+ 的桌面环境和应用程序，也就是 Gnome 项目。

据坊间传说，RedHat 成为第一个采用 Gnome 桌面环境的系统。Gnome 逐渐为人们所接受，走上快速发展之路。2000 年 5 月，Gnome 1.2 Bongo 发布。


Oracle 和 Sun 公司也宣布他们的服务器官方支持 Linux 版本。

### 2000~2005：Live（试用）模式的诞生

在这个 5 年，Linux 驱动的计算机数量激增，常常见诸报端。各种新应用层出不穷，更为重要的是，出现了一种 live 式的发行版本。

Knoppix，由 Klaus Knopper 开发的 Debian 系发行版本，当时红极一时。它值得夸耀的就是：Knoppix 可以直接从 CD 启动。 2000 年 9 月 30 日发布的 Knoppix 1.4，可以直接插入 PC 启动。Knoppix 成为其他发行版本模仿的标杆，开始默默无闻，渐渐也有了自己的分支。

很多 Linux 发行版本长得和 MS Windows 越来越像。此时，为了让人们了解 Linux 如何工作，而不是沉陷于图形界面和现成的发行版本不能自拔，LFS 项目创立了。创建者 Gerard Beekmans 写了 LFS 手册，帮助人们如何从源码一步步构建自己的 Linux 系统。

2000 年，Linux 基金会成立，以便更好地保护 Linux 的自由，让其健康成长。Linux 基金会 2000 年开始赞助 Linus 的工作和社区发展，不断努力帮助 Linux 成长，坚决维护 Linux 内在的自由、合作的核心价值观。

2.4 版本内核，支持 USB、PC 卡、ISA 插拔和播放，同时增加了对蓝牙、RAID 和 EXT3 的支持。2.4.x 系列版本内核是维护期最长的内核，在 2011 年以 2.4.37.11 版本结束。

Red Hat 公司上市，也不断探寻更加商业化的途径。于是，RedHat 企业版本诞生，Fedora Core 成为社区发行版本。因 RHEL 开源，一些爱好者或组织也利用这些源码制作自己的发行版本，如 Cent OS、CERN、Oracle Linux 和 Scientific Linux 等。 介绍下 CRUX，Crux 在别家发行版本越来越像 Windows，想要替代 Windows 时，它却独辟蹊径、特立独行，不断将自己瘦身，成为最受欢迎的最小化发行版本。它也成为 Arch Linux 的基础操作系统。

2.6 版本的内核支持 PAE、新的 CPU、64 位支持改进，支持 16 TB 大小的文件系统容量，EXT4 文件系统等。

虽然各种 Linux 发行版本努力保持用户和 PC 间的和谐，但于普罗大众还是有一定的距离。此时，倡导更加人性化的 Ubuntu 诞生。Ubuntu 第一个版本为 4.10，于 2004 年 10 月 20 日发布。

### 2006~2012：Ubuntu 的起起落落

此时发行版本的数量爆炸式增长。虽然各种新生力量猛攻，但老版本依然宝刀不老、砥砺中坚。

2006 年， Linux Mint 1.0，Ada 发布，掺杂了 FOSS 和版权软件。

KDE 4.0 发布，但因缺乏稳定，饱受诟病。

2009 年发布的 KDE 4.2 更加新潮美观，这让人们忘记了过去的痛苦。9 月 23 日，最最流行的 Linux 操作系统– Android 发布。

在此阶段，Ubuntu 势力不断稳固，坐上了第一流行操作系统的交椅。但在 11.04 发布的 Unity 桌面环境，让人另眼相看。大家一开始几乎无一不对它产生厌恶之感，不是 Gnome 3，也不是 KDE4，生的倒也奇葩。

2011 年 4 月，GNOME 3.0 发布，大家又是一片哗然。

### 未来：谁人能料？

未来，谁人能料？只拭目以待！但也请不要只做一个旁观者。今日你我所做，将是后人一壶浊酒中笑谈的历史。

## 来源链接

引用于 岁月有痕：Linux 桌面演进 
