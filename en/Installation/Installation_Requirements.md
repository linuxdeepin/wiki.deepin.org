---
title: Installation_Requirements
description: 
published: true
date: 2022-05-24T11:08:49.737Z
tags: 
editor: markdown
dateCreated: 2022-05-13T07:49:06.118Z
---

# Obtaining the Installation Source
System ISO images can be downloaded from the www.deepin.org download page. 
## 1. Download Official Image
Visit the Deepin Community Website Download Page (https://www.deepin.org/en/download)to download the latest version of the image file for deepin (so that you can experience the latest features).

> Note:
Sorry, due to human and resource limit, 32-bit version has not been available since deepin 15.4. If you want a bulk purchase or a customized 32-bit version, please contact support@deepin.com for paid commercial support.

## 2. ISO image integrity check
After downloaded deepin image, you need to verify it. An unofficial or incomplete image should not be used to install deepin:  
- Windows system: download Hash software, verify that the MD5 you downloaded is consistent with the Download Page provided. 
- Linux system: At the corresponding path of image file, open Deepin Terminal and execute md5sum deepin-15.3-amd64.iso and confirm that the MD5 is consistent with the Download Page provided.

> Note: deepin-15.3-amd64.iso is the filename of the downloaded system image, you can use Tab key to type the filename automatically.

## 3. Make the bootable disk

&emsp;&ensp;Besides, you need to prepare a USB flash drive, or a disc and CD/DVD drive,
&emsp;&ensp;The installation media can be burned to a CD, or now more commonly, “burned” to a USB drive
- Visit the Deepin Community Deepin Boot Maker page to download the Deepin Boot Maker.
- Create a bootable disk using the Deepin Boot Maker developed by the Deepin Technology team. 
- Plug the USB disk into the computer, run the Deepin Boot Maker. 
- Select deepin ISO image file to start making the bootable disk, and do not remove the USB disk during the process, When completed, please restart the computer.

### Cautions

- Please backup important data of the USB disk in advance, the Boot Maker will clear all the data on the disk.
- You are recommended to format the USB disk as FAT32 format to help the computer recognize the disk.
- Some of the USB disk is actually a mobile hard disk, if it cannot be identified, please choose another USB disk.
- The capacity of USB disk should not be less than 2G, or it can not create a bootable disk successfully.
- Do not touch the USB disk during the production process, so as to avoid writing failure due to incomplete production.

# Check Hardware Requirement
## Minimum Hardware Specifications

Please ensure that your computer meets the following requirements, otherwise you may not experience deepin perfectly:

- Architecture: x86_64 (AMD or Intel)
- CPU: Intel Pentium IV 2GHz or higher
- Memory: more than 4G RAM, 8G or higher is recommended
- Disk: more than 64 GB free disk space

Table of Minimum Hardware Specifications

| **Component** |                 **Minimum Hardware Specifications**              |
| :------------ | :-------------------------------------------------------------- |
| Architecture  |  x86_64                                                |
| CPU           | Two CPUs                                                         |
| Memory        | ≥ 4 GB (8 GB or higher recommended for better user experience)   |
| Hard disk     | ≥ 64 GB (120 GB or higher recommended for better user experience)|

### Hardware compatibility

# Change Motherboard Settings

If your computer's motherboard is in UEFI mode, please turn off Secure Boot in the motherboard settings, then restart the computer and press the key in the BIOS/UEFI interface which indicates "change boot order". 
For example, the shortcut keys might be: 
- Desktop: Delete key 
- Notebook: F2 key 
- HP notebooks: F10 key 
- Lenovo notebooks: F12 key 
- Apple computers: C key 

For details on how to install deepin, see How to install.

## When The installation is completed 
According to the installer interface prompts, enter and select the corresponding information, the system will install automatically. During the process, the installer will use the slides to show the features of the current system until the installation process is completed. After completed, follow the prompts to restart the computer to enter deepin. 
> Note: If the installation fails, it will show error info. You can scan QR code on mobile phones to send the error log to servers.

# Planning Disk Partitions
# Install Mode
## install on physical host
## install on virtual machine

# Installation Process

   Usually, the computer boots from hard disk, so before system installation, you should enter BIOS to set CD/USB flash drive as the first boot entry.
   For desktop computer, press Delete key, and for notebook, press F2, F10 or F12 key to enter BIOS settings.
   You can finish the installation process in just a cup of tea's time.

1. Put CD into the CD drive.
2. Boot and enter BIOS to set the CD as the first boot entry.
3. Enter the installation interface and choose the language you want to install.
4. Enter the account settings, input username and password.
5. Click Next.
6. Choose format, mountpoint and allocate disk space, etc.
7. Click Install.
8. In the popup confirm window, click OK.
9. The installation process will start automatically.