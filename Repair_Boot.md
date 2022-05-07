---
title: Repair_Boot
description: 
published: true
date: 2022-05-07T02:19:29.549Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:41:14.296Z
---

## Introduction

If GRUB takes over MBR, the GRUB installation is divided into three parts:

* The first part (in general) is written on the MBR
* The second part is the reserved sector part after core.img is embedded in the MBR
* The third part is /boot/grub the following modules and documents (if /boot  is on a separate partition, then it will be directly written in the corresponding  /grub directory) inside.

> Note: This entry is for GRUB2.0 only.

## Show GRUB menu

In general, the GRUB menu is displayed directly.

Sometimes the user will set GRUB wait time to 0, if you want to display GRUB menu temporarily, please hold Shift key before the GRUB load, some motherboard may need to restart more than once to take effect.

If you can enter deepin, you can also go to the Control Center -> Startup menu to adjust the corresponding options.

## GRUB error

### deepin 15.3 failed to start (UEFI)

#### Error and Analysis

  ```
GRUB loading
Minimal BASH-like line editing is supported.For the first word ... (omitted)
Grub>
  ```

Unlike the error in the old tutorial, in this case the identifier is` grub>` instead of `grub rescure>`, so enter `normal` and press Enter key will not perform any operation, this is caused by `normal.mod` error, grub here discovers deepin's /boot partition, but loaded an error version of `normal.mod` and unable to boot the system. The reason for the error may be due to the compatibility problem between easybcd and grub, or it may be other operating system is installed before the system deleted but the old system is not cleaned up efi partition or even deepin 15.3 is installed directly on the old system.

#### Solution

Use liveUSB, liveCD, or another linux release on the device to open gparted to see the root directory mount point of the boot error deepin 15.3, such as `/dev/sda1`, for example;
Boot deepin, enter the grub command line (that is, the error interface), type `set` and then press Enter key, example:

`prefix=(hd2,gpt1)/boot/grub`

where (hd2, gpt1) represents the partition of the system.
type `linux (hd2,gpt1)/boot/vmlinuz`, and then press Tab key to complete the name, press Space key, and then type `root=/dev/sda1 foo bar` and Enter key. This step is to load the system kernel.

> Note: There is no space between (hd2, gpt1) and /boot.

type `initrd (hd2,gpt1)/boot/init`, and then press the Tab key to complete the name, then press Enter key.
type `boot` press Enter key, and you can boot into the system.
After entering the system, execute `sudo update-grub` in the terminal, then open the Startup menu option in the control center, wait for it to update automatically, deepin 15.3 can be booted normally after the repair work succeed. Cheers!

Thanks to: @mattd

### Old Method

  ```
GRUB loading
error: unknown filesystem
grub rescue
  ```

It has been found that the following operations can cause this problem:

* One wants to delete Linux, then delete/format the Linux partition directly in windows.
* One adjusted the disk, and use partition tools to merge/split/adjust/delete the partition, so that the number of disk partitions changed.
* One reinstalled the system, and installed linux to a new partition, the original partition has been formatted, but did not reinstall GRUB2.
* One use Linux backup tools or manufacturing tools, restore the main partitions to the old 8.X version, but the old version of GRUB is GRUB1, then the GRUB2 destroyed.


### Reinstall GRUB

1. Use deepin boot disk to boot the computer, enter the installation interface, press Ctrl+Alt+F1, execute the following command, wait a moment to enter Live CD mode.

```
sudo service lightdm stop
startx
```

2. After entering the Live CD system, open the terminal, mount the system partition which is needed to be repaired to /mnt, you can use the gparted or `sudo fdisk-l` command to view, for example, the system partition which is needed to be repaired is /dev/sda1.

   Execute the following command:

  `sudo mount /dev/sda1 /mnt` 

   if the /boot is separated (assuming /dev/sda2), execute the following command:

  `sudo mount /dev/sda2 /mnt/boot`

   in addition, to mount the Live CD system /dev directory at the same time, execute the following command:

   `sudo mount --bind /dev /mnt/dev`

   and then use the `chroot` command to set the Live CD / to the previous /, execute the following command:

   ```
   sudo mount --bind /proc /mnt/proc
   sudo mount --bind /sys /mnt/sys
   sudo chroot /mnt
   ```

   Install and update GRUB settings (motherboard use BIOS boot), execute the following command:
   ```
   grub-probe -t device /boot/grub
   sudo grub-install /dev/sda
   sudo grub-install --recheck /dev/sda
   sudo update-grub
   ```

   Install and update GRUB settings (motherboard use UEFI boot), execute the following command:

   ```
  #after starting the root shell, check that your EFI system partition (most likely /dev/sda1) is installed on /boot/efi
  mount /dev/sda1 /boot/efi
  #reinstall the grub-efi package
  sudo apt-get install --reinstall grub-efi
  #place the debian boot loader in /boot/efi and create an appropriate entry in the computer's NVRAM
  sudo grub-install /dev/sda
  #re-create a grub configuration file based on the current disk partitioning mode
  sudo update-grub
   ```

3. mount the efi partition to /boot/efi and install the grub-efi package.

   ```
   grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=Deepin
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   ```

4. Repair completed, restart the computer to take effect.

### Delete GRUB

Deleting GRUB may cause the computer failed to boot deepin, please do with caution. If you need to completely remove GRUB2 (uninstall deepin), please see **How_to_uninstall_deepin**.