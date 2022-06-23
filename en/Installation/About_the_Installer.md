---
title: About_the_Installer
description: 
published: true
date: 2022-05-15T02:11:08.941Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:53:55.712Z
---

## Summary

Deepin Installer is an installer with user friendly interface to let users install deepin in a safe, stable and efficient environment and to troubleshoot more easily.

## Installation Guide

### Choose Language
Choose the language you need and click Next button.

### Set Account

Type username, computer name, password and retype password, then click Next button. You can also change avatar by clicking on the user avatar image.

### Choose Timezone

Click the timezone at the top left corner of the Set Account page, search and select the correct timezone and click return button to the Set Account page. The installer will locate the timezone by the network automatically, you can skip this step if the timezone settings are correct.

### Set Keyboard Layout

Choose the keyboard layout you need and click Next button.

### Select the installation location

Click the simple / advanced tab on the interface to switch between simple mode and advanced mode.

- When you select simple mode, select the drive letter you want to install the system and click Start to install.

- When you select advanced mode, you can create a new partition, edit the partition, select the boot installation location, and then return to the Select installation location interface, select the drive letter you want to install the system, and then click Start Installation.

### Create a new partition

After selecting a blank partition, click the "Edit" icon to the right of the blank area to enter the New Partition page.
Select the type of partition, location, file system, mount point, size, and then click Next.

### Edit the partition

After selecting a non-blank partition, click the "Edit" icon to the right of the blank area to enter the Edit Partition page.
Select the partition of the file system, mount point, determine whether to check the format partition, and then click Next.
When the partition is the root partition, boot partition, var partition, etc partition, usr partition, the format option is checked by default and is not allowed to cancel, to avoid the "uid changes or special files are not covered" problem caused by not formatting the system partition, and still do not work properly after reinstalled the system.

### Select the boot installation location

Select the boot installation location and click Back.

### Ready to install

Check the interface prompt, click OK to continue the installation.

## Frequently Asked Questions

### Virtual machine installation

Trying deepin under the virtual machine will affect the system performance and operating experience, in order to let you enjoy deepin, it is recommended that you install in a physical machine.

### Memory is less than 4G

If your system memory is less than 4G, you need to create a SWAP partition, the installer will automatically detect and automatically create SWAP partition.

### The disk space is too small

When installing in a virtual machine, allocate more than 12G of hard disk space for installing deepin, if the space is insufficient to install, you should adjust the virtual machine hard disk space according to the interface prompts.

### No EFI partition

To install deepin on GPT disk, you need to have an EFI partition, if you forget that, please create a new EFI partition according to the interface prompts.

### MBR disks need to be full formatted

When your motherboard boots as EFI, but the disk format is MBR, the installer cannot install the system directly. You can switch to USB boot installation, or do full format on the disk after the installation of GPT partition, please do a full disk backup before formatting the disk to avoid data lose.

### The minimum size of the root partition and boot partition

When you create a root partition, a boot partition, and an EFI partition, the disk size is at least 12G, 500MB, 300MB, or the installation fails.

## When the Installation failed

Generally you can install successfully, but if the installation fails, do not worry about that, and please take a photo or scan the QR code on the interface and send the error log to us, Deepin engineers will locate the problem as soon as possible to help you install the system.
