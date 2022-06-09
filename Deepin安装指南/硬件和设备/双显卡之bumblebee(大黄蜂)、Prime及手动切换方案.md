---
title: 双显卡之bumblebee(大黄蜂)、Prime及手动切换方案
description: 
published: true
date: 2022-06-09T01:20:27.582Z
tags: 
editor: markdown
dateCreated: 2022-05-26T03:24:05.973Z
---

## 简介
本文由论坛用户black-hole分享
时间：2021年1月17日
原帖地址：https://bbs.deepin.org/zh/post/210053

## 方案一：大黄蜂

1. 直接使用深度显卡驱动管理器里面的大黄蜂方案，如果不成功可以尝试手动实现下面的两种大黄蜂方案，不过成功几率不高，不想尝试的可直接跳到Prime方案

2. 两个方案分别为：开源驱动Bumblebee、系统集成闭源驱动Bumblebee（其实只有一行代码不同）

> #安装独立显卡驱动
> ctrl+Alt+F2进入tty2模式，然后登录。
> #关闭登录管理器服务，停止lightdm服
> sudo systemctl stop lightdm
> #不管有无驱动建议先卸载掉旧版驱动
> sudo apt-get remove --purge nvidia*
> #安装console-setup
> sudo apt-get install console-setup
> #安装nouveau驱动，primus是可选项用于提升性能，nvidia-settings用于图形化界面的设置。
> sudo apt-get install bumblebee primus nvidia-settings
> #如果使用闭源nvidia驱动则使用下面的代码，记得把前的#号去掉
> #sudo apt-get install bumblebee-nvidia nvidia-driver nvidia-settings
> #验证驱动是否安装成功
> sudo apt-get install mesa-utils
> #注：安装mesa-utils这个包，用来显示系统的glx相关信息。
> optirun glxinfo|grep NVIDIA
> #查看对应bumblebee版本
> bumblebeed --version
> #当您看到的bumblebeed版本是3.2.1的时候，恭喜您，你的电脑是自带电源管理功能开箱即用，不需要进行任何设置。
> #在安装nvidia-settings后，我们可能通过以下命令设置独显或查看独显温度。
> optirun nvidia-settings -c :8

因为我没有成功且本方案不是本文重点，所以其他详细内容如成功后如何调用独显等可以查看参考文献 1。

## 方案二：Prime

> 使用此方案的前提(来自参考文献)：①专有NVIDIA驱动版本：>=435.17；②Xorg-server版本：>=1.20.6-1(查看版的命令 sudo X -version，我的版本是1.20.4，虽然低于要求但可以使用此方案)，想要升级版本的请参考文献 3，不过我不建议升级，因为几乎不能升级成功，就算成功了系统稳定性会是极大的问题。
{.is-warning}


1. 在安装系统的时候勾选安装NVIDIA闭源驱动（deepin20版本会有此选项，15版本是没有的）

2. 安装集显驱动：安装好系统后下载深度显卡驱动管理器，一般情况显示勾选的是第一项(使用Inter默认驱动)，说明已经安装了Inter驱动，如果默认是第二项开源驱动，则则需要手动切换为第一项然后重启系统，如果无法切换至第一项则需手动安装集显，详细安装步骤请移步参考文献 2.

3. 下载官方NVIDIA闭源驱动，如果是笔记本在产品系列里面选有(Notebooks)的，下载后是后缀为run的文件，我的是：NVIDIA-Linux-x86_64-460.32.03.run

![双显卡5.png](/图片存储/双显卡5.png)

4. 安装独显驱动：

①禁用开源nouveau驱动
> 
> sudo vim /etc/modprobe.d/blacklist-bcm43.conf
> #在文件末尾加上这两行
> blacklist nouveau
> options nouveau modeset=0
> #再执行以下两条命令，从内核彻底禁用nouveau驱动并重启
> sudo update-initramfs -u
> sudo init 6
> #重启后，执行以下命令，如果没有输出，说明nouveau已经被禁用
> lsmod | grep -i nouveau
> 
②安装驱动

> #按下快捷键“Ctrl+Alt+F2”，进入tty2，然后登录系统，关闭登录管理器服务（简单理解就是关闭图形界面）
> sudo systemctl stop lightdm
> #卸载旧驱动（这样就将开源、闭源驱动都卸载了）
> sudo apt-get remove --purge nvidia*
> #赋予可执行权限
> chmod u+x NVIDIA-Linux-x86_64-460.32.03.run
> #安装驱动文件，安装过程装中两个选项的选yes，一个的选ok，三个的选over，注意在进度条走完后的两个选项，选no
> sudo ./NVIDIA-Linux-x86_64-460.32.03.run
> #配置启动脚本，新建一个display_setup.sh
> sudo vim /etc/lightdm/display_setup.sh
> #内容如下：
> #!/bin/sh
> xrandr --setprovideroutputsource modesetting NVIDIA-0
> xrandr --auto
> #然后赋予权限
> sudo chmod +x /etc/lightdm/display_setup.sh
> #然后在lightdm里配置启用这个脚本
> sudo vim /etc/lightdm/lightdm.conf
> #找到 display-setup-script这一行，去掉前面的注释，将display_setup.sh这个文件地址填进去
> display-setup-script=/etc/lightdm/display_setup.sh
> #重启系统
> shutdown -r now
> #重启之后使用以下命令检查是否安装成功，如果成功会有内容显示
> nvidia-smi

显示如下内容，说明独显安装成功
![双显卡6.png](/图片存储/双显卡6.png)

在启动器里面可以看到自动安装好的NVIDIA X server settings程序，如果输入命令后显示上面图片的内容但此程序无法打开说明独显处于待机状态；如果此程序可以打开说明独显已经进入工作状态
![双显卡7.png](/图片存储/双显卡7.png)
独显彻底关闭时，则显示为如下内容
![双显卡8.png](/图片存储/双显卡8.png)
5. 安装nvidia-prime

下载nvidia-prime，双击安装，然后使用如下命令进行配置，配置后需要重启生效

> #选择Intel显卡(此方案将彻底关闭N卡，为节能模式)
> sudo prime-select intel
> 选择NVIDIA显卡(此方案默认使用集显，且独显处于待机状态自动键入，为平衡模式)
> sudo prime-select nvidia
> 查看正在运行的方案
> prime-select query
> 
nvidia-prime程序不能使用的可以试一试不同的版本，在我这里0.8.15.2版本的就不可以用，0.8.15.1版本却是可以用的

## 方案三：手动切换

> 慎用此方案，因为次方案的配置文件在不同的电脑会出现不同的缺陷
{.is-warning}


前面的步骤与方案二的相同，在第 5 步，只进行显卡的配置，不安装nvidia-prime

#首先执行以下命令，查看显卡的BusID
> lspci | egrep -i 'VGA|3D'
> #输出如下，其中00:0.20是Intel集显，对应的BusID为0:2:0；01:00.0是Nvidia独显，对应的BusID为1:0:0
> 00:02.0 VGA compatible controller: Intel Corporation Device 9bc4 (rev 05)
> 01:00.0 VGA compatible controller: NVIDIA Corporation Device 1f15 (rev a1)
> #以下是配置文件，需要填写到/etc/X11/xorg.conf，注意修改自己的BusID

启用双显卡，集显为默认显卡配置如下：(此配置文件在我这里表现为集显与独显同时工作，会引发个别软件画面的撕裂及横纹)
> 
> Section "ServerLayout"
>     Identifier "layout"
>     Screen 0 "intel"
>     Screen 1 "nvidia"
> EndSection
> 
> Section "Device"
>     Identifier "intel"
>     Driver "intel"
>     BusID "0:2:0"
>     Option "AccelMethod" "SNA"
> EndSection
> 
> Section "Screen"
>     Identifier "intel"
>     Device "intel"
> EndSection
> 
> Section "Device"
>     Identifier "nvidia"
>     Driver "nvidia"
>     BusID "1:0:0"
>     Option "ConstrainCursor" "off"
> EndSection
> 
> Section "Screen"
>     Identifier "nvidia"
>     Device "nvidia"
>     Option "AllowEmptyInitialConfiguration" "on"
>     Option "IgnoreDisplayDevices" "CRT"
> EndSection
> 
启用双显卡另一种配置如下：(此配置文件在我这里会使独显待机集显工作，但却无法唤醒独显进行工作)

> Section "Device"
>   Identifier "iGPU"
>   Driver "modesetting"
>   BusID "PCI:0:2:0"
> EndSection
>  
> Section "Screen"
>   Identifier "iGPU"
>   Device "iGPU"
> EndSection
>  
> Section "Device"
>   Identifier "dGPU"
>   Driver "nvidia"
>   BusID  "PCI:1:0:0"
> EndSection

> 启用独显，屏蔽集显的配置如下：(此配置文件在我这里会屏蔽集显，但却无法唤醒独显进行工作，所以开机进入系统后会黑屏，此时需要按下快捷键“Ctrl+Alt+F2”，进入tty2，删除配置文件)
> 
> Section "Module"
>     Load "modesetting"
> EndSection
> 
> Section "Device"
>     Identifier "Card0"
>     Driver "nvidia"
>     BusID  "PCI:1:0:0"
> EndSection

启用集显，屏蔽独显的配置如下：(此配置文件在我这里会使独显彻底关闭集显工作，但个别软件画面会出现横纹或撕裂原因不明)

> Section "Module"
>     Load "modesetting"
> EndSection
> 
> Section "Device"
>     Identifier "Card0"
>     Driver "intel"
>     BusID "PCI:0:2:0"
> EndSection


📃 参考文献：

1. https://www.jianshu.com/p/2ec26406c473

2. https://www.jianshu.com/p/924c86a0859c

3. https://blog.csdn.net/qq_43325034/article/details/106440428

4. https://blog.blankshell.com/2020/07/08/deepin%E9%85%8D%E7%BD%AEintelnvidia%E5%8F%8C%E6%98%BE%E5%8D%A1/

5. https://www.jianshu.com/p/ff4847c138df

6. https://zhuanlan.zhihu.com/p/165158820