---
title: Installation_Requirements
description: 
published: true
date: 2022-06-17T01:44:48.876Z
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

# Minimum Hardware Specifications

&emsp;Please ensure that your computer meets the following requirements, otherwise you may not experience deepin perfectly:

## Table of Minimum Hardware Specifications

| **Component** |                 **Minimum Hardware Specifications**             |
| :------------ | :-------------------------------------------------------------- |
| Architecture  | Amd64 (AMD or Intel)                                       |
| CPU           | ≥ Intel Pentium IV 2GHz or higher                               |
| Memory        | ≥ 4 GB (8 GB or higher recommended for better user experience)   |
| Hard disk     | ≥ 64 GB (120 GB or higher recommended for better user experience)|

# Motherboard Settings

## How to choose BIOS boot mode when booting deepin Setup ?
There are two style of boot mode in motherboard firmware :  
- UEFI Mode 
- Legacy BIOS Mode

In general, install deepin using the newer UEFI mode, as it includes more better features than the legacy BIOS mode.After deepin is installed, the device boots automatically using the same mode it was installed with.

## How to change the boot mode setting ?
 1. Boot the PC, and press the motherboard manufacturer's key to open the BIOS menus.
 
> Note: Common keys used: Esc, Delete, F1, F2, F10, F11, or F12. During startup, there's often a screen that mentions the key. If there's not one, or if the screen goes by too fast to see it, check your manufacturer's site.

2. Select legacy only/UEFI only in boot section in BIOS menus, or select both in boot option and set legacy first/UEFI first in boot priority option

## How to make sure  you're booted into the right boot mode every time you start your PC?

There are two style of partition style in hard disk:
- GPT partition style 
- MBR partition style

If you want to ensure that your drive boots into a certain mode, use disk that you've preformatted with the GPT partition style for UEFI mode, or the MBR partition style for BIOS mode.

- GPT partition style + UEFI boot mode
- MBR partition style + legacy BIOS mode

> Note: When the installation starts, if the PC is booted to the wrong mode, deepin installation will fail. To fix this, restart the PC in the correct boot mode.

#### Summary of BIOS settings

| Option | Value |
| --- | --- |
| Secure Boot | Disabled |
| OS Optimized |Others or Disabled
| CSM(Compatibility Support Module) Support | Yes or Enabled |
| Boot | Legacy only or UEFI only or Both
| Boot Priority (Boot=both) | Legacy First or UEFI First |

## Boot Device Priority
&emsp;Usually, the computer boots from hard disk, so before system installation, you should enter BIOS to set CD/USB flash drive as the first boot entry.
&emsp;For desktop computer, you can press <kbd>Delete</kbd> key, but for laptop, you can press the shortcut key to enter boot menu.

#### The shortcut keys of laptop might be: 
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

# Disk Partitions and File System
## Manual operation 

| **Mount point** |	**Partition Name** | **File system** | **Size** |
| --- | --- | --- | :---: | 
| / | root (required)	| EXT4(recommend)	| more than 15G |
| /home | home (recommend) |	EXT4(recommend) |	more than 15G |
| swap | swap(optional) |	swap | less than 4G memory should get 4G size, memory more than 4G can ignore |

## Auto operation 

| **Mount point** |	**Partition Name**	| **File system**	| **Size** | **Label** |
| --- | --- | --- | :---: | --- |
| /boot| boot | EXT4 | 1G | Boot |
| / | root | EXT4	| 15G | Roota |
| None | None | EXT4 | 15G | Rootb |
| /data | None |	EXT4 | 15G | _dde_data |
| /recovery | None |	EXT4 | 15G | Backup |
| swap | swap | swap | 1.5 * memory size | SWAP |

> **For examlpe :** Disk Size : 64GB , Memory Szie : 2GB , Swap Size : 3GB (1.5*2GB)


