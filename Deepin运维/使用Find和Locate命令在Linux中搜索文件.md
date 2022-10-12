---
title: 使用Find和Locate命令在Linux中搜索文件
description: 
published: true
date: 2022-10-12T05:59:22.349Z
tags: find locate
editor: markdown
dateCreated: 2022-10-12T05:52:41.081Z
---

# 使用Find和Locate命令在Linux中搜索文件
## 按名称搜索
搜索文件最直接的方法就是按名称搜索，要使用 find 命令按名称查找文件，可以用以下语法：

`find -name "query"`

这种方式搜索会区分大小写。如果要按名称查找文件但忽略大小写，请使用 -iname 选项：

`find -iname "query"`

如果要查找不匹配某个关键字的所有文件，可以使用 -not或者! 反转搜索：
```
find -not -name "query_to_avoid"
find ! -name "query_to_avoid"
```

## 按类型搜索

也可以使用 -type 选项指定要查找的文件类型。如下操作，搜索/dev目录录下的 b(块设备)：

`find /dev -type b`

![2022-10-12_73253.png](/2022-10-12_73253.png)

以下是一些可用于指定文件类型的选项：

f: 常规文件

d: 目录

l: 符号链接

c: 字符设备

b: 块设备

可以使用如下命令搜索所有以 .conf 结尾的文件。该示例在 /etc 目录中搜索匹配的文件：

`find /etc -type f -name "*.conf"`

![2022-10-12_54118.png](/2022-10-12_54118.png)

## 按时间和大小过滤

find 提供了多种按文件大小和时间过滤结果的方法。

### 文件大小

可以使用 -size 参数按文件大小过滤文件。为此，必须在数值的末尾添加一个特殊的后缀，表示按字节、兆字节、千兆字节还是其他大小来计算大小。以下是一些常用的尺寸后缀：
c: bytes
k: kilobytes
M: megabytes
G: gigabytes
b: 512-byte blocks

为了说明这一点，以下命令将查找 /usr 目录中正好为 50c 的每个文件、大于20M的文件、小于1M的文件：
`find /usr -size 50c`
`find /usr -size +20M`
`find /usr -size -1M`

### 时间

对于系统上的每个文件，Linux 都存储有关访问时间、修改时间和更改时间的时间数据。
-atime: Access Time, 文件最后一次被读写的时间.
-mtime: Modification Time, 文件内容最后一次被修改的时间.
-ctime: Change Time, 文件的inode元数据最后一次更改的时间.

例如，要查找 /usr 目录中最近一天内修改过的文件，请运行以下命令：
`find /usr -mtime 1`

如果查找最近两天内访问过的文件，可以运行以下命令：
`find /usr -atime -2`

要查找上次更改元信息超过 3 天的文件，可以执行以下操作：
`find /usr -ctime +3`

这些选项还具有可用于指定分钟而不是天的配套参数，这将给出在一分钟内修改过的文件：
`find /var/log -mmin -1`

## 按所有者和权限查找

还可以分别使用 -user 和 -group 参数按拥有文件的用户或组搜索文件。若要查找 chrony 用户在/var 目录中拥有的文件，请运行以下命令：

[root@LinuxProbe ~]# find /var -user chrony
同样，可以通过键入以下命令指定kmem组在/etc目录中拥有的文件 ：
[root@LinuxProbe ~]# find / -group kmem
也可以使用-perm选项搜索指定权限的文件：
[root@LinuxProber ~]# find /var/log -perm 644

## 对查找结果执行命令

你可以使用以下语法使用 -exec 参数对找到匹配项的所有内容执行任意操作。{} 用作查找匹配文件的占位符。这 \;让 find 知道命令在哪里结束。

例如，查找/etc目录中的*.conf文件，并使用ls -l列出文件信息：
[root@LinuxProbe ~]# find /etc -name "*.conf" -exec ls -l {} \;

/home/uos/Desktop/640.png
