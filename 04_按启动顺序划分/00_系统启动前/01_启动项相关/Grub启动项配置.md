---
title: Grub启动项配置
description: 
published: true
date: 2022-10-17T09:15:05.134Z
tags: 
editor: markdown
dateCreated: 2022-06-24T02:15:47.019Z
---

# Grub启动项配置
## 1.简单了解一下什么是grub
 grub是一个引导管理程序，全称GRand Unified Bootloader。它可以在多个操作系统共存时选择引导哪个系统。它可以引导的操作系统包括Linux,FreeBSD,Solaris,NetBSD,BeOSi,OS/2,Windows95/98,Windows NT,Windows2000。它可以载入操作系统的内核和初始化操作系统（如Linux,FreeBSD），或者把引导权交给操作系统来完成引导。
## 2.grub有以下特点：

### 2.1支持大硬盘
### 2.2支持定制开机画面
   grub支持在引导开机的同时显示一个开机画面。对于玩家来说，这样可以制作自己的个性化开机画面；对于PC厂商，这样可以在开机时显示电脑的一些信息和厂商的标志等；目前我们oem定制工具已经实现此功能。
### 2.3菜单式选择
   grub使用一个菜单来选择不同的系统进行引导”menuentry”。你还可以自己配置各种参数，如延迟时间:timeout，默认操作系统等。grub是通过文件系统直接把核心读取到内存，因此只要操作系统核心的路径没有改变，grub就可以引导系统。 
   除此之外，grub还有许多非常强大的功能。例如支持多种外部设备，动态装载操作系统内核，甚至可以通过网络装载操作系统核心。grub支持多种文件系统，支持多种可执行文件格式，支持自动解压，可以引导不支持多重引导的操作系统等。
   grub的命令行有非常强大的功能，而且支持如bash或doskey一样的历史功能，你可以用上下键来寻找以前的命令。
   grub并不需要按照什么规则去硬盘中找系统，而是根据/boot/grub/grub.cfg中的启动项加载内核、启动系统，而这个配置文件则是在系统安装或者执行命令生成最新的配置文件。
## 3.制作grub启动盘

首先确定grub已经安装，然后进入grub的目录，键入：
```
#cd /boot/grub
```
放入一张软盘，然后敲入命令：
```
 #dd if=stage1 of=/dev/fd0 bs=512 count=1
 #dd if=stage2 of=/dev/fd0 bs=512 seek=1
```
 这样就可以做好一张启动盘了。 也可以用mkbootdisk命令 #mkbootdisk 2.2.16：2.2.16是指内核版本号
 uos操作系统在完成安装后，就附带安装了grub，并对grub的主题进行了定制和美化
## 4.grub配置

完成安装之后，GRUB 在每次启动的时候载入配置文件 /boot/grub/grub.cfg。
**注意：每当修改 /etc/default/grub 或者 /etc/grub.d/ 中的文件之后，都需要再次#生成主配置文件。也就是说如果需要永久修改某个启动项，就要修改grub.cfg文件或者会影响grub.cfg生成的/etc/default/grub以及/etc/grub.d/中的脚本文件了**。
###  4.1重新生成配置文件指令：
update-grub或grub-mkconfig
update-grub这个命令其实是对grub-mkconfig的一个包装，在非Debian系的发行版上是没有的。
grub-mkconfig会执行的动作主要是：
加载/etc/default/grub中的一些配置项。比如GRUB_CMDLINE_LINUX_DEFAULT配置项会控制Linux的boot param。
挨个执行/etc/grub.d/目录中的脚本，用来生成最终的grub.cfg文件。
我们平常看到update-grub命令执行时输出的哪些启动项，其实就是/etc/grub.d/03_os-prober这个脚本里面执行os-prober这个工具产生的。
### 4.2/etc/grub.d里面脚本功能解释：
```
00_header               #配置初始的显示项目，如默认选项，时间限制等，一般由/etc/default/grub导入，一般不需要配置
05_debian_theme         #配置引导画面，文字颜色等主题
10_linux                #定位当前操作系统使用中的root设备内核的位置,包含deepin 启动项和advanced里面的启动项
15_linux_bar            #救援模式的启动项
20_linux_xen            #虚拟机监视器的东西，(暂时不知有什么用
30_uefi-firmware        #“system setup” 的启动项
35_os-prober            #windows的启动项一般在这个里面
40_custom               #用来加入用户自定义的启动项，将会在执行update-grub时更新至grub.cfg中
41_custom               #判断custom.cfg此配置文件是否存在，如果存在就加载它
```
**注：前面的数字是对文件排列执行的顺序进行排序，可进行更改，比如你想把windows启动项调到第一个，就把35_os-prober前面那个数字改成5到10的数字，比如06、07、08、09.**
### 4.3更改grub的字体
如果你认为默认的unicode字体在1024x768或更高分辨率的屏幕上显得太小，或者你认为默认的字体不好看，想换换口味，那就要用到"grub-mkfont"工具。下面的示例展示了如何从一个ttc字体(文泉驿等宽微米黑)制作一个24px大小的pf2字体：

   grub-mkfont -i1 -n WenQuanYiMicroHeiMono24px -o WenQuanYiMicroHeiMono24px.pf2 -s24 -v wqy-microhei.ttc
   将制作好的字体文件(WenQuanYiMicroHeiMono24px.pf2)放到"$prefix/fonts"目录中，修改'grub.cfg'文件中的两行：
``` 
 set gfxterm_font=WenQuanYiMicroHeiMono24px
 loadfont WenQuanYiMicroHeiMono24px
```
**注意：你最好使用等宽中文字体(推荐使用文泉驿等宽正黑或者等宽微米黑)，否则可能会让GRUB2的字体间距过大，十分难看。**
### 4.4替换grub的背景图：
```
 替换 /boot/grub/themes/deepin/background.jpg ,重新生成cfg配置文件，否则重启后失效；
```
### 4.5去除默认的选项框：
``` 
修改  /boot/grub/themes/deepin/theme.txt
将    menu_pixmap_style = "menu_*.png" 
用 # 注释掉即可。重新生成cfg配置文件，否则重启后失效；
```
### 4.6修改grub等待时间
```  
sudo vi /boot/grub/grub.cfg
修改set timeout = 10           10为10秒，根据自己需要修改等待时间即可
这个方法为直接修改cfg文件，也可以在/etc/default/grub文件内放入键值：GRUB_TIMEOUT=xxxx
```
### 4.7修改grub启动项菜单名称
   grub.cfg文件里 顶格写的menuentry字段后，单引号内容即为启动项菜单显示栏内容，修改这个字段即可简单达到grub启动项自定义目的，修改完记得更新cfg文件