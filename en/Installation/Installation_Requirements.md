---
title: Installation_Requirements
description: 
published: true
date: 2022-05-25T12:58:46.545Z
tags: 
editor: markdown
dateCreated: 2022-05-13T07:49:06.118Z
---

# Obtaining the Installation Source
## 1. Download Official Image
&emsp;Visit the Deepin Community Website Download Page (https://www.deepin.org/en/download)to download the latest version of the image file for deepin (so that you can experience the latest features).

> Note:Due to human and resource limit, 32-bit version has not been available since deepin 15.4. If you want a bulk purchase or a customized 32-bit version, please contact support@deepin.com for paid commercial support.

## 2. ISO image integrity check
&emsp;After downloaded deepin image, you need to verify it. An unofficial or incomplete image should not be used to install deepin:  
- Windows system: Download Hash software, verify that the MD5/SHA256 you downloaded is consistent with the Download Page provided. 
- Linux system: At the corresponding path of image file, open Deepin Terminal and execute md5sum/sha256sum deepin-desktop-community-20.5-amd64.iso and confirm that the MD5/SHA256 is consistent with the Download Page provided.

```bash
 md5sum deepin-desktop-community-20.5-amd64.iso
```
```bash
 sha256sum deepin-desktop-community-20.5-amd64.iso
```

> Note: deepin-desktop-community-20.5-amd64.iso is the filename of the downloaded system image, you can use Tab key to type the filename automatically.

## 3. Make the bootable disk

Besides, you need to prepare a USB flash drive, or a disc and CD/DVD drive.
The installation media can be burned to a CD, or now more commonly, “burned” to a USB drive
- Visit the Deepin Community Deepin Boot Maker page (https://www.deepin.org/en/original/deepin-boot-maker) to download it.
- Create a bootable disk using the Deepin Boot Maker developed by the Deepin Technology team. 
- Plug the USB disk into the computer, run the Deepin Boot Maker. 
- Select deepin ISO image file to start making the bootable disk, and do not remove the USB disk during the process, When completed, please restart the computer.

> Note:
1.Please backup important data of the USB disk in advance, the Boot Maker will clear all the data on the disk.
2.You are recommended to format the USB disk as FAT32 format to help the computer recognize the disk.
3.Some of the USB disk is actually a mobile hard disk, if it cannot be identified, please choose another USB disk.
4.The capacity of USB disk should not be less than 4G, or it can not create a bootable disk successfully.
5.Don't touch the USB disk during the production process, so as to avoid writing failure due to incomplete production.

# Check Hardware Requirement
## Minimum Hardware Specifications

Please ensure that your computer meets the following requirements, otherwise you may not experience deepin perfectly:

- Architecture: x86_64 (AMD or Intel)
- CPU: Intel Pentium IV 2GHz or higher
- Memory: more than 4G RAM,8G or higher is recommended
- Disk: more than 64 GB free disk space,120 GB or higher is recommended 

Table of Minimum Hardware Specifications

| **Component** |                 **Minimum Hardware Specifications**              |
| :------------ | :-------------------------------------------------------------- |
| Architecture  | x86_64                                                      |
| CPU           | ≥ Intel Pentium IV 2GHz                                     |
| Memory        | ≥ 4 GB (8 GB or higher recommended for better user experience)   |
| Hard disk     | ≥ 64 GB (120 GB or higher recommended for better user experience)|

## Hardware compatibility

# Change Motherboard Settings

&emsp;Usually, the computer boots from hard disk, so before system installation, you should enter BIOS to set CD/USB flash drive as the first boot entry.
&emsp;For desktop computer, you can press <kbd>Delete</kbd> key, but for laptop, you can press the shortcut key to enter boot menu.

For example, the shortcut keys might be: 
| Laptop | Key |
|---|---|
| HP | F10 | 
| Dell | F12 |
| Acer | F12 |
| ASUS | F12 |
| Lenovo | F12 |
| Apple | Command |
| Huawei | F12 |
| Xiaomi | F12 |

> Note: If your computer's motherboard is in UEFI mode, please turn off Secure Boot in the motherboard settings, then restart the computer and press the key in the BIOS/UEFI interface which indicates "change boot order". 

# Planning Disk Partitions
# Install Mode
## install on physical host
## install on virtual machine
