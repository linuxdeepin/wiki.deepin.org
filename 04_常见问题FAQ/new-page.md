---
title: deepin引导
description: 
published: true
date: 2023-06-13T06:33:47.591Z
tags: 
editor: markdown
dateCreated: 2023-06-13T06:33:46.182Z
---

#  使用Deepin引导黑苹果（OC引导）
同时使用windows系统,苹果系统，deepin系统的引导福利来啦
- 分享论坛用户：wuhaigang的实践操作步骤

安装了三块硬盘，分别安装了windows,苹果，deepin系统的情况下，切换windows和Deepin直接使用deepin引导就行了，可是黑苹果就是引导不了，后来在B站发现一个方法，直接修改deepin引导就可以引导黑苹果了，记录一下。
### 以下是操作步骤
1.文件管理器打开 /boot，右键-以管理员身份打开
![2023-6-13_83845.png](/2023-6-13_83845.png)
2、打开grub文件夹，备份grub.cfg，然后用文本编辑软件打开grub.cfg，这里使用的Notepad--
![2023-6-13_83882.png](/2023-6-13_83882.png)
3.找到windows引导项并复制，如果没有可以参照下面的
![2023-6-13_52895.png](/2023-6-13_52895.png)
以下是文本正文

###BEGIN /etc/grub.d/30_os-prober###

menuentry 'Windows Boot Manager (在 /dev/nvme0n1p1)' --class windows --class os $menuentry_id_option 'osprober-efi-CA1C-CD52' {
insmod part_gpt
insmod fat
if [ x$feature_platform_search_hint = xy ]; then
search --no-floppy --fs-uuid --set=root CA1C-CD52
else
search --no-floppy --fs-uuid --set=root CA1C-CD52
fi
chainloader /efi/Microsoft/Boot/bootmgfw.efi
}

###END /etc/grub.d/30_os-prober###

把这部分复制，记得带上注释，粘贴到刚刚要复制的文本下方。然后开始把这个修改成黑苹果的引导。
4.修改引导参数

（1）、查找黑苹果的安装位置

打开终端输入：sudo fdisk -l

我的比较长，直接贴出我安装的硬盘。
![2023-6-13_80987.png](/2023-6-13_80987.png)
重点是sda1。我安装时单独占的盘，所以引导文件就放在了sda1也就是第一个EFI盘符上。

（2）、查找硬盘uuid（卷或文件系统唯一标识）

终端输入：ls -al /dev/disk/by-uuid
![2023-6-13_45358.png](/2023-6-13_45358.png)
用这个FE4-A8CE替换上面复制的文本中的CA1C-CD52，有3处，注意别把别处的替换了，不然启动不了相应的操作系统。替换后，我的文本为

###BEGIN /etc/grub.d/30_os-prober###

menuentry 'Windows Boot Manager (在 /dev/nvme0n1p1)' --class windows --class os $menuentry_id_option 'osprober-efi-FE4-A8CE' {
insmod part_gpt
insmod fat
if [ x$feature_platform_search_hint = xy ]; then
search --no-floppy --fs-uuid --set=root FE4-A8CE
else
search --no-floppy --fs-uuid --set=root FE4-A8CE
fi
chainloader /efi/Microsoft/Boot/bootmgfw.efi
}

###END /etc/grub.d/30_os-prober###

（3）、修改黑苹果启动文件名及位置

###BEGIN /etc/grub.d/30_os-prober###

menuentry 'Windows Boot Manager (在 /dev/nvme0n1p1)' --class windows --class os $menuentry_id_option 'osprober-efi-FE4-A8CE' {
insmod part_gpt
insmod fat
if [ x$feature_platform_search_hint = xy ]; then
search --no-floppy --fs-uuid --set=root FE4-A8CE
else
search --no-floppy --fs-uuid --set=root FE4-A8CE
fi
chainloader /EFI/OC/OpenCore.efi
}

###END /etc/grub.d/30_os-prober###

如果是CLOVER好像是EFI/CLOVER/CLOVERX64.efi,这个我不确定，请自己确认文件名。

（4）、修改名称参数

Windows Boot Manager (在 /dev/nvme0n1p1)替换为macOS，或者其他你喜欢的名字。替换后我的是这样的

###BEGIN /etc/grub.d/30_macOS###

menuentry 'macOS' --class windows --class os $menuentry_id_option 'osprober-efi-FE4-A8CE' {
insmod part_gpt
insmod fat
if [ x$feature_platform_search_hint = xy ]; then
search --no-floppy --fs-uuid --set=root FE4-A8CE
else
search --no-floppy --fs-uuid --set=root FE4-A8CE
fi
chainloader /EFI/OC/OpenCore.efi
}

###END /etc/grub.d/30_macOS###

保存退出。

启动设置-通用就可以看到自己的杰作啦！修改默认启动项可以在这里改。
大功告成啦
![2023-6-13_83853.png](/2023-6-13_83853.png)