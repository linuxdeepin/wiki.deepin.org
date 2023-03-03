---
title: Linux的文件系统及文件缓存知识点整理
description: 
published: true
date: 2023-03-03T02:35:02.721Z
tags: 文件系统 缓存
editor: markdown
dateCreated: 2023-03-03T02:24:38.912Z
---

# Linux的文件系统及文件缓存知识点整理
## 文件系统的特点

文件系统要有严格的组织形式，使得文件能够以块为单位进行存储。
文件系统中也要有索引区，用来方便查找一个文件分成的多个块都存放在了什么位置。
如果文件系统中有的文件是热点文件，近期经常被读取和写入，文件系统应该有缓存层。
文件应该用文件夹的形式组织起来，方便管理和查询。
Linux内核要在自己的内存里面维护一套数据结构，来保存哪些文件被哪些进程打开和使用。
总体来说，文件系统的主要功能梳理如下:

![2023-3-3_95798.png](/2023-3-3_95798.png)

## inode与块的存储
硬盘分成相同大小的单元，我们称为块（Block）。一块的大小是扇区大小的整数倍，默认是4K。在格式化的时候，这个值是可以设定的。

一大块硬盘被分成了一个个小的块，用来存放文件的数据部分。这样一来，如果我们像存放一个文件，就不用给他分配一块连续的空间了。我们可以分散成一个个小块进行存放。这样就灵活得多，也比较容易添加、删除和插入数据。

inode就是文件索引的意思，我们每个文件都会对应一个inode；一个文件夹就是一个文件，也对应一个inode。

inode数据结构如下：

```
struct ext4_inode {
  __le16  i_mode;    /* File mode */
  __le16  i_uid;    /* Low 16 bits of Owner Uid */
  __le32  i_size_lo;  /* Size in bytes */
  __le32  i_atime;  /* Access time */
  __le32  i_ctime;  /* Inode Change time */
  __le32  i_mtime;  /* Modification time */
  __le32  i_dtime;  /* Deletion Time */
  __le16  i_gid;    /* Low 16 bits of Group Id */
  __le16  i_links_count;  /* Links count */
  __le32  i_blocks_lo;  /* Blocks count */
  __le32  i_flags;  /* File flags */
......
  __le32  i_block[EXT4_N_BLOCKS];/* Pointers to blocks */
  __le32  i_generation;  /* File version (for NFS) */
  __le32  i_file_acl_lo;  /* File ACL */
  __le32  i_size_high;
......
};
```