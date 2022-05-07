---
title: File_system
description: 
published: true
date: 2022-05-07T02:29:39.970Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:55:35.697Z
---

## Summary

File system is the organization of data that defines the way that data is saved, read and update. Different types of file system has different method for optimizing efficiency according to the storage device they are applied for. 

Linux kernel supports many types of file system, which can satisfy personal needs as well as administrators' requirements. Some file systems are designed for certian types of media, such as ISO9660 for CD and DVD disk; while others have a wider range of usage, such as ext4, or fat32.

## Local file system

There are generally two types of local file system:

- Non-journaled file system: traditional file system, no journal support is provided

- Journaling file system: file system that writes changes to journal (log) before writing changes to file (or directory) content. When machines encounter problems like kernel crash or power failure, journaling file system can easily keep its integrity and be restored to its earlier state.

### Non-jornaled file system

* Ext: Extented file system. It is the first file system designed for Linux, released in April 1992.It plays an important role in the developement of Linux, but is rarely used now for its defects in performance and compatibility.

* Ext2: Extend file system version 2. Compared to other file systems, ext2 is much more simple, making it faster when dealing with I/O operation. However, it does not support journaling, which means one needs to check file system thoroughly after a crash to find (and fix) possible errors.

* Minix: The first file system supported by Linux. It has a lot of limits of using, and does not perform well. The biggest problem for minix is that the largest partition it supports is 64 MB, making it virtually useless in recent years.

* Xia: The enhanced version of Minix. It solves the problem of size limit of file system and file name, whereas no new features is introduced. Rarely used this days.

* Msdos: Commonly used file system in DOS、Windows and some OS/2 operating system. The file name has the format of "8+3", i.e. 8 characters representing the main body and 3 characters representing file extension.

* umsdos: Enhanced file system of msdos in Linux. It supports features like long file name, file owner,  permission, link and device file. Allow using common msdos file system in Linux without creating seperated partition.

* FAT16: Also known as FAT, it uses 16-bit file allocation table to manage disk (floppy disk, hard disk or removable disk). It is commonly used by Windows 98 and Windows ME.

* FAT32: Another type of file system commonly used by Windows 98 and later version. It uses 32-bit file allocation table to provide support for lagrger partition.

* VFAT: Enhanced version of FAT16, added support of long file name. It provides solution to file sharing between different operating system like Windows, Linux and OS X. Note that in Linux kernel, both FAT16 vFAT and FAT32 are recognized as vFAT.

iso9660: Designed for optical media, such as CD and DVD. As it is mostly used for reading data, rather than writing, Linux kernel provides read-only support for ISO9660. To create a file system of this kind, you will need to use user-level utility like mkiso or growisfs.

### Journaling file system

* Ext3: Similar to ext2, with the purpose of replacing ext2. It adds support of journaling, while maintaining most of the data structures of ext2, so that a ext2 partition can be converted directly to ext3 partition without losing of data. Because of this, ext3 has been very popular, and receives lots of support of data recovery when hardware failure occurs. However, it does not perform so well in some cases, and needs frequent check that may annoy users during startup.

* Ext4: The latest version of ext file system. It is based on ext3, and adds more useful function for dealing with files. Although most Linux distribution support ext4, it should be noted that ext4 is still a new one. Some people may not trust its stability, though it has had indeed greet improvement in its performance.

* Reisefs: The one that receives the longest support by Linux kernel. It is a file system with fast reading /writing speed when dealing with files of small size. Unfortunately, reisefs crashes more often than ext3, and there are few tools for recovery when crashing occurs.

* XFS: Contributed to Linux kernel by SGI, it is one of the best system for dealing with large volumes and files. XFS use more RAM than other types of file system, but it is worthwhile if it shows efficiency in dealing with large files. XFS may not be suitable with desktop computer or laptop, but indeed do well on many servers. Like ext3, XFS is a complete journaling file system.

* Btrfs: Developped by Oracle since 2007 that support COW (copy-on-write) feature, aimed to replace ext3 file system in Linux. It provides solution to limits in ext3, especially in size of single file and whole file system, as well as file checksums, disk snapshots, snapshots of snapshots, RAID, subvolumes and online resizing.

* JFS: Contributed to Linux kernel by IBM, famous for its speed under extreme condition. It can be spanned over huge volume, which makes it applicable to NAS (Network Attached Storage) device. The long history as well as the though test of JFS mak it one of the most reliable file system.

* Hpfs: High Performance File System is the file system used by Microsoft in LAN Manager. It is also used by IBM LAN Server and OS/2. HPFS provides access to large disk drive and more features to improve the security.

* Proc: Usually seen as a fake file system, used as an interface to unify kernel data structure.

## Network file system

Linux supports various types of network file system (both client and server), make it possible to share data transparently among computers. There are two types of network file systems that are commonly used: NFS and SMB.

### NFS

Network File System (NFS) is a protocal of distributed file system, developped by Sun and released in 1984. It allows different machines and different operating systems to share data through network, make it possible for applications to access data stored in server. It is widely used by Unix system to share disk files.

The basic priciple of NFS is "allowing different clients and servers share same file systems through a set of RPC (Remote Procedure Call). Following service are provided by NFS:

- Searching files in directory
- List files in directory
- Managing directory
- Get file attributes
- File reading / writing

NFS was soon accepted after it had been released. Althrough there were many kinds of distributed file systems developped by universities and laboritories, NFS was indeed the first successful product both in academy and in business. Later Sun made it a standard for network sharing and publish its interface in 1989, making more manufacturers willing to include NFS in their product. The fourth version of NFS, released in 2000, received great influence from OpenAFS and SMB.

The major problem of NFS is that it is not suitable for large distributed system.

#### Mounting NFS

There is al little difference between mounting of local file system and mounting of NFS. Users need to specify the domain or IP address of target server, rather than a local device. Use a colon to seperate server name and directory. For example:

    mount -t nfs darkstar.example.com:/home /home

### SMB

Samba is used for connection between Unix-like systems and SMB/CIFS (Server Message Block/Common Internet File System) for Windows. It is a free software too.

The current version (v3) of SMB can not only access SMB folder and printer, but also be integrated into Windows Server Domain, acting as Domain Controller and joining into Active Directory. In bref, SMB serves as a bridge between UNIX-like OS and Windows for data sharing.

Users can directly link to samba shares. Unfortunately, the support for SMB is not so good as NFS, but it still provide connectivity to Windows hosts with well performance. Thus, SMB is generally deployed in local area network (LAN).

#### Mounting SMB

It is very easy to mount a SMB file system. Like SMB, you can use the same way to tell how to find the server and the resource you want. Beside, you need to provide a user name and a password.

     mount -t cifs //darkstar/home /home -o username=alan,password=secret

You may want to known the reason that the type of file system is "cifs" instead of "smbfs". The latter is firstly introduced in Linux kernel, yet replaced latter by cifs driver for its better performance and security.

All SMB shares need user  name and password to be accessed. This may cause problems when users want to add this entry to fstab. It is then recommanded to use "credentials" option, which tells mount to read from a file that contains the user name and the password. So long as this file is protected and accessible only to root user, the risk of identity leak can be maintained in a low level.

## Reference

[维基百科网络文件系统](http://zh.wikipedia.org/wiki/%E7%BD%91%E7%BB%9C%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F)

[通用线程：Samba 简介第一部分](http://www.ibm.com/developerworks/cn/linux/server/samba/samba-1/index.html)

[网络文件系统与 Linux](http://www.ibm.com/developerworks/cn/linux/l-network-filesystems/index.html)

[NFS 文件系统源代码剖析](http://www.ibm.com/developerworks/cn/linux/l-cn-nfs/index.html)

[文件:模板:File System](https://wiki.deepin.org/index.php?title=%E6%A8%A1%E6%9D%BF:File_System&action=edit&redlink=1)