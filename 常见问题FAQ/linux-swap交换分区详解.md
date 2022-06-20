---
title: linux-swap交换分区详解
description: 
published: true
date: 2022-06-20T07:55:11.722Z
tags: swap
editor: markdown
dateCreated: 2022-06-20T07:26:50.183Z
---

# linux swap交换分区详解

## 1、什么是SWAP  

```$ swapon -s
Filename    Type  Size Used Priority
/swap.img                               file     2097148 0 -2
```

从功能上讲，交换分区主要是在内存不够用的时候，将部分内存上的数据交换到swap空间上，以便让系统不会因内存不够用而导致oom或者更致命的情况出现。所以，当内存使用存在压力，开始触发内存回收的行为时，就可能会使用swap空间

## 2、swappiness调节什么