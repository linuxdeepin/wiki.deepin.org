---
title: deepin下使用Ventoy安装Windows
description: 本文由论坛用户pzm9012分享，原帖地址：https://www.yuque.com/pzm9012/ct5ume/uf10gv
published: true
date: 2022-08-14T00:24:37.549Z
tags: 
editor: markdown
dateCreated: 2022-06-15T03:24:14.525Z
---

> 这是[原文章](https://bbs.deepin.org/zh/post/209123)的2022年重制版，也[发布在论坛上](https://bbs.deepin.org/zh/post/235065)，[转载在deepin wiki上](https://wiki.deepin.org/zh/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98FAQ/deepin%E4%B8%8B%E4%BD%BF%E7%94%A8Ventoy%E5%AE%89%E8%A3%85Windows)。感谢 [Jack](https://bbs.deepin.org/zh/user/206453) 和 [shenmo](https://bbs.deepin.org/user/223313) 对本次重制提供了支持。
> 教程仅供参考，请根据实际情况操作。

本文主要讲述如何在deepin下制作Ventoy启动盘和用Ventoy启动盘安装Windows（省略了部分细节，可以自行查阅其他教程了解）。

# 一、Ventoy是什么？

[Ventoy](https://ventoy.net/cn/index.html) 是一个制作可启动U盘的开源工具。Ventoy能轻松实现多镜像启动，支持常见的操作系统和硬件类型。Ventoy强大的插件功能可以让你对它进行自定义、扩展启动盘功能。

# 二、制作Ventoy启动盘

官方使用说明：https://ventoy.net/cn/doc_start.html    这里介绍[GTK/QT图形化界面](https://ventoy.net/cn/doc_linux_gui.html)的使用方式。

1. 准备一个8G或以上的U盘，将U盘中的所有文件复制到电脑上或备份到其他位置。
2. 进入[Ventoy下载页](https://ventoy.net/cn/download.html)，从说明中的链接选择一个进入(建议选蓝奏云或镜像站)，下载`ventoy-x.x.xx-linux.tar.gz`。

![1](https://storage.deepin.org/thread/202108190919393816_%E6%88%AA%E5%9B%BE_Navigator_20210819090650.png)

3. 解压这个文件，在文件管理器中打开解压后文件所在的文件夹，运行Ventoy安装程序。**方式一**：直接双击 `VentoyGUI.x86_64` 文件运行。 **方式二**：右击窗口内空白处，选择“在终端中打开”。

![img](https://storage.deepin.org/thread/202108190921059317_%E6%88%AA%E5%9B%BE_dde-file-manager_20210819090958.png)

在终端输入以下命令，并回车：

```bash
./VentoyGUI.x86_64
```

4. 确认U盘中文件已备份后，在“设备”处选择U盘，点击“安装”。

![t4.png](/图片存储/t4.png)

5. 安装完成后，U盘在文件管理器中显示的名称为Ventoy（可改）。只需将系统镜像复制到U盘的任意目录下（支持多个镜像文件），即可作为该系统的启动盘使用。 U盘存储其他文件的功能不受影响。



### 提示

1. 若用这种方式安装Ventoy失败，可以尝试换另外2种方式（见附录 4.1），再不行就换U盘、USB接口等。使用中有问题可阅读[常见问题](https://ventoy.net/cn/faq.html)、[文档手册](https://ventoy.net/cn/doc_news.html)或在[论坛](https://forums.ventoy.net/)发帖。
2. 有能力的用户可以使用[Vlnk](https://ventoy.net/cn/doc_vlnk.html)实现启动本地硬盘中的镜像文件。请阅读[文档手册](https://ventoy.net/cn/doc_news.html)，了解Ventoy的更多功能。
3. 使用启动盘（尤其是拷贝完文件）后，**一定要先安全弹出再拔下U盘**，否则可能使U盘文件系统损坏。如果没有在U盘中存储4GB以上大文件的需求（有些Windows 10/11镜像大小已超过4GB），可以先在任务栏上的托盘处（或文件管理器的侧边栏）卸载（弹出）U盘，再在文件管理器（或磁盘管理器）中右击U盘，点击“格式化”，将U盘（的第一个分区）文件系统格式化成FAT32，减少该问题的发生。

![t5.png](/图片存储/t5.png)

1. 深度文件管理器在操作大文件时可能存在容易卡死等问题。可以安装其他文件管理器（例如Cinnamon桌面环境的Nemo，在终端执行命令`sudo apt install nemo` 安装它）并使用它复制系统镜像等。
2. Ventoy启动盘也可用于安装deepin等Linux发行版，可以参考[我的deepin变形记](https://bbs.deepin.org/zh/post/228568)或[deepin常用资源整理 “2.1 系统安装”](https://www.yuque.com/pzm9012/ct5ume/nte586#QNdfM) 部分的文章。运行内存比较小的电脑建议启用swap（交换）分区或交换空间。

# 三、安装Windows

本文以使用[Edgeless PE](https://home.edgeless.top/)和[Dism++](https://github.com/Chuyu-Team/Dism-Multi-language)为例，介绍一种安装Windows的方法。

## 3.1  安装前的准备

1. 下载Windows 系统镜像。[微软官网](https://www.microsoft.com/zh-cn/software-download)提供了Windows 11/10/8.1 的磁盘映像（ISO）（选择简体中文的64-bit版本）。大部分原版Windows镜像可在[MSDN，我告诉你](https://msdn.itellyou.cn/)下载。(选择x64的，同一代中较新的操作系统，ed2k链接需要用Windows版迅雷等下载器下载)。也可以下载纯净的第三方Windows系统，如[Windsys](https://windsys.win/)， [ThirdWIN](https://third.win/)等。镜像文件可以不拷贝至U盘，但一定要放在deepin系统盘、数据盘以外的地方**(要能在Windows下看到)**。（如果不知道下载哪个版本，新电脑选择Windows 11/10，老电脑选择Windows 8.1/7）下载完成后，最好校验一下文件的完整性。
2. 访问[Edgeless 下载页](https://down.edgeless.top/)，点击“访问网页版”右侧小箭头里的“下载 ISO 镜像”，下载Edgeless的iso文件，并按照[手动制作](https://wiki.edgeless.top/v2/guide/burn_manual.html)教程操作。点击下载页的“访问网页版”，进入 `/插件包/磁盘数据`  ，下载分区助手的插件包，放在U盘的 `/Edgeless/Resources` 中。（你也可以用[FirPE](https://firpe.cn/page-247)，[下载它的iso文件](https://cloud.189.cn/web/share?code=uemAfebe63Uv) （访问码：ffs9）并复制到U盘，自带分区助手）其他的插件包根据需要获取，但**不要在系统中加载太多插件包**。（了解Edgeless的更多信息，请阅读[Edgeless文档](https://wiki.edgeless.top/v2/)）

1. 迁移电脑上的文件（与3.2 第2步结合操作）。
2. 访问电脑或者显卡、声卡等硬件的生产商网站，下载其Windows驱动程序。若找不到生产商提供的驱动，建议准备一个驱动安装程序和含多种常见驱动的驱动包。

## 3.2  用WinPE安装Windows

1. 在网上搜索了解“xx（品牌）电脑进BIOS按哪个键”（可以参考一下[这张表](https://www.coolexe.com/735.html)）。插入U盘，重启电脑，按对应的键进入BIOS，关闭安全启动（Legacy传统启动不需要），在启动项中选择U盘回车。在Ventoy引导菜单中上下键进择Edgeless，回车。

1. 进入Edgeless后，用分区助手（或DiskGenius）调整电脑上的分区（注意：**不要调整deepin系统盘和数据盘分区，删除除外；请谨慎操作分区，以免丢失文件**）。在文件资源管理器中迁移各个盘内的文件，最终使电脑中有一个32GB以上的NTFS分区。(传统启动需要设其为活动分区)

1. 打开Dism++，点击菜单栏上的“文件”>“释放镜像”。点击“浏览”，分别选中要安装的Windows镜像及安装到的分区，勾选“添加引导”和“格式化”，点击“释放”。

![t6.png](/图片存储/t6.png)

![t7.png](/图片存储/t7.png)


1. 释放完成后，拔掉U盘并重启电脑，继续完成Windows的安装。

## 3.3  安装后

1. 若之前电脑已安装正版激活的Windows 10/11，重装相同版本（家庭版、专业版等）的Windows时会自动激活。否则，请自行激活系统。请支持正版软件。
2. 如果deepin的启动项丢失，可以参考[这篇Linux Mint文档](https://linuxmint-installation-guide.readthedocs.io/zh_CN/latest/multiboot.html)（也适用于deepin和大多数常见的Linux），将deepin、[Ubuntu](https://cn.ubuntu.com/desktop)等系统的镜像拷贝至Ventoy U盘，重启到其试用版系统中，进行修复操作。也可以使用deepin live系统中系统修复工具的修复引导功能。（见附录 4.2）
3. 若deepin下访问Windows系统盘时文件无法修改，请在Windows的控制面板里关闭快速启动。



# 四、附录

## 4.1  另外2种Ventoy安装方式

### 4.1.1  使用Web UI模式(适用于1.0.48或者更高版本)

官方说明：https://ventoy.net/cn/doc_linux_webui.html

1.进入Ventoy官网（https://www.ventoy.net/cn/index.html），点击网页顶栏上的下载，在下载页面里下载末尾为linux.tar.gz的文件。（推荐通过“说明”中的蓝奏云下载地址下载）


![1](https://storage.deepin.org/thread/202108190919393816_%E6%88%AA%E5%9B%BE_Navigator_20210819090650.png)

2.解压下载的tar.gz文件，从文件管理器打开至解压后文件的目录下，右击空白处，点击“在终端中打开”。

![img](https://storage.deepin.org/thread/202108190921059317_%E6%88%AA%E5%9B%BE_dde-file-manager_20210819090958.png)

3. 在终端中输入sudo sh ./VentoyWeb.sh并回车执行，输入密码。在浏览器中输入网址http://127.0.0.1:24680并进入，此时会看到Ventoy的图形界面。

![s1.png](/图片存储/s1.png)

![s1.png](/图片存储/s2.png)

4.准备一个大小为4G或以上的U盘，插入电脑，复制U盘中的所有文件到电脑上。点击“安装”，将Ventoy安装到U盘上。（U盘将会被格式化，请务必确保U盘中的文件已做好备份）安装完成后，切换到终端，按Ctrl+C退出。

若上述方法使用时出现问题，请尝试下面的命令行模式。



### 4.1.2  使用命令行模式

1.进入Ventoy官网（https://www.ventoy.net/cn/index.html），点击网页顶栏上的下载，在下载页面里下载末尾为linux.tar.gz的文件。（推荐通过“说明”中的蓝奏云下载地址下载）

![s7.png](/图片存储/s7.png)

2.解压下载的tar.gz文件，从文件管理器打开至解压后文件的目录下，右击文件管理器窗口的空白处，点击“在终端中打开”，打开终端。

![img](https://storage.deepin.org/thread/202108190921059317_%E6%88%AA%E5%9B%BE_dde-file-manager_20210819090958.png)

3.点击文件管理器左边栏上的“计算机”，回到主界面。准备一个大小为4G或以上的U盘，插入电脑，拷贝走U盘中所有的文件。右击文件管理器中显示的U盘，点击属性，记住显示在下图中蓝色方框位置的标识。（我的是sdd1，标识因电脑上的硬盘数量和顺序等因素而异，请以自己的实际情况为准）

![s1.png](/图片存储/s3.png)


4.打开ventoy官网的使用说明（https://www.ventoy.net/cn/doc_start.html），滚动至Linux系统安装Ventoy部分。将显示为红色的那行命令(sh Ventoy2Disk.sh -i /dev/XXX)复制到先前打开的终端，在这行命令前面加上“sudo ”（注意空格，如果不方便加的话可以先输入“sudo ”再粘贴命令），将末尾的“XXX”换成第3步在文件管理器看到的U盘标识（略去数字，只输入前3个字母）。

![s1.png](/图片存储/s4.png)

5.回车执行命令。先输入系统用户的密码给予root权限，然后务必确保U盘中没有重要文件，两次输入y继续操作，之后U盘将会被格式化，开始制作。

![s1.png](/图片存储/s5.png)

![s1.png](/图片存储/s6.png)

6.当显示安装成功完成后，关闭终端，回到文件管理器，此时你会看见U盘的名称被修改为“Ventoy”（你也可以通过磁盘管理器更改其名称）。一个Ventoy U盘就制作好了，你可以将系统的iso镜像放在U盘中做启动盘（支持多个iso文件），也可以与此同时用U盘存储其他文件，不会影响启动盘的作用。
4.2  deepin安装镜像进入Live模式
从deepin安装镜像启动，当任意一个“Install Deepin”高亮时，按 e（uefi启动）或者Tab键（ Legacy启动），在显示的代码中用方向键移动光标，删去“livecd-installer ”（也有人认为是删去“-installer ”）（包括installer后的一个空格）（uefi启动模式下还要在“locales=zh_CN”后面添加“.UTF-8”） ，按F10（uefi启动）或者回车（Legacy启动），等待一会，可进入deepin的Live模式。但是，该模式下系统密码未知，因此可能无法进行修复引导的操作。（参考资料：https://bbs.deepin.org/zh/post/223203）
另外，gfdgd xi 自制了deepin live cd，比官方的live系统要新，也自带了系统修复工具。前往获取：https://bbs.deepin.org/zh/post/236521
写作不易，请尊重他人的劳动成果。
由于作者能力有限，这篇文章可能还存在需要改进之处，望大家指出。


