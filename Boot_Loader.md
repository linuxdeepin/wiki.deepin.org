---
title: Boot_Loader
description: 
published: true
date: 2022-04-21T03:30:01.138Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:29:59.117Z
---

## Debian default boot loaders

| Architecture  | Default Bootloader| Alternatives Bootloader | DebianInstaller Bootloader |
|:------:||:------:||:------:||:------:|
| i386, amd64 (BIOS)| GRUB 2 | lilo, syslinux, grub v1 | syslinux, pxelinux |
| i386, amd64 (UEFI) | GRUB-EFI 2 | lilo, syslinux, grub-efi-v2, kernel EFI stub | |
| powerpc (NewWorld) | yaboot | grub-ieee1275 | |
| powerpc (OldWorld) | quik | quik, yaboot | |
| sparc | silo | | |
| hppa | palo | | |

## Available Bootloaders

### i386 + amd64

#### GRUB : GRand Unified Bootloader

Grub "version 1" (upstream calls it grub-legacy) is a GPLed bootloader intended to unify bootloading across x86 operating systems. In addition to loading the Linux kernel, it implements the Multiboot standard, which allows for flexible loading of multiple boot images (needed for modular kernels such as the GNU Hurd).(package:grub-legacy, previously grub up-to DebianLenny) 

PROs : very stable, very powerful.

CONs : Don't support GUID_Partition_Table (can run into problem with disk > 2TB). Can't handle LVM and LUKS-encrypted /boot partition.

#### GRUB2 (PC) : GRand Unified Bootloader, "version 2" (PC/BIOS version)

GRUB 2 (also known as PUPA) is the Preliminary Universal Programming Architecture for GRUB. It is a research project for the next generation of GNU GRUB. The most important goal is to make GNU GRUB cleaner, safer, more robust, more powerful, and more portable. 

This package contains a version of GRUB that has been built for use with traditional PC/BIOS architecture.(package:grub-pc) 

PROs : Support GUID_Partition_Table (Hence disk > 2TB). Understands LVM2

CONs :

#### GRUB2 (EFI) : GRand Unified Bootloader, "version 2" (EFI version)

GRUB 2 (also known as PUPA) is the Preliminary Universal Programming Architecture for GRUB. It is a research project for the next generation of GNU GRUB. The most important goal is to make GNU GRUB cleaner, safer, more robust, more powerful, and more portable.

This package contains a version of GRUB that has been built for use with EFI architecture, such as the one provided by Intel Macs (that is, unless they are in MBR emulation mode).(package:grub-efi) 


#### LILO : LInux LOader - The Classic OS loader can load Linux and others

This Package contains LILO (the installer) and boot-record-images to install Linux, OS/2, DOS and generic Boot Sectors of other OSes. You can use LILO to manage your Master Boot Record (with a simple text screen, text menu or colorful splash graphics) or call LILO from other Boot-Loaders to jump-start the Linux kernel.(package:lilo) 

PROs : Very stable, can work with LVM2, provided the kernel and initrd are not spread across non contiguous logical volume extents

CONs : has to be "reinstalled" every time you touch the kernel or initrd, or in LVM2 case, move extents containing the kernel or initrd.

#### ELILO : Bootloader for systems using EFI-based firmware
This is the Linux bootloader for systems using the Intel EFI firmware specification. This includes all ia64 systems, and some ia32 systems.(package:elilo) 

PROs : 

CONs : Doesn't support Hurd/Mach.

#### EFI boot stub : the linux kernel can be loaded directly from UEFI

By using the EFI Boot Stub it's possible to boot a Linux kernel without the use of a conventional EFI boot loader, such as grub or elilo.

PROs :* speed up the boot process.* can theoretically improve the kernel's ability to initialize the computer's hardware in a sane way. Whether this translates into improvements in practice is another matter.

CONs :* not flexible.* not really usable with multiple kernels.* you should keep a traditional bootloader, just in case...

### SPARC

#### SILO : Sparc Improved LOader

Like LILO or MILO, but for SPARC. This is the program you need to use if you plan to boot SPARC/Linux via a hard drive, floppy or CDROM. It installs to the boot block of your system and will allow for booting of Linux, Solaris, and SunOS.(package:silo) 

### parisc/hppa

#### PALO : Linux boot loader for parisc/hppa

This package contains the parisc boot loader itself, plus palo which is the boot media management tool as lilo is for i386.(package:palo) 

### PowerPC

#### YaBoot : Yet Another Bootloader

This package contains YaBoot, the bootloader of choice on the NewWorld line of Power Macintosh computers and on IBM CHRP computers. It supports loading a kernel from several different filesystems, including ext2, ext3, xfs, and reiserfs, as well as separate ramdisks and dual-booting. 

This package includes the installer tools ybin and mkofboot, and a yabootconfig program for generating a simple /etc/yaboot.conf file. (package:yaboot) 

#### QUIK : Bootloader for PowerMac or CHRP systems

QUIK provides the functionality necessary for booting a Linux Debian/PowerPC PowerMac or CHRP system from disk. It includes first and second stage disk bootstrap and a program for installing the first stage bootstrap on the root disk. 

Note that if you are running on a NewWorld machine, quik will be of limited use to you. You should install yaboot instead. It may even come handy on an OldWorld machine, since it provides the ofpath program and some documentation. (package:quik) 

## Other Loaders (not MBR Bootloaders)

### LoadLin - LInux LOader - The Classic OS loader can load Linux and others

This Package contains lilo (the installer) and boot-record-images to install Linux, OS/2, DOS and generic Boot Sectors of other OSes. You can use LILO to manage your Master Boot Record (with a simple text screen, text menu or colorful splash graphics) or call LILO from other Boot-Loaders to jump-start the Linux kernel. (package:loadlin) 

### win32-loader

This package provides GRUB boot record images that can be used by the native bootloaders of various win32 platforms in order to load a full instance of GNU GRUB. 

It is used by the win32-loader package to provide suitable GRUB images for loading Debian-Installer on win32. (package:win32-loader) 

### grubutil-win32

Debian-Installer loader for win32. This package provides a win32 program that can be used as a loader for Debian Installer, acting as a more user-friendly boot mechanism than traditional BIOS-based boot. 

It can act as a standalone netboot loader (for a practical example, see goodbye-microsoft.com), or as a cdrom/usb-disk add-on that starts a media-based install. (package:grubutil-win32) 

### GRUB4DOS and WINGRUB

GRUB4DOS is an universal boot loader based on GNU GRUB. It can boot off DOS/LINUX, or via Windows boot manager/syslinux/lilo, or from MBR/CD. It also has builtin BIOS disk emulation, ATAPI CDROM driver, etc.(package: none ; WWW) 

PROs : grubldr can be added to Windows boot.ini

CONs : Not supported by Debian.

### SysLinux : Bootloader for Linux/i386 using MS-DOS floppies

SYSLINUX is a boot loader for the Linux/i386 operating system which operates off an MS-DOS/Windows FAT filesystem. It is intended to simplify first-time installation of Linux, and for creation of rescue and other special-purpose boot disks.(package:syslinux) 

PROs : also a PXE and CD Bootloader 
CONs :

## Related Tools

[ms-sys](https://packages.debian.org/ms-sys) Write a Microsoft compatible boot record.

[gpart](https://packages.debian.org/gpart) Guess PC disk partition table, find lost partitions.