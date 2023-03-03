---
title: Linux的文件系统及文件缓存知识点整理
description: 
published: true
date: 2023-03-03T02:50:30.956Z
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

inode里面有文件的读写权限i_mode，属于哪个用户i_uid，哪个组i_gid，大小是多少i_size_io，占用多少个块i_blocks_io，i_atime是access time，是最近一次访问文件的时间；i_ctime是change time，是最近一次更改inode的时间；i_mtime是modify time，是最近一次更改文件的时间等。

所有的文件都是保存在i_block里面。具体保存规则由EXT4_N_BLOCKS决定，EXT4_N_BLOCKS有如下的定义：

```
#define  EXT4_NDIR_BLOCKS    12
#define  EXT4_IND_BLOCK      EXT4_NDIR_BLOCKS
#define  EXT4_DIND_BLOCK      (EXT4_IND_BLOCK + 1)
#define  EXT4_TIND_BLOCK      (EXT4_DIND_BLOCK + 1)
#define  EXT4_N_BLOCKS      (EXT4_TIND_BLOCK + 1)
```

在ext2和ext3中，其中前12项直接保存了块的位置，也就是说，我们可以通过i_block[0-11]，直接得到保存文件内容的块。

![2023-3-3_2054.png](/2023-3-3_2054.png)

但是，如果一个文件比较大，12块放不下。当我们用到i_block[12]的时候，就不能直接放数据块的位置了，要不然i_block很快就会用完了。

那么可以让i_block[12]指向一个块，这个块里面不放数据块，而是放数据块的位置，这个块我们称为间接块。如果文件再大一些，i_block[13]会指向一个块，我们可以用二次间接块。二次间接块里面存放了间接块的位置，间接块里面存放了数据块的位置，数据块里面存放的是真正的数据。如果文件再大点，那么i_block[14]同理。

这里面有一个非常显著的问题，对于大文件来讲，我们要多次读取硬盘才能找到相应的块，这样访问速度就会比较慢。

为了解决这个问题，ext4做了一定的改变。它引入了一个新的概念，叫作Extents。比方说，一个文件大小为128M，如果使用4k大小的块进行存储，需要32k个块。如果按照ext2或者ext3那样散着放，数量太大了。但是Extents可以用于存放连续的块，也就是说，我们可以把128M放在一个Extents里面。这样的话，对大文件的读写性能提高了，文件碎片也减少了。

Exents是一个树状结构：

![2023-3-3_83850.png](/2023-3-3_83850.png)

每个节点都有一个头，ext4_extent_header可以用来描述某个节点。

```
struct ext4_extent_header {
  __le16  eh_magic;  /* probably will support different formats */
  __le16  eh_entries;  /* number of valid entries */
  __le16  eh_max;    /* capacity of store in entries */
  __le16  eh_depth;  /* has tree real underlying blocks? */
  __le32  eh_generation;  /* generation of the tree */
};
```

eh_entries表示这个节点里面有多少项。这里的项分两种，如果是叶子节点，这一项会直接指向硬盘上的连续块的地址，我们称为数据节点ext4_extent；如果是分支节点，这一项会指向下一层的分支节点或者叶子节点，我们称为索引节点ext4_extent_idx。这两种类型的项的大小都是12个byte。

```
/*
 * This is the extent on-disk structure.
 * It's used at the bottom of the tree.
 */
struct ext4_extent {
  __le32  ee_block;  /* first logical block extent covers */
  __le16  ee_len;    /* number of blocks covered by extent */
  __le16  ee_start_hi;  /* high 16 bits of physical block */
  __le32  ee_start_lo;  /* low 32 bits of physical block */
};
/*
 * This is index on-disk structure.
 * It's used at all the levels except the bottom.
 */
struct ext4_extent_idx {
  __le32  ei_block;  /* index covers logical blocks from 'block' */
  __le32  ei_leaf_lo;  /* pointer to the physical block of the next *
         * level. leaf or next index could be there */
  __le16  ei_leaf_hi;  /* high 16 bits of physical block */
  __u16  ei_unused;
};如果文件不大，inode里面的i_block中，可以放得下一个ext4_extent_header和4项ext4_extent。所以这个时候，eh_depth为0，也即inode里面的就是叶子节点，树高度为0。
```

如果文件比较大，4个extent放不下，就要分裂成为一棵树，eh_depth>0的节点就是索引节点，其中根节点深度最大，在inode中。最底层eh_depth=0的是叶子节点。

除了根节点，其他的节点都保存在一个块4k里面，4k扣除ext4_extent_header的12个byte，剩下的能够放340项，每个extent最大能表示128MB的数据，340个extent会使你的表示的文件达到42.5GB。

## inode位图和块位图
inode的位图大小为4k，每一位对应一个inode。如果是1，表示这个inode已经被用了；如果是0，则表示没被用。block的位图同理。

在Linux操作系统里面，想要创建一个新文件，会调用open函数，并且参数会有O_CREAT。这表示当文件找不到的时候，我们就需要创建一个。那么open函数的调用过程大致是：要打开一个文件，先要根据路径找到文件夹。如果发现文件夹下面没有这个文件，同时又设置了O_CREAT，就说明我们要在这个文件夹下面创建一个文件。

创建一个文件，那么就需要创建一个inode，那么就会从文件系统里面读取inode位图，然后找到下一个为0的inode，就是空闲的inode。对于block位图，在写入文件的时候，也会有这个过程。

文件系统的格式 数据块的位图是放在一个块里面的，共4k。每位表示一个数据块，共可以表示
4 * 1024 * 8 = 215
4 * 1024 * 8 = 215

如果每个数据块也是按默认的4K，最大可以表示空间为128M，那么显然是不够的