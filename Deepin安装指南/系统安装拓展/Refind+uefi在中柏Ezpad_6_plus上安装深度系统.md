---
title: Refind+uefi在中柏Ezpad 6 plus上安装深度系统
description: 
published: true
date: 2022-05-17T11:38:12.748Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:41:05.095Z
---

## 中柏Ezpad 6 plus
中柏 Ezpad 6 plus 为中柏的一款二合一平板电脑，配置为：
CPU:  英特尔Apollo Lake N3450 1.1~2.2GHz
内存: 6G
SSD: 60G
eMMC: 58G
接口: DC电源接口×1、耳机接口×1、USB3.0×1、TF读卡器插口×1、Micro HDMI×1
尺寸: 11.6
屏幕: 电容屏 1920×1080
## 参考的帖子
1.[给寨板装Deepin](https://bbs.deepin.org/forum.php?mod=viewthread&tid=149744&extra=&page=1)  
2.[寨板折腾笔记.md](https://segmentfault.com/n/1330000012319645#articleHeader3)  
3.[Install Ubuntu on Celeron (Apollo Lake) N3450 (UEFI Windows 10) Computer](https://thanhsiang.org/faqing/node/221)  
4.[refind(arch)](https://wiki.archlinux.org/index.php/REFInd_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))  
## 需要的软件
1.deepin 系统镜像  
2.deepin 系统启动盘制作工具，可以在 deepin 系统镜像中解压出来  
3.refind 文件，这是 [refind](http://www.rodsbooks.com/refind/) 的网站，请到这里下载。Ezpad 6 plus 的 uefi 大概是阉割版，咨询过售后技术人员，该平板并不支持 legacy，且未锁 secure boot (事实上并无法找到 secure boot 的相关设置)，根据[寨板折腾笔记.md](https://segmentfault.com/n/1330000012319645#articleHeader3)的说法：“这个问题应该是板子uefi固件和grub2共同造成的，但是rEFInd似乎比grub2做的更好，可以正常使用rEFInd代替grub2实现对多操作系统的引导，并且会自动搜索现有硬盘般上存在哪些系统，然后将其显示出来，供用户选择
再都rEFInd引导程序的使用也是简单的不行了，因为省掉了用户自己写配置文件的过程”目前似乎只能通过 refind 来引导系统  
## 制作启动盘
1.使用深度启动盘制作工具制作深度系统安装盘  
2.在 refind 的官网下载到压缩文件，解压，在里面找到 refind 文件夹  
3.将 refind 目录下面的 refind_x64.efi 重命名为 BOOTX64.EFI  
4.复制 refind 目录下的所有文件到启动盘的 EFI/boot 目录下，根据[Install Ubuntu on Celeron (Apollo Lake) N3450 (UEFI Windows 10) Computer](https://thanhsiang.org/faqing/node/221)，需要将 refind.conf-sample 文件命名为 refind.conf  
5.编辑 refind.conf 文件，这里是在末尾添加参数说明  
```
menuentry "Try deepin" {
    loader /live/vmlinuz.efi
    options "file=/cdrom/pressed/deepin.seed boot=live quiet splash ---"
    initrd /live/initrd.lz
}
```  

'loader /live/vmlinuz.efi' 指定加载的内核引导文件路径  
'options "file=/cdrom/pressed/deepin.seed boot=live quiet splash ---"' 加载时使用的参数  
`initrd /live/initrd.lz`制定加载临时根文件系统 initrd 的文件路径  
6.开机并引导 U 盘启动，进入引导的 live CD 系统  
7.系统分区，注意，安装前需要将要安装的分区删除，留出空白空间，否则系统安装器不会识别。注意分区时需要一个 efi 分区 (uefi 需要有 efi 分区)，否则无法安装。我的分区方案是：efi, /, /home, swap  
8.安装系统  
9.重新启动系统，再次引导进入 live CD 系统  
10.挂载 efi 分区，此处为：  
```
sudo mount /dev/sda1 ~/Music
```  
11.使用 root 权限，cp 指令复制 U 盘的 /EFI/boot/ 下的所有文件到 efi 分区下的 boot 文件夹下，/EFI/boot/ 文件夹在 /lib/live/mount/medium/ 下：  
```
sudo cp -r /lib/live/mount/media/EFI/BOOT/* ~/Music/EFI/boot/
```   
删除 refind.conf，注意，如果这个文件不删除，refind 将按这个文件进行引导，根据 Arch 论坛的讨论帖 [rEFInd -- Error: Not Found while loading vmlinuz-linux](https://bbs.archlinux.org/viewtopic.php?id=182069), refind.conf 文件需要放置在 /boot 目录下，然而 refind.conf 只有在 /eft/boot/ 下才能起作用，而如果 /efi/boot/ 下没有 refind.conf，则将自动寻找并引导 vmlinuz-linux 文件，故删除 refind.conf 才能成功引导系统。系统启动后，efi 分区将自动挂载到 /boot/ 目录下。  
12.重启，进入 BIOS，将 UEFI OS 项设置为第一启动项  
13.启动系统，进入 deepin
## 安装 refind 引导的另一种方法
在安装系统完毕后，此时拔出U盘，重新启动将无法进入系统，因 grub2.02 与 BIOS 冲突，故无法寻找到正确引导（2.03的 grub 可以正确引导，使用了 grub 2.03 的发行版有 manjaro, ubuntu），所以需要安装 refind 替代 grub2.02。
安装方法之一是进入 U 盘 live 系统，然后将 refind 文件拷贝到 /efi 下，另一个方法更加简单，如下。  
1.插入 U 盘引导，因 U 盘已经修改为 refind 引导，所以将进入 refind 选择界面，找到 vmlinuz 开头的引导项，进入，即可进入安装在 SSD 上的系统。  
2.进入系统后，因网卡问题，无法联网，此时可以使用 USB 无线网卡 或 手机通过 USB 共享网络提供 ezpad 的联网。  
3.联网更新软件列表，安装 refind:  
```
$ sudo apt-get update
$ sudo apt-get install refind
```
4.将显卡驱动切换为 Intel 兼容模式，关机，拔 USB 无线网卡，重启，这样 WiFi 就可以使用。（如果不行就再次尝试）（WiFi问题个人估计是因为网卡与 Intel 显卡驱动冲突，目前没有能力解决。