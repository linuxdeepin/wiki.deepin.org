---
title: Linux 内存泄露案例分析和内存管理分享
description: 这篇文章是一篇关于Linux内存泄露案例分析和内存管理分享的技术文章，详细介绍了问题的发现、排查过程、解决方案以及相关的Linux内存管理知识。
published: true
date: 2024-11-08T03:04:12.909Z
tags: linux内存泄露, linux内存管理, 内存泄露, 内存管理
editor: markdown
dateCreated: 2024-11-08T03:01:46.162Z
---

# Linux 内存泄露案例分析和内存管理分享
这篇文章是一篇关于Linux内存泄露案例分析和内存管理分享的技术文章，详细介绍了问题的发现、排查过程、解决方案以及相关的Linux内存管理知识。以下是文章的主要内容：

    问题描述
        内存报警：近期运维同事接到线上LB（负载均衡）服务内存报警，部分机器内存使用率超过80%，甚至达到90%。
        影响：LB服务是零售、物流、科技等业务服务的流量入口，一旦有故障影响较大。

    排查过程
        初步检查：通过 cat /proc/meminfo查看Slab的内核内存可能有泄漏。
        深入分析：使用 slabtop命令发现dentry对象占比高，怀疑与socket文件有关。
        确认问题：通过curl论坛确认curl-7.19.7版本在发送HTTPS请求时存在dentry泄漏的bug。

    解决方案
        临时措施：停止探测脚本，通过 drop_caches清理缓存。
        永久解决方案：在大促过后，探测脚本中设置环境变量 NSS_SDB_USE_CACHE，彻底修复问题。

    复盘和总结
        根本原因：curl-7.19.7依赖的NSS库存在dentry泄漏的bug。
        Linux内存管理知识：
            内存寻址：逻辑地址、线性地址、物理地址。
            分页机制：页、页框、页表。
            NUMA架构：非一致性内存架构。
            伙伴关系算法：解决内存管理外碎片问题。
            Slab机制：基于对象的内存分配器。
            进程内存分布：代码段、数据段、BSS段、堆、栈。

    Linux内存检测工具
        free命令：监控系统内存。
        top命令：查看系统内存及进程内存。
        smaps文件：查看某进程虚拟内存空间的分布情况。
        vmstat命令：实时动态监视操作系统的虚拟内存、进程、CPU活动。
        meminfo文件：记录系统内存使用的详细情况。

通过以上内容，读者可以全面了解Linux内存泄露的排查和解决方法，并掌握相关的Linux内存管理知识。

## 一、问题

近期我们运维同事接到线上 LB（负载均衡）服务内存报警，运维同事反馈说 LB 集群有部分机器的内存使用率超过 80%，有的甚至超过 90%，而且内存使用率还再不停的增长。接到内存报警的消息，让整个团队都比较紧张，我们团队负责的 LB 服务是零售、物流、科技等业务服务的流量入口，承接上万个服务的流量转发，一旦有故障影响业务服务比较多，必须马上着手解决内存暴涨的问题。目前只是内存报警，暂时不影响业务，先将内存使用率 90% 以上的 LB 服务下线，防止内存过高导致 LB 服务崩溃，影响业务，运维同事密切关注相关的内存报警的消息。

## 二、排查过程

经过开发同学通过 cat /proc/meminfo 查看 Slab 的内核内存可能有泄漏。

`$ cat /proc/meminfo
MemTotal:       65922868 kB
MemFree:         9001452 kB
...

Slab:           39242216 kB
SReclaimable:   38506072 kB
SUnreclaim:       736144 kB
....`

通过 slabtop 命令分析 slab 发现内核中 dentry 对象占比高，考虑到 dentry 对象跟文件有关，Linux 中一切皆可以为文件，这个可能跟 socket 文件有关，通过进一步排查发现 LB 服务上有个 curl 发送的 HTTPS 探测脚本，这个脚本存在 dentry 对象泄漏，并且在 curl 论坛上找到一篇文章确认了这个问题（Red Hat Bugzilla – Bug 1779325），这个文章说明了 curl-7.19.7 版本在发送 HTTPS 请求时，curl 依赖的 NSS 库存在 dentry 泄漏的 bug，我查看一下我们 curl 版本就是 7.19.7，问题终于真相大白了！！！

`$ curl -V
curl 7.19.7 (x86_64-redhat-linux-gnu) libcurl/7.19.7 NSS/3.15.3 zlib/1.2.3 libidn/1.18 libssh2/1.4.2
Protocols: tftp ftp telnet dict ldap ldaps http file https ftps scp sftp
Features: GSS-Negotiate IDN IPv6 Largefile NTLM SSL libz

$ rpm -aq|grep nss-
nss-util-3.16.1-3.el6.x86_64
nss-sysinit-3.16.1-14.el6.x86_64
nss-softokn-freebl-3.14.3-17.el6.x86_64
nss-softokn-3.14.3-17.el6.x86_64
nss-3.16.1-14.el6.x86_64
nss-tools-3.16.1-14.el6.x86_64`

文章中介绍可以设置环境变量 NSS_SDB_USE_CACHE 修复这个 bug，我们验证通过了这个解决方案。

## 三、解决方案

1、目前先将探测脚本停止，在业务流量低峰时将内存使用率超过 90% 的服务先通过 drop_caches 清理一下缓存。

2、等大促过后，探测脚本中设置环境变量 NSS_SDB_USE_CACHE，彻底修复这个问题。

## 四、复盘和总结

这次内存暴涨的问题根本原因是 curl-7.19.7 依赖的 NSS 库存在 dentry 泄漏的 bug 导致的，探测脚本只是将这个问题暴露出来。这次问题由 Linux 内存泄漏引发的问题，因此以点带面再次系统学习一下 Linux 内存管理的知识非常有必要，对我们以后排查内存暴涨的问题非常有帮助。
### 1）Linux 内存寻址

Linux 内核主要通过虚拟内存管理进程的地址空间，内核进程和用户进程都只会分配虚拟内存，不会分配物理内存，通过内存寻址将虚拟内存与物理内存做映射。Linux 内核中有三种地址，

a、逻辑地址，每个逻辑地址都由一段 (segment) 和偏移量 (offset) 组成，偏移量指明了从段开始的地方到实际地址之间的距离。

b、线性地址，又称虚拟地址，是一个 32 个无符号整数，32 位机器内存高达 4GB，通常用十六进制数字表示，Linux 进程的内存一般说的都是这个内存。

c、物理地址，用于内存芯片级内存单元寻址。它们与从 CPU 的地址引脚发送到内存总线上的电信号对应。

Linux 中的内存控制单元 (MMU) 通过一种称为分段单元 (segmentation unit) 的硬件电路把一个逻辑地址转换成线性地址，接着，第二个称为分页单元 (paging unit) 的硬件电路把线性地址转换成一个物理地址。
![2024-11-8_52224.png](/2024-11-8_52224.png)

### 2）Linux 分页机制

分页单元把线性地址转换成物理地址。线性地址被分成以固定长度为单位的组，称为页 (page)。页内部连续的线性地址被映射到连续的物理地址中。一般 "页" 既指一组线性地址，又指包含这组地址中的数据。分页单元把所有的 RAM 分成固定长度的页框 (page frame)，也成物理页。每一页框包含一个页 (page)，也就是说一个页框的长度与一个页的长度一致。页框是主存的一部分，因此也是一个存储区域。区分一页和一个页框是很重要的，前者只是一个数据块，可以存放任何页框或者磁盘中。把线性地址映射到物理地址的数据结构称为页表 (page table)。页表存放在主存中，并在启用分页单元之前必须有内核对页表进行适当的初始化。

x86_64 的 Linux 内核采用 4 级分页模型，一般一页 4K，4 种页表：

a、页全局目录
b、页上级目录
c、页中间目录
d、页表

页全局目录包含若干页上级目录，页上级目录又依次包含若干页中间目录的地址，而页中间目录又包含若干页表的地址。每个页表项指向一个页框。线性地址被分成 5 部分。
![2024-11-8_74099.png](/2024-11-8_74099.png)

### 3）NUMA 架构

随着 CPU 进入多核时代，多核 CPU 通过一条数据总线访问内存延迟很大，因此** NUMA** 架构应运而生，NUMA 架构全称为非一致性内存架构 (Non Uniform Memory Architecture)，系统的物理内存被划分为几个节点 (node)，每个 node 绑定不同的 CPU 核，本地 CPU 核直接访问本地内存 node 节点延迟最小。
![2024-11-8_3800.png](/2024-11-8_3800.png)
可以通过 lscpu 命令查看 NUMA 与 CPU 核的关系。

`$ lscpu

Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                32
On-line CPU(s) list:   0-31
Thread(s) per core:    2
Core(s) per socket:    8
Socket(s):             2
NUMA node(s):          2
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 62
Stepping:              4
CPU MHz:               2001.000
BogoMIPS:              3999.43
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              20480K
NUMA node0 CPU(s):     0-7,16-23      #这些核绑定在numa 0
NUMA node1 CPU(s):     8-15,24-31     #这些核绑定在numa 1`

![2024-11-8_87489.png](/2024-11-8_87489.png)

Linux 内核是操作系统中优先级最高的，内核函数申请内存必须及时分配适当的内存，用户态进程申请内存被认为是不紧迫的，内核尽量推迟给用户态的进程动态分配内存。

a、请求调页，推迟到进程要访问的页不在 RAM 中时为止，引发一个缺页异常。

b、写时复制 (COW)，父、子进程共享页框而不是复制页框，但是共享页框不能被修改，只有当父 / 子进程试图改写共享页框时，内核才将共享页框复制一个新的页框并标记为可写。

### 4）Linux 内存检测工具

a、free 命令可以监控系统内存
`$ free -h
              total        used        free      shared  buff/cache   available
Mem:           31Gi        13Gi       8.0Gi       747Mi        10Gi        16Gi
Swap:         2.0Gi       321Mi       1.7Gi`

b、top 命令查看系统内存以及进程内存

•<span>VIRT</span> Virtual Memory Size (KiB)：进程使用的所有虚拟内存，包括代码（code）、数据（data）、共享库（shared libraries），以及被换出（swap out）到交换区和映射了（map）但尚未使用（未载入实体内存）的部分。

•<span>RES</span> Resident Memory Size (KiB)：进程所占用的所有实体内存（physical memory），不包括被换出到交换区的部分。

•<span>SHR</span> Shared Memory Size (KiB)：进程可读的全部共享内存，并非所有部分都包含在 <span>RES</span> 中。它反映了可能被其他进程共享的内存部分。

c、smaps 文件

cat /proc/$pid/smaps 查看某进程虚拟内存空间的分布情况

`0082f000-00852000 rw-p 0022f000 08:05 4326085    
/usr/bin/nginx/sbin/nginx

Size:                140 kB
Rss:                 140 kB
Pss:                  78 kB
Shared_Clean:         56 kB
Shared_Dirty:         68 kB
Private_Clean:         4 kB
Private_Dirty:        12 kB
Referenced:          120 kB
Anonymous:            80 kB
AnonHugePages:         0 kB
Swap:                  0 kB
KernelPageSize:        4 kB
MMUPageSize:           4 kB`

d、vmstat

vmstat 是 Virtual Meomory Statistics（虚拟内存统计）的缩写，可实时动态监视操作系统的虚拟内存、进程、CPU 活动。

`## 每秒统计3次
$ vmstat 1 3
procs -----------memory---------------- ---swap-- -----io---- --system-- -----cpu-----
 r  b    swpd   free   buff  cache       si   so    bi    bo   in   cs us sy id  wa st
 0  0      0 233483840 758304 20795596    0    0     0     1    0    0  0  0 100  0  0
 0  0      0 233483936 758304 20795596    0    0     0     0 1052 1569  0  0 100  0  0
 0  0      0 233483920 758304 20795596    0    0     0     0  966 1558  0  0 100  0  0`
 
 e、meminfo 文件

Linux 系统中 /proc/meminfo 这个文件用来记录了系统内存使用的详细情况。

`$ cat /proc/meminfo

MemTotal:        8052444 kB
MemFree:         2754588 kB
MemAvailable:    3934252 kB
Buffers:          137128 kB
Cached:          1948128 kB
SwapCached:            0 kB
Active:          3650920 kB
Inactive:        1343420 kB
Active(anon):    2913304 kB
Inactive(anon):   727808 kB
Active(file):     737616 kB
Inactive(file):   615612 kB
Unevictable:         196 kB
Mlocked:             196 kB
SwapTotal:       8265724 kB
SwapFree:        8265724 kB
Dirty:               104 kB
Writeback:             0 kB
AnonPages:       2909332 kB
Mapped:           815524 kB
Shmem:            732032 kB
Slab:             153096 kB
SReclaimable:      99684 kB
SUnreclaim:        53412 kB
KernelStack:       14288 kB
PageTables:        62192 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:    12291944 kB
Committed_AS:   11398920 kB
VmallocTotal:   34359738367 kB
VmallocUsed:           0 kB
VmallocChunk:          0 kB
HardwareCorrupted:     0 kB
AnonHugePages:   1380352 kB
CmaTotal:              0 kB
CmaFree:               0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
DirectMap4k:      201472 kB
DirectMap2M:     5967872 kB
DirectMap1G:     3145728 kB`

---
原文作者：京东云开发者

原文链接：Linux 内存泄露案例分析和内存管理分享