---
title: linux内存相关的命令详解
description: 
published: true
date: 2022-07-12T02:30:37.097Z
tags: linux 内存
editor: markdown
dateCreated: 2022-07-12T02:15:56.732Z
---

# 一、内存相关命令

## 1、free – 显示系统内存使用量情况

> free命令的功能是显示系统内存使用量情况，包含物理和交换内存的总量、使用量和空闲量情况。

**语法格式**

```bash
free [参数]
1
```

**常用参数**

| 参数 | 解析                             |
| ---- | -------------------------------- |
| -b   | 以Byte显示内存使用情况           |
| -k   | 以kb为单位显示内存使用情况       |
| -m   | 以mb为单位显示内存使用情况       |
| -g   | 以gb为单位显示内存使用情况       |
| -s   | 持续显示内存                     |
| -t   | 显示内存使用总合                 |
| -h   | 以易读的单位方式显示内存使用情况 |

**参考实例**

以默认的容量单位显示内存使用量信息：

[root@root ~]# `free`
              total        used        free      shared  buff/cache   available
Mem:       65353144     8354168    53956676       39716     3042300    56522796
Swap:             0           0           0

以MB位单位显示内存使用量信息：

[root@root ~]# `free -m`
              total        used        free      shared  buff/cache   available
Mem:          63821        8154       52696          38        2971       55202
Swap:             0           0           0


以易读的单位显示内存使用量信息：

[root@root ~]# `free -h`
              total        used        free      shared  buff/cache   available
Mem:            62G        8.0G         51G         38M        2.9G         53G
Swap:            0B          0B          0B

以易读的单位显示内存使用量信息，每个10秒刷新一次：

[root@root ~]# `free -hs 10`
              total        used        free      shared  buff/cache   available
Mem:            62G        8.0G         51G         38M        2.9G         53G
Swap:            0B          0B          0B

              total        used        free      shared  buff/cache   available
Mem:            62G        8.0G         51G         38M        2.9G         53G
Swap:            0B          0B          0B

字段解析
#total：物理内存大小，就是机器实际的内存
#used：已使用的内存大小，这个值包括了cached和应用程序实际使用的内存
#free：未被使用的内存大小（俗称：空闲内存）
#shared：共享内存大小，是进程间通信的一种方式
#buffers：被缓冲区占用的内存大小
#cached：被缓存占用的内存大小

## 2、cat /proc/meminfo
meminfo中包含所有的内存相关信息。

[root@root ~]# cat /proc/meminfo 
MemTotal:       65353144 kB
MemFree:        53956672 kB
MemAvailable:   56523144 kB
Buffers:           49180 kB
Cached:          2889216 kB
SwapCached:            0 kB
Active:          7603092 kB
Inactive:        2691064 kB
Active(anon):    7356648 kB
Inactive(anon):    38800 kB
Active(file):     246444 kB
Inactive(file):  2652264 kB
Unevictable:           0 kB
Mlocked:               0 kB
SwapTotal:             0 kB
SwapFree:              0 kB
Dirty:                36 kB
Writeback:             0 kB
AnonPages:       7360372 kB
Mapped:           764500 kB
Shmem:             39684 kB
Slab:             306384 kB
SReclaimable:     104216 kB
SUnreclaim:       202168 kB
KernelStack:       22464 kB
PageTables:        30644 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:    32676572 kB #系统中可以申请的虚拟内存最大值
Committed_AS:   27883644 kB #系统已经申请的虚拟内存
VmallocTotal:   34359738367 kB
VmallocUsed:      525916 kB
VmallocChunk:   34325372924 kB
HardwareCorrupted:     0 kB
AnonHugePages:   4573184 kB
CmaTotal:              0 kB
CmaFree:               0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
DirectMap4k:      771124 kB
DirectMap2M:    11393024 kB
DirectMap1G:    56623104 kB
DirectMap1G:    56623104 kB

# 二、内存相关问题

## 1、内存泄漏

解析：

> 如果程序运行过程中不能正常回收不用的内存，那么一段时间就会导致内存增长很高，最终导致系统不可以用，这种情况称为内存泄漏。

内存泄漏可以使用内存分享工具[valgrind](https://so.csdn.net/so/search?q=valgrind&spm=1001.2101.3001.7020)进行内存分析。

[root@root ~]# yum -y install valgrind
[root@root ~]# valgrind --tool=memcheck  ./mem_leak -t
==1738== Memcheck, a memory error detector
==1738== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==1738== Using Valgrind-3.13.0 and LibVEX; rerun with -h for copyright info
==1738== Command: ./mem_leak -t
==1738== 
==1738== 
==1738== HEAP SUMMARY:
==1738==     in use at exit: 4 bytes in 1 blocks
==1738==   total heap usage: 1 allocs, 0 frees, 4 bytes allocated
==1738== 
==1738== LEAK SUMMARY:
==1738==    definitely lost: 4 bytes in 1 blocks
==1738==    indirectly lost: 0 bytes in 0 blocks
==1738==      possibly lost: 0 bytes in 0 blocks
==1738==    still reachable: 0 bytes in 0 blocks
==1738==         suppressed: 0 bytes in 0 blocks
==1738== Rerun with --leak-check=full to see details of leaked memory
==1738== 
==1738== For counts of detected and suppressed errors, rerun with: -v
==1738== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

## 2、虚拟内存、物理内存
内存的最小单位为页（paging）,默认情况一页是4K。

虚拟内存与物理内存
  当进程向操作系统申请10G内存时，操作系统收到请求后先进行自我检查，经过分析后决定给予进程10G内存空间，但是系统此时并没有真正给进程10G，系统会判断进程运行实际需要的内存大小，比如该进程需要300M就够用了，所以系统只划分300M内存给进程运行，这种实际分配的内存称为物理内存。
  
[root@root ~]# ps aux|head -1
USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
#VSZ 虚拟内存
#RSS 物理内存

## 3、内存溢出
内存溢出（out of memory，OOM），当进程运行向系统申请内存时，系统没有更多的进程分配给该进程了，就会出现内存溢出。内存溢出后系统会杀掉系统中的一些进程来释放内存，通常OOM Killer杀死的都是占用内存较多的服务，直到内存够用为止，所以内存溢出的直观现象通常是某些服务异常或宕机。当发生内存溢出后可以通过dmesg命令或者通过/var/log/messages来快速确定。

[root@root ~]# dmesg |tail -5
[ 3613.796390] [ 1277]     0  1277    28983        1      16      217             0 bash
[ 3613.796393] [ 1446]     0  1446    29003        0      16      221             0 bash
[ 3613.796395] [ 1546]     0  1546   774216   213712    1439   513149             0 python
[ 3613.796396] Out of memory: Kill process 1546 (python) score 912 or sacrifice child
[ 3613.796422] Killed process 1546 (python) total-vm:3096864kB, anon-rss:854848kB, file-rss:0kB, shmem-rss:0kB

[root@root ~]# cat /proc/sys/vm/panic_on_oom 
0
[root@root ~]# echo 1 > /proc/sys/vm/panic_on_oom

panic_on_oom该参数的值共有三个选项:
0是触发oom killer进行杀进程处理(/proc/sys/vm/oom_kill_allocating_task默认值为0，结束占用内存多的进程;如果设置为1,就杀死当前申请内存的进程)
2是直接触发kernel panic（类似于Windows的蓝屏），此时系统开发人员可以连接进行debug,但是对其他人并没有什么用。更多希望系统快速重启（/proc/sys/kernel/panic设置多少秒后重启）
1是根据不同情况会有不同处理;



## 4、Overcommit
一般情况下，进程并不会一次用光申请的内存，所以操作系统为了提高内存使用率，会向进程“超卖”内存，以便能响应更多的进程内存申请。但是为了保证程序的稳定运行，系统并不会无限制响应内存申请，这样可以防止内存申请过多，导致系统本身内存空间不足而无法正常运行。Linux系统中使用OverCommit的方式控制内存的申请。

内核参数overcommit_memory 内存分配策略
可选值：0、1、2。
0， 表示内核将检查是否有足够的可用内存供应用进程使用；如果有足够的可用内存，内存申请允许；否则，引发OOM。
1， 表示内核允许分配所有的物理内存，而不管当前的内存状态如何。
2， 表示内核不允许进程申请超过系统设置大小的内存空间。在这种模式下，系统设置的可申请内存空间大小为：Swap+RAM*(“/proc/sys/vm/overcommit_ratio”/100)
说明:/proc/sys/vm/overcommit_ratio就是系统最大可以分配内存的百分比，默认是50.


