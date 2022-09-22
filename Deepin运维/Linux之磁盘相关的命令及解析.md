---
title: Linux之磁盘相关的命令及解析
description: 
published: true
date: 2022-07-13T08:50:09.774Z
tags: 磁盘
editor: markdown
dateCreated: 2022-07-13T07:48:32.117Z
---

# 一、df – 显示磁盘空间使用情况

df命令来自于英文词组”Disk Free“的缩写，其功能是用于显示系统上磁盘空间的使用量情况。df命令显示的磁盘使用量情况含可用、已有及使用率等信息，默认单位为Kb，建议使用-h参数进行单位换算。
df命令可以查看磁盘使用情况和挂载信息

**语法格式**

 df [参数] [对象磁盘/分区]


**常用参数**

| 参数              | 解析                        |
| ----------------- | --------------------------- |
| -a                | 显示所有系统文件            |
| -B                | <块大小> 指定显示时的块大小 |
| -h                | 以易读的方式显示            |
| -H                | 以1000字节为换算单位来显示  |
| -i                | 显示索引字节信息            |
| -k                | 指定块大小为1KB             |
| -l                | 只显示本地文件系统          |
| -t <文件系统类型> | 只显示指定类型的文件系统    |
| -T                | 输出时显示文件系统类型      |

**参考实例**

带有容量单位的显示系统全部磁盘使用量情况：

[root@linuxcool ~]# df -h
文件系统        容量  已用  可用 已用% 挂载点
devtmpfs         32G     0   32G    0% /dev
tmpfs            32G     0   32G    0% /dev/shm
tmpfs            32G   12M   32G    1% /run
tmpfs            32G     0   32G    0% /sys/fs/cgroup
/dev/nvme0n1p2  200G  128G   72G   65% /
/dev/nvme0n1p1  2.0G  217M  1.8G   11% /boot
/dev/sda        1.8T  1.2T  526G   70% /data

带有容量单位的显示指定磁盘分区使用量情况：

[root@linuxcool ~]# df -h /boot
文件系统        容量  已用  可用 已用% 挂载点
/dev/nvme0n1p1  2.0G  217M  1.8G   11% /boot

显示系统中所有文件系统格式为xfs的磁盘分区使用量情况：

[root@linuxcool ~]# df -t xfs
文件系统           1K-块      已用     可用 已用% 挂载点
/dev/nvme0n1p2 209612800 134200384 75412416   65% /
/dev/nvme0n1p1   2086912    221760  1865152   11% /boot

常用的查看磁盘空间的命令及参数：

[root@hpt3 ~]# df -Th
文件系统       类型      容量  已用  可用 已用% 挂载点
devtmpfs       devtmpfs   32G     0   32G    0% /dev
tmpfs          tmpfs      32G     0   32G    0% /dev/shm
tmpfs          tmpfs      32G   12M   32G    1% /run
tmpfs          tmpfs      32G     0   32G    0% /sys/fs/cgroup
/dev/nvme0n1p2 xfs       200G  128G   72G   65% /
/dev/nvme0n1p1 xfs       2.0G  217M  1.8G   11% /boot
/dev/sda       ext4      1.8T  1.2T  526G   70% /data

# 二、du – 查看文件或目录的大小

du命令来自于英文词组“Disk Usage”的缩写，其功能是用于查看文件或目录的大小。人们经常会把df和du命令混淆，df是用于查看磁盘或分区使用情况的命令，而du命令则是用于按照指定容量单位来查看文件或目录在磁盘中的占用情况。

**语法格式**

du [参数] 文件

**常用参数**

| 参数 | 解析                   |
| ---- | ---------------------- |
| -a   | 显示目录中所有文件大小 |
| -k   | 以KB为单位显示文件大小 |
| -m   | 以MB为单位显示文件大小 |
| -g   | 以GB为单位显示文件大小 |
| -h   | 以易读方式显示文件大小 |
| -s   | 仅显示总计             |

**参考实例**

以易读的容量格式显示指定目录内各个文件的大小信息：

[root@root ~]# cd /home/restart/
[root@restart ~]# du -sh *
4.0K	if.sh
6.1M	main
4.0K	restart.sh

以易读的容量格式显示指定目录内总文件的大小信息：

[root@root ~]# du -sh restart/
6.1M	restart/

# 三、iostat – 监视系统输入输出设备和CPU的使用情况

> iostat 用于监视系统输入输出设备和 CPU 的使用情况。它的特点是汇报磁盘活动统计情况，同时也会汇报出 CPU 使用情况。iostat 不能对某个进程进行深入分析，仅对系统的整体情况进行分析。



**语法格式**

```
iostat [ 选项 ] [ <时间间隔> [ <次数> ] ]
```

**常用参数**

| **参数** | **解析**                                   |
| -------- | ------------------------------------------ |
| -c       | 显示 CPU 使用情况                          |
| -d       | 指定磁盘设备                               |
| -x       | 显示详细的信息                             |
| -k       | 显示状态以千字节每秒为单位，而不使用块每秒 |
| -m       | 显示状态以兆字节每秒为单位                 |
| -p       | 仅显示块设备和所有被使用的其他分区的状态   |
| -t       | 显示每个报告产生时的时间                   |

**参考实例**

 

- 每隔两秒报告一次：

```
[root@root ~]# iostat -d 2
```

- 每 2 秒报告一次，持续 6 次：

```
[root@root ~]# iostat -d 2 6
```

- 显示 sda 与 sdc 的统计数据，每 2 秒报告一次，持续 6 次：

```
[root@root ~]# iostat -x sda sdc 2 6
```

**字段解析**

```perl
[root@root ~]# iostat
Linux 3.10.0-1062.9.1.el7.x86_64 (root)  2022年06月07日  _x86_64_ (48 CPU)
 
avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           1.19    0.00    3.60    0.01    0.00   95.20
 
Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
nvme0n1           0.56        23.49         2.63    1606516     179564
sda               1.01        20.67         2.66    1413473     181680
```

- tps：该设备每秒的传输次数

- kB_read/s：每秒从设备（drive expressed）读取的数据量；

- kB_wrtn/s：每秒向设备（drive expressed）写入的数据量；

- kB_read：读取的总数据量；

- kB_wrtn：写入的总数量数据量

[root@root ~]# iostat -d -x -k 1 1
Linux 3.10.0-1062.9.1.el7.x86_64 (root)  2022年06月07日  _x86_64_ (48 CPU)

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
nvme0n1           0.00     0.00    0.31    0.26    23.39     2.62    92.63     0.00    2.95    0.21    6.23   1.14   0.06
sda               0.02     0.18    0.68    0.32    20.58     2.65    46.20     0.01    6.65    5.35    9.42   3.03   0.31
  
- rrqm/s 每秒对该设备的读请求被合并次数，文件系统会对读取同块 (block) 的请求进行合并
- wrqm/s 每秒对该设备的写请求被合并次数
- r/s 每秒完成的读次数
- w/s 每秒完成的写次数
- rkB/s 每秒读数据量 (kB 为单位)
- wkB/s 每秒写数据量 (kB 为单位)
- avgrq-sz 平均每次 IO 操作的数据量 (扇区数为单位)
- avgqu-sz 平均等待处理的 IO 请求队列长度
- await 平均每次 IO 请求等待时间 (包括等待时间和处理时间，毫秒为单位)
- svctm 平均每次 IO 请求的处理时间 (毫秒为单位)
- %util 采用周期内用于 IO 操作的时间比率，即 IO 队列非空的时间比率
- 如果 %util 接近 100%，说明产生的 I/O 请求太多，I/O 系统已经满负荷

# 四、iotop – 监视磁盘 I/O 状态

> iotop 命令用来动态地查看磁盘 IO 情况, 使用方法和 top 命令十分类似。iotop 具有与 top 相似的 UI，其中包括 PID、用户、I/O、进程等相关信息。

> Linux 下的 IO 统计工具如 iostat，nmon 等大多数是只能统计到 per 设备的读写情况，如果你想知道每个进程是如何使用 IO 的就比较麻烦，使用 iotop 命令可以很方便的查看。

> 下载 iotop 命令要用 yum 在线源

**语法格式**

```
iotop [参数]
```

**常用参数**

| **参数** | **解析**                                 |
| -------- | ---------------------------------------- |
| -o       | 只显示有 io 操作的进程                   |
| -b       | 运行在非交互式的模式，主要用作记录到文件 |
| -n NUM   | 在非交互式模式下，设置显示的次数，       |
| -d SEC   | 设置显示的间隔秒数，支持非整数值         |
| -p PID   | 只显示指定 PID 的信息                    |
| -u USER  | 监控的进程用户                           |

**参考实例**

- 使用 - o 参数只显示 IO 操作进程：

```
[root@root ~]# iotop -o
```

- 使用 - b 参数批量显示，无交互：

```
[root@root ~]# iotop -b
```

- 使用 - u 参数显示 root 用户的 IO 进程：

```
[root@root ~]# iotop -u root
```

**字段解析**

![img](https://img-blog.csdnimg.cn/09434ce37988445ca4e47b2c814768ee.png)

- Total DISK READ：从磁盘中读取的总速率
- Total DISK WRITE：往磁盘里写入的总速率
- Actual DISK READ：从磁盘中读取的实际速率
- Actual DISK WRITE：往磁盘里写入的实际速率
- TID：线程 ID，按 p 可转换成进程 ID
- PRIO：优先级
- USER：线程所有者
- DISK READ：从磁盘中读取的速率
- DISK WRITE：往磁盘里写入的速率
- SWAPIN：swap 交换百分比
- IO- :IO 等待所占用的百分比
- COMMAND：具体的进程命令

