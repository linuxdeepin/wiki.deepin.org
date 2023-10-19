---
title: dd
description: dd命令
published: true
date: 2023-10-19T09:36:53.607Z
tags: dd, 数据读写
editor: markdown
dateCreated: 2023-10-19T09:36:53.607Z
---

# Linux dd 命令
> 本文参考[yaoying](https://bbs.deepin.org/user/197440)的论坛文章[https://bbs.deepin.org/post/263490](https://bbs.deepin.org/post/263490)，在这做个记录方便大家检索。{.is-info}

---
## 一、简介
Linux dd 命令用于读取、转换并输出数据。
dd 可从标准输入或文件中读取数据，根据指定的格式来转换数据，再输出到文件、设备或标准输出。
## 二、选项
dd命令支持的选项有以下的内容：
```
if=文件名：输入文件名，默认为标准输入。即指定源文件。
of=文件名：输出文件名，默认为标准输出。即指定目的文件。
ibs=bytes：一次读入bytes个字节，即指定一个块大小为bytes个字节。
obs=bytes：一次输出bytes个字节，即指定一个块大小为bytes个字节。
bs=bytes：同时设置读入/输出的块大小为bytes个字节。
cbs=bytes：一次转换bytes个字节，即指定转换缓冲区大小。
skip=blocks：从输入文件开头跳过blocks个块后再开始复制。
seek=blocks：从输出文件开头跳过blocks个块后再开始复制。
count=blocks：仅拷贝blocks个块，块大小等于ibs指定的字节数。
conv=<关键字>，关键字可以有以下11种：
  conversion：用指定的参数转换文件。
  ascii：转换ebcdic为ascii
  ebcdic：转换ascii为ebcdic
  ibm：转换ascii为alternate ebcdic
  block：把每一行转换为长度为cbs，不足部分用空格填充
  unblock：使每一行的长度都为cbs，不足部分用空格填充
  lcase：把大写字符转换为小写字符
  ucase：把小写字符转换为大写字符
  swap：交换输入的每对字节
  noerror：出错时不停止
  notrunc：不截短输出文件
  sync：将每个输入块填充到ibs个字节，不足部分用空（NUL）字符补齐。
--help：显示帮助信息
--version：显示版本信息
```
## 三、备份还原磁盘实例
### 3.1 说明
dd指令是一个简单的复制指令，它不管源和目标的编码、格式、数据结构，简单粗暴的把二进制数据从A复制到B。所以恢复的目标硬盘甚至不需要提前分区，因为dd会把分区信息也写入。

dd指令依然是有多少数据占多少空间，所以我们可以使用gzip进行压缩。具体代码后面贴出来。

实际使用中，它因为要读取硬盘所有数据（包括垃圾数据），即便用SSD，读盘速度还是会很慢。
### 3.2 DD备份
```
dd if=/dev/sda of=/dev/sdb              # 备份整个磁盘到另外一个磁盘
dd if=/dev/sdb of=sda.img				       # 备份整个磁盘为img文件
dd if=/dev/sda | gzip > sda.img    		 # 备份并且压缩
dd if=/dev/sda bs=1M | gzip > sda.img	 # 指定块大小，备份并压缩（据说能提速）
```
### 3.2 DD复原
```
dd if=/dev/sdb of=/dev/sda                # 从另一个磁盘恢复回来
gzip -dc sda.img | dd of=/dev/sda			   # 从压缩的文件恢复出来
gzip -dc sda.img | dd of=/dev/sda bs=1M	 # 前面指定了块大小，这里也需要
dd if=/dev/sdb of=/dev/sda					      # 从另一个磁盘恢复回来
gzip -dc sda.img | dd of=/dev/sda			   # 从压缩的文件恢复出来
gzip -dc sda.img | dd of=/dev/sda bs=1M	 # 前面指定了块大小，这里也需要
```
## 四、最后
dd的优点是可以简单粗暴的把整个磁盘或者文件夹打包成img文件，比如我们在做安卓镜像时候很香。
因为是直接简单粗暴复制文件，所以也比较安全，不用担心复制时候权限丢失或者其他的问题。  
但日常备份、复制我觉得没有rsync、cp快。