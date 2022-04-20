[[zh:原生安装]]
## Introduction

For details on how to install deepin, please refer to the [Official Installation Guide](https://www.deepin.org/en/installation/) and the [Deepin Installer](https://www.deepin.org/en/original/deepin-installer/) project.


## Download

### Official Image

Visit the Deepin Community [Download Page](https://www.deepin.org/en/download/) to download the latest  deepin ISO/Live System. 

> Note: In order to better allocate development resources, from deepin 15.4 version on a 32 bit ISO will no longer be provided. Per the deepin website, custom 32 bit images are available as a paid commercial support option (contact: tech@deepin.com). 


### MD5 check

After downloading the deepin image, you need to verify it. Unofficial or incomplete images should not be used for installing deepin:

* Windows: Download [Hash software](http://soft.hao123.com/soft/appid/25574.html), then use it to verify that the MD5 hash of the image you downloaded is consistent with the checksum from the [Download Page](https://www.deepin.org/en/download/).

* GNU/Linux: Navigate to the directory where you downloaded deepin, open Deepin Terminal and execute `md5sum deepin-15.3-amd64.iso`; then confirm the md5sum output is consistent with the checksum from the  [Download Page](https://www.deepin.org/en/download/).

> Note: deepin-15.9-amd64.iso is the filename of the current ISO


## Make the bootable disk

### Download the Boot Maker tool

Visit the Deepin Community [Deepin Boot Maker](https://www.deepin.org/en/original/deepin-boot-maker/) page to download the Deepin Boot Maker.


1. Create a bootable disk using the [Deepin Boot Maker](https://www.deepin.org/en/original/deepin-boot-maker/) developed by the Deepin Technology team.

2. Plug the USB disk into the computer, run the Deepin Boot Maker.

3. Select deepin ISO image file to start making the bootable disk, and do not remove the USB disk during the process, When completed, please restart the computer.


### Cautions

* Please backup important data of the USB disk in advance, the Boot Maker will clear all the data on the disk.
* You are recommended to format the USB disk as FAT32 format to help the computer recognize the disk.
* Some of the USB disk is actually a mobile hard disk, if it cannot be identified, please choose another USB disk.
* The capacity of USB disk should not be less than 2G, or it can not create a bootable disk successfully.
* Do not touch the USB disk during the production process, so as to avoid writing failure due to incomplete production.

## Installation process

If your computer's motherboard is in UEFI mode, please turn off Secure Boot in the motherboard settings, then restart the computer and press the key in the BIOS/UEFI interface which indicates "change boot order".


For example, the shortcut keys might be:


* Desktop: Delete key
* Notebook: F2 key
* HP notebooks: F10 key
* Lenovo notebooks: F12 key
* Apple computers: C key


For details on how to install deepin, see [How to install](https://www.deepin.org/en/installation/).


## When The installation is completed

According to the installer interface prompts, enter and select the corresponding information, the system will install automatically. During the process, the installer will use the slides to show the features of the current system until the installation process is completed. After completed, follow the prompts to restart the computer to enter deepin.

> Note: If the installation fails, it will show error info. You can scan QR code on mobile phones to send the error log to servers.

## Hardware compatibility
Check operability of hardware and register it in the Deepin database by [[Hardware Probe]].

## Common Problem
### Install deepin in a multiple hard disk environment

If the motherboard is using the old boot mode (Legacy BIOS), rather than UEFI mode to boot the system, it cannot find the system after installed deepin. Here is a example:

/dev/sda has installed Windows system, you can boot and use; and you have installed deepin on /dev/sdb. If you install the system, restart the computer, and boot directly into the windows system, That means you have encountered the problem above.

There are several solutions:
* In BIOS, modify the hard disk boot priority, boot /dev/sdb first (recommended)
* Reinstall the deepin system, in the partition page of the installation interface, switch to the "Advanced" mode, in the bottom left corner of the advanced partition page, you can modify the grub installation location to /dev/sda (the first hard disk)