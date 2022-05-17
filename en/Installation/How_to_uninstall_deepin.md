---
title: How_to_uninstall_deepin
description: 
published: true
date: 2022-05-17T05:02:00.660Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:55:52.552Z
---

This page provides a quick overview of how to uninstall deepin. If you have any suggestions for us, please go to [Deepin Forum](https://bbs.deepin.org/forum.php?mod=forumdisplay&fid=70) for feedback.

## Uninstall deepin on Windows

If you use native installation, you can uninstall deepin in the following ways.

* If you are running the UEFI + GPT environment, please run [EasyUEFI](http://www.easyuefi.com/index-cn.html) under Windows, remove deepin startup items, enter Computer -> Management Tools -> Disk Management, and delete the Linux system partition.

* If you are in the BIOS + MBR environment, please run [NTBootAutofix](http://pan.baidu.com/s/1c0T9tOO) under Windows, after repairing Windows system startup items, go to Computer -> Management Tools -> Disk Management, and delete the Linux system partition.

## Uninstall Linux on deepin

If you use native installation, you can uninstall deepin in the following ways.

1. Enter deepin, open the terminal on the desktop, and execute the following command to reset MBR.

 `sudo dd if = / usr / lib / SYSLINUX / mbr.bin of = / dev / sda #SYSLINUX must be in uppercase`

2. If prompted to have "no syslinux / mbr.bin file or folder", execute the following command.

 `sudo apt-get install syslinux`

3. Restart the computer and enter Windows system.

4. Enter Computer -> Management Tools -> Disk Management, delete the Linux system partition to uninstall deepin.

## uninstall the deepin under Wubi

If you use Experience Install, you can uninstall deepin in the following ways.

1. Enter Windows system.

2. Open "Start -> Control Panel -> Add or Remove Programs".

3. Find deepin, select Change/Delete to uninstall deepin.
