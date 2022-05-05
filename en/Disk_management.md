---
title: Disk_management
description: 
published: true
date: 2022-04-21T03:55:15.807Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:55:13.056Z
---



## Summary

In Linux system, it is a important task for system administrator to well organize their disks. Here are some commonly used operations:

* Partitioning: Create an area of fixed (or variable) volume (partition) on free disk space to store data;

* Delete: Remove existing partition(s);

* Mount: Load a device or disk partition on existing directory for using.

## Disk Management

### Using graphical interface

Users can utilize software named Gparted to manage their disks.

### Using command


#### View disk information

Use `df` to display information about disk.

Example:

    df ## List disk usage of each file system
    df -ia ## List inode usage of each file system
    df -T ## List types of file system
    
Use `du` to estimate the usage of specified file system

Example:

    du -h --max-depth=1 ## Show the size of each subdirectory under current directory, as well as the total size of current directory
    du -h --max-depth=1 work/testing ## Show the size of directory"work/testing"
    du -h --max-depth=1 work/testing/* ## Show sizes of each file and subdirectory under directory "work/testing"
    du -ha ## Show sizes of each subdirectory and file recursively
    
#### Disk partitioning

Use `fdisk` for partitioning.

Example:

    fdisk -l  ## List disks and their partitions

#### Formatting

Use `mkfs` to format disk or partition

Example:

    mfks -t ext3 /dev/sda6 ## Format partition "/dev/sda6" as ext3
    mkfs -t ext2 /dev/sdb7 ## Format partition "/dev/sdb7" as ext2

#### Disk maintenance

Use `fsck` to check and fix possible errors on disk.

Example:

     fsck -t msdos -a /dev/hda5    ## Check msdos partition "/dev/sda5", and repaire it if it has error
     
Use `badblocks` to check bad sector on disk

Syntax:

    badblocks -[svw] Disk
    
Options:

    -s : Show progress on screen
    -v : Show more details during check
    -w : Also do writing test. Do not use this option when you have data on the disk!

#### Disk writing

Use `sync` to synchronize data between RAM and external storage device



    sync  # Write unsynchronized data in memory to hard disk

Use `dd` to copy or convert file

Example:

    sudo dd if=/home/cxbii/deepin.iso of=/dev/sdb bs=8M conv=fsync  # Write file "/home/cxbii/deepin.iso" to device "/dev/sdb"

#### Disk quota

Disk quota is used to limit the usage of disk space by users, so that the whole system does not malfunction when disk space runs out.

Types of disk quota:

* Soft limit: When disk usage goes beyond soft limit, system will generate a warning, but still allow using of disk.

* Hard limit: The actual boundary of disk usage. When hard limit is passed, system does not allow user to increase their data size anymore.

Disk quota can be turned on or off manually.


## Reference

[鸟哥私房菜:磁盘管理](http://vbird.dic.ksu.edu.tw/linux_basic/0230filesystem.php)

[鸟哥私房菜:磁盘配额](http://vbird.dic.ksu.edu.tw/linux_basic/0420quota.php)