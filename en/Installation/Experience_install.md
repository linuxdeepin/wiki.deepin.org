---
title: Experience_install
description: 
published: true
date: 2022-05-17T05:02:30.650Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:55:20.356Z
---

## Introduction

Please refer to the official website for details on how to install deepin. [Installation](https://www.deepin.org/en/installation/).

## Download

### Official Image

Visit the Deepin Community Website [Download Page](https://www.deepin.org/en/download/) to download the latest version of the image file for deepin (so that you can experience the latest features).

> Note: In order to focus more on the development of the system, deepin 15.4 version will no longer provide 32bit official iso image, for technical support, please send an email to support@deepin.org.

### MD5 check

After downloaded deepin image, you need to verify it. An unofficial or incomplete image should not be used to install deepin:

* Windows system: download [Hash software](http://soft.hao123.com/soft/appid/25574.html), verify that the MD5 you downloaded is consistent with the [Download Page](https://www.deepin.org/en/download/) provided.

* Linux system: At the corresponding path of image file, open Deepin Terminal and execute `md5sum deepin-15.3-amd64.iso` and confirm that the MD5 is consistent with the [Download Page](https://www.deepin.org/en/download/) provided.

> Note: deepin-15.3-amd64.iso is the filename of the downloaded system image, you can use Tab key to type the filename automatically.

## Installation Process

The supported Windows system versions are: Windows XP, Windows 7, Windows 8, Windows 10, if you're using Windows 8 or above, please turn off Fast Boot, or deepin can not be installed.

1. Copy the deepin system image to the Windows system root directory (for example, under the D drive).
2. Download [Deepin Boot Maker](http://cdimage.deepin.com/applications/deepin-boot-maker/windows/deepin-system-installer.exe) and copy to the same location of the deepin system image.

 > Note: you can use archive manager to open deepin system image, and extract the boot maker.

3. Double-click to run deepin-system-installer.exe, enter the username and password, adjust the system language and partition size, and click **Start**.
4. Wait for the installer, when it prompts that the installation is complete, please follow the instructions to restart the computer.

## When The installation is completed

According to the installer interface prompts, enter and select the corresponding information, and the system will install automatically. During the process, the installer will use the slides to show the features of the current system until the installation process is completed. After the installation process is completed, follow the prompts to restart the computer.

> Note: If the installation fails, it will show error info. You can scan QR code on mobile phones to send the error log to servers.

## Hardware compatibility

Check operability of hardware and register it in the Deepin database by [[Hardware Probe]].

## UEFI mode, and can not find Windows startup items after the installation

This problem occurs because Windows did not successfully write its startup entry to your motherboard, but instead it uses UEFI's fallback mode to boot (via EFI/Boot/bootx64.efi).

At this point, if you installed other systems, and it has successfully written the info to the motherboard chip, the motherboard will ignore the fallback mode. This can cause the result that the Windows startup item cannot be found.

You can manually add a boot entry for Windows in the terminal, here assuming that your EFI/ESP partition is located in /dev/sda2:

$ sudo efibootmgr -c -d /dev/sda -p 2 -L "Windows Boot Manager" -l \\EFI\\Microsoft\\Boot\\bootmgfw.efi

Where "-d /dev/sda" specifies the hard disk path; "-p 2" specifies the partition number is 2; '-L "Windows Boot Manager"' specifies the name of the boot entry; "-l \\EFI\Microsoft\\Boot\\bootmgfw.efi" specifies the path of efi firmware.
