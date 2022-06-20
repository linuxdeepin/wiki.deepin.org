---
title: linux-swap交换分区详解
description: 
published: true
date: 2022-06-20T08:09:42.610Z
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

`/proc/sys/vm/swappiness` 这个文件，是个可以用来调整跟swap相关的参数。这个文件的默认值是60，可以的取值范围是0-100

```
$  cat /proc/sys/vm/swappiness
60
$ sysctl -q vm.swappiness
vm.swappiness = 60
 
$ sysctl vm.swappiness=10
$ sysctl -q vm.swappiness
vm.swappiness = 10
```

**持久操作**
```
1、$ vim /etc/sysctl.conf
2、vm.swappiness=10    #到末行，需要重启生效```
```

**定义内核使用swap的积极程度：**

值越高，内核就会越积极的使用swap；
值越低，就会降低对swap的使用积极性。
如果这个值为0，那么内存在free和file-backed使用的页面总量小于高水位标记
（high watermark）之前，不会发生交换。调整为0意味着，尽量通过清缓存来回收内存。
设置为100表示内存发生回收时，从cache回收内存和swap交换的优先级一样。
就是说，如果目前需求100M内存，那么较大机率会从cache中清除50M内存，再将匿名页换出50M，
把回收到的内存给应用程序使用。但是这还要看cache中是否能有空间，以及swap是否可以交换50m。
file-backed:就是上文所说的文件映射页的大小


## 3.什么时候会进行swap操作？
kswapd周期检查和直接内存回收的两种内存回收机制。当申请的内存大于剩余内存的时候，
就会触发直接回收。那么kswapd进程在周期检查的时候触发回收的条件是什么呢？
还是从设计角度来看，kswapd进程要周期对内存进行检测，达到一定阈值的时候开始进行内存回收。
这个所谓的阈值可以理解为内存目前的使用压力，就是说，虽然我们还有剩余内存，
但是当剩余内存比较小的时候，就是内存压力较大的时候，就应该开始试图回收些内存了，
这样才能保证系统尽可能的有足够的内存给突发的内存申请所使用。

kswapd根据内存水位标记决定是否开始回收内存，如果标记达到low就开始回收，回收到剩余内存达到high标记为止。

查看当前系统的内存水位标记
$ `cat /proc/zoneinfo`


## 4.swap分区的优先级（priority）
可以使用-p参数指定相关swap空间的优先级， 值越大优先级越高 ，可以指定的数字范围是－1到32767.

$ `swapoff /dev/sdc1; swapon -p 0 /dev/sdc1`
$ `swapon -s`
Filename    Type  Size Used Priority
/dev/sdc1                             file     2097148 0 0
 
$ `cat /proc/swaps`
Filename    Type  Size Used Priority
/dev/sdc1                             file     2097148 0 0
`/etc/ fstab`放入一个条目，以使其在每次Linux重新启动时生效：

/dev/sdc1 swap swap pri=0 0 0

## 5.启停swap

1 $ `swapoff -a`  停止
2 $ `swapon -a`  启动

## 6.创建swap空间
```
制作swap文件
dd if=/dev/sda3 of=./swapfile bs=1M count=1G
mkswap ./swapfile
 
启用swap文件
$ swapon swapfile
 
$ swapon -s
Filename    Type  Size Used Priority
/swap.img                               file     2097148 3340 0
/mnt/swapfile            file     6388156 0 -2
 
关闭swap空间
$ swapoff swapfile
$ swapon -s
Filename    Type  Size Used Priority
/swap.img                               file     2097148 3156 0
```