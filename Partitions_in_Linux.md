---
title: Partitions_in_Linux
description: 
published: true
date: 2022-04-21T03:39:47.203Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:39:45.330Z
---

Before Installing the system, we'd better know the partitions in Linux.

As we known, the partitions in a hard disk are usually divided into **primary partion** and **extension partion**. The total partitions of them should be less than **four**. 
  

Primary partion can not be divided again and can be used immediately. But extension partion can be used only after divided into more partitions, which are logical partion without limited quantity.


In linux, IDEdevices will be assigned to a file prefixed by hd and SCSI devices will be assigned to a file prefixed by sd.


For IDE hard disk, the drive identifier is hdx~. hd is the device type, namely IDE hard disk. x is the disk serial number and ~is the partition. The first four partitions marked from 1 to 4 are primary partion or extension partion, 5 is the logical partion.


eg. hda3 is the third primary partion or extension partion in the first IDE device.


sda2 is the second primary partion or extension partion in the first SCSI device.


In Linux, each IDE or SCSI device are assigned 1 to 16 serial number and many filesytem formats are supported, such as FAT32, FAT16, NTFS, HP-UX, various Linux Native and Linux Swap partition types.


Here are the daily used partitions:

/boot: It contains the OS kernel and required files. The space is between 50mb—100mb.

/usr: To store software, more space is better.

/home: It's the home directory.

/var/log: To store system log.

/tmp: To store temporary files.

/bin: To store standard system utility.

/dev: To store device files.

/opt: To store optional installation software.

/sbin: To store standard system management files.

Usually, we need one swap partition, one /boot partition, one /usr partition, one /home partition and one /var/log partition. But at least one swap partition and one / partition.