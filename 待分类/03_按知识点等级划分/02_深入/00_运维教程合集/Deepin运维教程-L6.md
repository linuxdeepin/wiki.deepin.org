---
title: Deepin运维教程-L6
description: 
published: true
date: 2022-10-18T03:30:13.278Z
tags: 运维
editor: markdown
dateCreated: 2022-06-28T05:59:06.814Z
---

# Deepin运维教程-L6
#### 删除逻辑卷

- 逻辑卷的删除不允许联机操作，需要先卸载，在执行删除
- 在执行删除操作时，首先删除LV逻辑卷，在删除VG卷组，最后删除PV物理卷
- 删除命令：lvremove

```shell
#删除逻辑卷错误示范
[root@localhost ~]# lvremove /dev/systemvg/mylv 
  Logical volume systemvg/mylv contains a filesystem in use.  #提示文件正在使用中

#需要先卸载
[root@localhost ~]# umount /dblod/

#删除逻辑卷
[root@localhost ~]# lvremove /dev/systemvg/mylv 
Do you really want to remove active logical volume systemvg/mylv? [y/n]: y
  Logical volume "mylv" successfully removed

#删除卷组
[root@localhost ~]# vgremove systemvg
  Volume group "systemvg" successfully removed
```

#### 逻辑卷的缩减

- 命令lvreduce
- 不允许连接缩减
- 先缩减文件系统的空间，在缩减逻辑卷的空间

#### RAID磁盘阵列

- RAID中文全称：独立磁盘冗余阵列 ，简称磁盘阵列
- RAID可通过技术（软件/硬件）将多个独立的磁盘整合成一个巨大容量大逻辑磁盘使用
- RAID可以提高数据I/O（读写）速度，和冗余数据的功能

#### RAID级别

RAID0：等量存储，至少由2块磁盘组成，同一个文档等量存放在不同的磁盘并行写入数据来提高效率，但只是单纯的提高效率，并没有冗余功能，如果其中一块盘故障，数据会丢失，不适合存放重要数据



RAID1：完整备份，至少由两块磁组成，同一个文档复制成多份存储到不同磁盘提高可靠性，读写速度没有提升，适合存储重要的数据



RAID2：至少由3块磁盘组成，数据分散存储在不同磁盘，在读写数据时需要对数据时时校验，由于采用的校验算法复杂，数据量比原有数据增大，而且导致硬件开销较大



RAID3：至少由三块磁盘组成，同一份文档分散写入不同的磁盘，校验数据单独存放在另外一块磁盘，由于每次读写操作都会访问校验盘，容易导致校验盘长时间高负荷工作而挂掉，如果校验盘损坏数据将无法恢复



RAID4：与RAID3类似，至少由3块磁盘组成，同一份文档分散存写入不同磁盘，校验数据单独存放在另外一块磁盘，由于每次读写操作都会访问校验盘，容易导致校验盘长时间高负荷工作而挂掉，如果校验盘损坏数据将无法恢复，与RAID3的区别是数据分割方式不一样



RAID5：至少由3块磁盘组成，同一份文档分散写入不同磁盘，每个硬盘都有校验数据，其中校验数据会占用磁盘三分之一的空间，三分之二的空间存放原始数据，允许同时坏一块磁盘，当一块磁盘损坏，其他磁盘里的数据配合校验信息可将数据恢复回来





RAID6：至少由4块磁盘组成，同一份文档分散写入不同磁盘，每个磁盘都有校验数据，由于采用双校验算法，所以校验数据量是RAID5的两倍，需要占用2块磁盘空间存放校验数据，两块盘存放原始数据，由于数据校验的算法计算量偏大，所以在速写速度上没有RAID5快，允许同时坏2块磁盘





RAID7：美国SCC公司专利，花钱

RAID10：RAID10=RAID1+RAID0合二为一，最少需要4块磁盘，先将4块硬盘组成两组RAID1，在将两组RAID1组成一个RAID0，既提高数据读写速度，又能保障数据安全性，缺点是可用容量是总容量的一半



#### 实现RAID方式

- 实现RAID通常有三种方式，通过软件技术实现RAID功能（软RAID）
- 外接式磁盘阵列柜，被常用在大型服务器上，不过这类产品价格昂贵
- RAID磁盘阵列卡，分为服务器自带和额外安装，硬RAID比软RAID更安全稳定，RAID卡带有缓存功能可实现数据自动恢复，RAID卡有电池




- 配置硬RAID方式







#### 进程管理

- 什么是程序：用计算机语言编写的命令序列集合，用来实现特定的目标或解决特定的问题，程序占用磁盘空间，程序是静态并且是永久的
- 什么是进程：正在运行中的程序叫进程，占用内存空间，进程是动态的，进程是有生命周期的，进程有自己的独立内存空间，每启动一个进程，系统就会为它分配内存空间并分配一个PID号，每个进程都会对应一个父进程，而父进程可以复制多个子进程，每种进程都有两种方式存在，前台与后台，一般进程都是以后台方式运
- 什么是线程：线程也被称为轻量级进程，被包含在进程中，是进程的一个子集，是进程中的实际运作单位，一个进程中可以并发多个线程，每条线程并行执行不同的任务，每个线程都是独立的，线程之间共享进程的内存空间，在多线程的程序中，由于线程很“轻”，故线程的切换非常迅速且开销小（在同一进程中）

#### 查看进程树

- pstree以树状结构显示进程信息，包括进程之间的关系
- 命令格式：pstree [选项...] [参数...]
- 常用选项：
  - -p #显示进程PID
  - -a #显示完整的命令行
  - -u #列出每个进程所属账号名称

```shell
#查看进程树
[root@localhost ~]# pstree
systemd─┬─ModemManager───2*[{ModemManager}]
CentOS7版本：天父进程systemd
CentOS6版本：天父进程init，Apstart
CentOS5版本：天赋进程init

#以PID形式显示进程信息
[root@localhost ~]# pstree -p
systemd(1)─┬─ModemManager(6714)─┬─{ModemManager}(6739)

#查看系统用户的进程信息
[root@localhost ~]# pstree -p lisi
sshd(15086)───bash(15089)───vim(15244)
[root@localhost ~]# pstree -pa lisi
sshd,15086    
  └─bash,15089
      └─vim,15244 1.txt

#查看系统所有用户的进程
root@localhost ~]# pstree -up
...
           ├─smartd(6726)
           ├─sshd(7337)─┬─sshd(8880)───bash(8887)───pstree(15395)
           │            └─sshd(15066)───sshd(15086,lisi)───bash(15089)───vim(15244)
```

- ps aux：unix格式静态查看系统进程，查看系统所有进程信息
  - a #显示当前终端所有进程
  - u #以用户格式输出
  - x #当前用户在所有终端下的进程
- ps -ef：Linux格式静态查看系统进程，查看系统所有进程信息
  - -e #显示系统所有进程
  - -l #以长格式输出信息
  - -f #显示最完整的进程信息

```shell
#查看系统所有进程信息
[root@localhost ~]# ps aux
USER PID %CPU %MEM VSZ   RSS TTY STAT START TIME COMMAND
root  1  2.2  0.3 127992 6576 ?  Ss   09:08 0:01 /usr/lib/systemd/systemd --switched-root
#个字段含义如下：
user：进程属于那个用户
PID ：进程PID号
%CPU：进程占用CPU资源百分比
%MEM：进程占用物理内存百分比
VSZ ：进程使用掉的虚拟内存量（单位：Kb）
RSS ：进程占用固定内存量（单位：Kb）
TTY ：进程在那个终端运行，如果内核直接调用则显示“？”，tty1-tty6表示本机终端登录的用户进程，pts/0-255则表示远程终端登录用户的进程
STAT：进程状态：R（Running）运行，S（Sleep）休眠，s包含子进程，T（stop）停止，Z（Zombie）僵尸，+后台进程
START：进程启动时间
TIME ：占用CPU运算时间
COMMAND：产生进程的命令

#查看系统所有进程信息
[root@localhost ~]# ps -ef
UID  PID PPID  C STIME TTY    TIME  CMD
root 1      0  0 09:08 ?  00:00:01 /usr/lib/systemd/systemd --switched-root --system --dese
#PPID ：该进程的父进程ID号
```

#### top查看系统健康状态

- top命令动态来查看系统运行性能及状态信息
- 命令格式：top [选项...]
- 常用选项：-d #指定刷新秒数，默认为3秒刷新一次
- 交互界面显示指令：
  - 键盘上下键翻行
  - h #获取交互模式帮助
  - P(大写) #按照CPU使用资源排序
  - M #按照内存使用资源排序
  - q #退出

```shell
[root@localhost ~]# top
top - 21:22:04 up 12:13,  2 users,  load average: 0.00, 0.01, 0.05
Tasks: 115 total,   1 running, 114 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.3 us,  0.3 sy,  0.0 ni, 99.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  1863224 total,  1502920 free,   107872 used,   252432 buff/cache
KiB Swap:  2097148 total,  2097148 free,        0 used.  1565576 avail Mem 
   
   PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND   
   8317 root      20   0  161984   2220   1568 R  0.7  0.1   0:01.62 top   
#第一行top每个字段含义如下：
第二列：21:22:04：当前系统时间
第三列：up 12:13：系统运行时间，该系统以运行12小时13分钟（up 10 day，12:13 代表运行10天12小时13分钟）
第四列：2 users：当前系统登录终端数量
第五列：load average: 0.00, 0.01, 0.05：CPU1分钟，5分钟，15分钟之前平均负载量，根据CPU核数判断系统CPU负载量，1核CPU若高于1代表负载过高，2核CPU若高于2代表负载过高，依次类推。。。

#第二行Tasks每个字段含义如下：
第二列：115 total：当前系统中进程的总数量
第三列：1 running：正在运行的进程数量
第四列：114 sleeping：正在睡眠的进程数量
第五列：0 stopped：正在停止的进程数量
第六列：0 zombie：僵尸进程数量，僵尸进程是当子进程比父进程先结束，而父进程又没有回收子进程，释放子进程占用的资源，此时子进程将成为一个僵尸进程。
#查找僵尸进程与其父进程
ps -A -o stat,ppid,pid,cmd | grep "^Zz"
命令解释：
-A 参数列出所有进程
-o 自定义输出字段，我们设定显示字段为 stat（状态）, ppid（父进程id）, pid(进程id)，cmd（命令）这四个参数，因为状态为 z或者Z的进程为僵尸进程，所以我们使用grep抓取stat状态为zZ进程
#杀死进程
kill -9 + 父进程号

#第三行%Cpu(s)每个字段含义如下
第二列：0.0 us：用户进程占用的CPU百分比
第三列：0.0 sy：系统进程占用的CPU百分比
第四列：0.0 ni：改变过优先级的用户进程占用的CPU百分比
第五列：100.0 id：空闲的CPU百分比（重点关注）
第六列：0.0 wa：等待输入/输出的进程的占用CPU百分比
第七列：0.0 hi：硬中断请求服务占用的CPU百分比
第八列：0.0 si：软中断请求服务占用的CPU百分比
第九列：0.0 st：虚拟时间百分比，当有虚拟机时，虚拟CPU等待实际CPU的时间百分比

#第四行KiB Mem每个字段含义如下：
第二列：1863224 total：物理内存总量，单位KB
第三列：1502516 free： 空闲内存总量，单位KB
第四列：108240 used：  以使用的内存总量，单位KB
第五列：252468 buff/cache：块设备与普通文件占用的缓存数量

#第五行KiB Swap每个字段含义如下：
第二列：2097148 total：交换空间总量，单位KB
第三列：2097148 free：可用空闲交换空间总量，单位KB
第四列：0 used：：以使用的交换空间总量，单位KB
第五列：1565180 avail Mem：可用于进程下一次分配的物理内存数量

#第六行每个字段含义如下：
PID：进程PID号
USER：进程所有者的用户名
PR：进程优先级执行顺序，越小越优先被执行
NI：负值表示高优先级，正值表示低优先级，越小越优先被执行
VIRT：进程使用的虚拟内存总量，单位kb   
RES：进程使用的、未被换出的物理内存大小，单位kb
SHR：共享内存大小，单位kb
S：进程状态。D=不可中断的睡眠状态 R=运行 S=睡眠 T=跟踪/停止 Z=僵尸进程
%CPU：进程使用的CPU百分比（重点关注）
%MEM：进程使用的物理内存百分比（重点关注）
TIME+：进程使用的CPU时间总计，单位1/100秒
COMMAND：命令名/命令行
```

#### 检索进程

- pgrep 通过匹配其程序名，找到匹配的进程
- 命令格式：pgrep [选项...] [参数...]
- 常用选项：
  - -l #输出进程名与PID
  - -U #检索指定用户进程
  - -t #检索指定终端进程
  - -x #精确匹配完整进程名

```shell
#过滤sshd进程
[root@localhost ~]# pgrep sshd
7337
8880
15066
15086
17027

过滤sshd进程，并显示进程名称
[root@localhost ~]# pgrep -l sshd
7337 sshd
8880 sshd
15066 sshd
15086 sshd
17027 sshd

#过滤指定用户的进程
[root@localhost ~]# pgrep -lU lisi
15086 sshd
15089 bash
15244 vim

#按照用户名过滤进程时，选项不要颠倒
[root@localhost ~]# pgrep -Ul lisi
pgrep: invalid user name: l


#查看系统所有终端用户
[root@localhost ~]# who
root     pts/0        2021-04-24 14:06 (192.168.0.1)
lisi     pts/1        2021-04-24 15:57 (192.168.0.1)
root     pts/2        2021-04-24 16:29 (192.168.0.1)

#过滤用户在指定终端开启的进程信息
[root@localhost ~]# pgrep -lU lisi -t pts/1
15089 bash
15244 vim

#过滤用户在指定终端开启的进程信息
[root@localhost ~]# pgrep -lU lisi -t pts/3
19704 bash
19754 top

#精确匹配进程名（没有则不显示）
[root@localhost ~]# pgrep -x ssh

#精确匹配进程名
[root@localhost ~]# pgrep -xl crond
7362 crond
```

#### 进程的前后台调度

- & #将进程放入后台运行
- jobs -l #查看后台进程列表
- fg 进程编号 #将后台进程恢复至前台运行
- ctrl + z 组合键 #挂起当前进程并放入后台
- bg 进程编号 #激活后台被挂起进程

```shell
[root@localhost ~]# sleep 5m &
[1] 20130

#查看后台进程信息
[root@localhost ~]# jobs -l
[1]+ 20130 运行中               sleep 5m &

#将后台进程放入前台运行
[root@localhost ~]# fg 1
sleep 5m

#挂起前台进程放入后台
[root@localhost ~]# jobs
[1]+  已停止               sleep 5m

#激活后台的进程
[root@localhost ~]# bg 1
[1]+ sleep 5m &
[root@localhost ~]# jobs -l
[1]+ 20130 运行中               sleep 5m &
```

#### 杀死进程

- 杀死进程的方式
- ctrl + c 组合键结束当前命令程序
- kill [选项...] PID
  - 常用选项：-l #列出可用进程信号
  - 常用信号：-1重启进程，-9强制杀死进程，-15正常杀死进程（默认信号无需指定）
- killall -9 进程名 #强制杀死进程
- killall -9 -u 用户名 #强制杀死该用户所有进程
- pkill -9 进程名 #强制杀死进程
  - 常用选项：-t 终端号 #提出终端用户

```shell
#结束前台正在运行的进程
[root@localhost ~]# sleep 5m
^C

#启用进程放入后台
[root@localhost ~]# sleep 5m &
[1] 21150
[root@localhost ~]# sleep 6m &
[2] 21155
[root@localhost ~]# sleep 7m &
[3] 21159
[root@localhost ~]# sleep 8m &
[4] 21162

#查看后台进程
[root@localhost ~]# jobs -l
[1]  21150 运行中               sleep 5m &
[2]  21155 运行中               sleep 6m &
[3]- 21159 运行中               sleep 7m &
[4]+ 21162 运行中               sleep 8m &

#杀死后台指定的进程（按照PID）
[root@localhost ~]# kill 21150

#查看后台进程
[root@localhost ~]# jobs -l
[1]  21150 已终止               sleep 5m
[2]  21155 运行中               sleep 6m &
[3]- 21159 运行中               sleep 7m &
[4]+ 21162 运行中               sleep 8m &

[root@localhost ~]# kill 21155
[root@localhost ~]# jobs -l
[2]  21155 已终止               sleep 6m
[3]- 21159 运行中               sleep 7m &
[4]+ 21162 运行中               sleep 8m &

#强制杀死进程
[root@localhost ~]# kill -9 21159
[root@localhost ~]# jobs -l
[3]- 21159 已杀死               sleep 7m
[4]+ 21162 运行中               sleep 8m &

#启动进程
[root@localhost ~]# sleep 4m &
[5] 21402
[root@localhost ~]# sleep 5m &
[6] 21406
[root@localhost ~]# sleep 6m &
[7] 21409
[root@localhost ~]# sleep 7m &
[8] 21412

[root@localhost ~]# jobs -l
[4]  21162 运行中               sleep 8m &
[5]  21402 运行中               sleep 4m &
[6]  21406 运行中               sleep 5m &
[7]- 21409 运行中               sleep 6m &
[8]+ 21412 运行中               sleep 7m &

#按照进程名去杀
[root@localhost ~]# killall sleep
[4]   已终止               sleep 8m
[5]   已终止               sleep 4m
[6]   已终止               sleep 5m
[7]-  已终止               sleep 6m
[8]+  已终止               sleep 7m

#启动进程
[root@localhost ~]# sleep 5m &
[1] 21491
[root@localhost ~]# sleep 6m &
[2] 21495
[root@localhost ~]# sleep 7m &
[3] 21498
[root@localhost ~]# jobs -l
[1]  21491 运行中               sleep 5m &
[2]- 21495 运行中               sleep 6m &
[3]+ 21498 运行中               sleep 7m &

#按照进程名强制杀死进程
[root@localhost ~]# killall -9 sleep
[1]   已杀死               sleep 5m
[2]-  已杀死               sleep 6m
[3]+  已杀死               sleep 7m

[root@localhost ~]# who
root     pts/0        2021-04-24 14:06 (192.168.0.1)
lisi     pts/1        2021-04-24 15:57 (192.168.0.1)
root     pts/2        2021-04-24 16:29 (192.168.0.1)
lisi     pts/3        2021-04-24 17:14 (192.168.0.1)

#杀死指定用户的所有进程（讲用户提出系统）
[root@localhost ~]# killall -9 -u lisi
[root@localhost ~]# who
root     pts/0        2021-04-24 14:06 (192.168.0.1)
root     pts/2        2021-04-24 16:29 (192.168.0.1)

#pkill命令演示
[root@localhost ~]# sleep 4m &
[1] 21870
[root@localhost ~]# sleep 5m &
[2] 21873
[root@localhost ~]# sleep 6m &
[3] 21876

[root@localhost ~]# jobs 
[1]   运行中               sleep 4m &
[2]-  运行中               sleep 5m &
[3]+  运行中               sleep 6m &

[root@localhost ~]# pkill sleep
[1]   已终止               sleep 4m
[2]-  已终止               sleep 5m
[3]+  已终止               sleep 6m

[root@localhost ~]# who
root     pts/0        2021-04-24 14:06 (192.168.0.1)
lisi     pts/1        2021-04-24 17:47 (192.168.0.1)
root     pts/2        2021-04-24 16:29 (192.168.0.1)
lisi     pts/3        2021-04-24 17:48 (192.168.0.1)
[root@localhost ~]# pkill -9 -t pts/3
```

#### 用户登录分析

- users who w #查看以登录的用户信息（详细度不同）
- last #显示登录成功的用户信息
- lastb #显示登录失败的用户信息

```shell
[root@localhost ~]# users
[root@localhost ~]# users
lisi root root
#root：以登录系统的用户名

[root@localhost ~]# who
root     pts/0        2021-04-24 14:06 (192.168.0.1)
lisi     pts/1        2021-04-24 17:47 (192.168.0.1)
root     pts/2        2021-04-24 16:29 (192.168.0.1)
#第一列：以登录系统的用户名
#第二列：用户登录的终端编号
#第三列：登陆时间
#第四列：远程登录地址

[root@localhost ~]# w
 08:16:10 up 55 min,  1 user,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    192.168.0.1      07:21    2.00s  0.10s  0.00s w
#第一行为top命令显示的第一行数据
#第二行每列含义如下：
USER：以登录的用户名
TTY：用户登录终端编号
FROM：登录地址
LOGIN@：登录时间
IDLE：用户空闲时间，这是个计时器，一旦用户执行任何操作，该计时器便会被重置
JCPU：该终端所有进程占用CPU处理时间，包括正在运行和后台作业占用时间。
PCPU：进程执行以后消耗的CPU时间
WHAT：当前正在执行的任务

#显示登录成功用户
[root@localhost ~]# last
lisi     pts/1        192.168.0.1      Sat Apr 24 07:56 - 08:03  (00:07) 
#第一列：用户名
#第二列：用户登录终端编号
#第三列：登录地址
#第四列：登录起使时间
#第五列：登录结束时间
#第六列：登录持续时间

#查看最近2次登录系统成功的用户
[root@localhost ~]# last -2
lisi     pts/3        192.168.0.1      Sat Apr 24 17:48 - 17:50  (00:01)    
lisi     pts/3        192.168.0.1      Sat Apr 24 17:47 - 17:48  (00:00)  

#查看登录失败用户
[root@localhost ~]# lastb
lisi     ssh:notty    192.168.0.1      Sat Apr 24 08:52 - 08:52  (00:00) 
```

#### Linux软件包的分类

- 源码包
- 二进制包（RPM包）

#### 源码包特点

- 源码包缺点：安装过程麻烦，需要用户手动编译，需要手动解决软件包的依赖关系
- 源码包优点：软件源代码开放，允许用户二次开发，安装灵活，可以自定义安装路径与安装功能，卸载方便

#### RPM包特点

- RPM包缺点：所有功能用户无法自定义，安装没有源码包灵活，不可以看到软件源代码
- RPM包优点：由于已经提前被编译过，所以安装简单，安装速度快
- RPM包命名规则，如：vsftpd-3.0.2-25.el7.x86_64.rpm
  - vsftpd #软件包名称
  - 3.0.2 #软件包版本，主版本.次版本.修改版本
  - 25 #补丁次数
  - el7 #适合的系统（el7表示RHEL7）
  - x86_64 #适合的CPU架构
  - rpm #rpm包扩展名

#### RPM管理软件包

- RPM命令管理软件包需要手动解决软件包之间依赖关系
  - 树形依赖：a-->b-->c--d
  - 环形依赖：a-->b-->c--a
  - 模块依赖：需要模块文件支持，模块查询地址：www.rpmfind.net
- 命令格式：rpm 选项... 软件包全名
- 常用选项：
  - -q #仅查询软件是否安装
  - -qa #列出所有已经安装在系统中的所有软件，可配合grep过滤指定的软件包
  - -qi #列出软件包详细信息，包含版本与官网地址
  - -qf #后边接文件名，查询配置文件由哪个软件包产生
  - -ql #列出与该软件包相关所有文件与目录的存放位置
  - -ivh #i安装，v显示详细信息，h显示软件安装进度
  - -Uvh #升级安装软件包
  - -e #卸载软件包
  - --import #导入红帽签名

```shell
#挂载iso镜像文件
[root@localhost ~]# mkdir /mnt/centos
[root@localhost ~]# mount /dev/cdrom  /mnt/centos/

#实现永久挂载，查看iso镜像文件系统类型
[root@localhost ~]# df -hT
[root@localhost ~]# vim /etc/fstab
...
/dev/cdrom /mnt/centos iso9660 defaults 0 0

#重新加载
[root@localhost ~]# mount -a

#查询软件包是否安装
[root@localhost centos]# rpm -q vsftpd
未安装软件包 vsftpd 

#安装vsftpd软件包
[root@localhost centos]# rpm -i Packages/vsftpd-3.0.2-25.el7.x86_64.rpm 

#查询系统中以安装的所有软件
[root@localhost centos]# rpm -qa
[root@localhost centos]# rpm -qa | grep vsftpd
vsftpd-3.0.2-25.el7.x86_64

#查询软件包详细的信息
[root@localhost centos]# rpm -qi vsftpd
Name        : vsftpd    #软件包名
Version     : 3.0.2     #版本
Release     : 25.el7    #最终稳定版
Architecture: x86_64    #适合安装的CPU架构
Install Date: 2021年05月04日 星期二 14时47分06秒   #安装时间
Group       : System Environment/Daemons        #软件包属于哪个群组
Size        : 361335      #软件包大小
License     : GPLv2 with exceptions
Signature   : RSA/SHA256, 2018年11月12日 星期一 22时48分54秒, Key ID 24c6a8a7f4a80eb5
Source RPM  : vsftpd-3.0.2-25.el7.src.rpm
Build Date  : 2018年10月31日 星期三 03时45分10秒
Build Host  : x86-01.bsys.centos.org
Relocations : (not relocatable)
Packager    : CentOS BuildSystem <http://bugs.centos.org>
Vendor      : CentOS
URL         : https://security.appspot.com/vsftpd.html   #软件包官网地址
Summary     : Very Secure Ftp Daemon
Description :      #描述信息
vsftpd is a Very Secure FTP daemon. It was written completely from
scratch.


[root@localhost centos]# which ls
alias ls='ls --color=auto'
	/usr/sbin/ls
	
#查询文件由哪个软件包产生	
[root@localhost centos]# rpm -qf /usr/bin/ls
coreutils-8.22-23.el7.x86_64

[root@localhost centos]# which vim
/usr/bin/vim

[root@localhost centos]# rpm -qf /usr/bin/vim
vim-enhanced-7.4.160-5.el7.x86_64

[root@localhost centos]# rpm -qi vim-enhanced

#查询软件包自带的文件与目录安装路径
[root@localhost centos]# rpm -ql vsftpd

[root@localhost centos]# rpm -qf /usr/bin/vim
vim-enhanced-7.4.160-5.el7.x86_64

[root@localhost centos]# rpm -ql vim-enhanced
/etc/profile.d/vim.csh
/etc/profile.d/vim.sh
/usr/bin/rvim
/usr/bin/vim
/usr/bin/vimdiff
/usr/bin/vimtutor

[root@localhost centos]# rpm -q vsftpd
vsftpd-3.0.2-25.el7.x86_64

#卸载软件包
[root@localhost centos]# rpm -e vsftpd

[root@localhost centos]# rpm -q vsftpd
未安装软件包 vsftpd 

#安装vsftpd软件包
[root@localhost centos]# rpm -ivh Packages/vsftpd-3.0.2-25.el7.x86_64.rpm 
警告：Packages/vsftpd-3.0.2-25.el7.x86_64.rpm: 头V3 RSA/SHA256 Signature, 密钥 ID f4a80eb5: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:vsftpd-3.0.2-25.el7              ################################# [100%]

[root@localhost centos]# rpm -q vsftpd
vsftpd-3.0.2-25.el7.x86_64

#升级安装软件包
[root@localhost centos]# rpm -Uvh Packages/vsftpd-3.0.2-25.el7.x86_64.rpm 
警告：Packages/vsftpd-3.0.2-25.el7.x86_64.rpm: 头V3 RSA/SHA256 Signature, 密钥 ID f4a80eb5: NOKEY
准备中...                          ################################# [100%]
	软件包 vsftpd-3.0.2-25.el7.x86_64 已经安装

#导入红帽签名文件
[root@localhost centos]# rpm --import RPM-GPG-KEY-CentOS-7

#安装软件包，查看是否还有警告信息
[root@localhost centos]# rpm -q vsftpd
vsftpd-3.0.2-25.el7.x86_64
[root@localhost centos]# rpm -e vsftpd
[root@localhost centos]# rpm -ivh Packages/vsftpd-3.0.2-25.el7.x86_64.rpm 
准备中...                          ################################# [100%]
正在升级/安装...
   1:vsftpd-3.0.2-25.el7              ################################# [100%]

#安装httpd软件包
[root@localhost centos]# rpm -ivh Packages/httpd-(tab键)
httpd-2.4.6-88.el7.centos.x86_64.rpm         httpd-manual-2.4.6-88.el7.centos.noarch.rpm
httpd-devel-2.4.6-88.el7.centos.x86_64.rpm   httpd-tools-2.4.6-88.el7.centos.x86_64.rpm
[root@localhost centos]# rpm -ivh Packages/httpd-2.4.6-88.el7.centos.x86_64.rpm 
错误：依赖检测失败：
	/etc/mime.types 被 httpd-2.4.6-88.el7.centos.x86_64 需要
	httpd-tools = 2.4.6-88.el7.centos 被 httpd-2.4.6-88.el7.centos.x86_64 需要
	libapr-1.so.0()(64bit) 被 httpd-2.4.6-88.el7.centos.x86_64 需要
	libaprutil-1.so.0()(64bit) 被 httpd-2.4.6-88.el7.centos.x86_64 需要
[root@localhost centos]# ls /etc/mime.types
ls: 无法访问/etc/mime.types: 没有那个文件或目录

#解决依赖关系
[root@localhost centos]# rpm -ivh Packages/mailcap-2.1.41-2.el7.noarch.rpm 
准备中...                          ################################# [100%]
正在升级/安装...
   1:mailcap-2.1.41-2.el7             ################################# [100%]
[root@localhost centos]# ls /etc/mime.types
/etc/mime.types

#解决依赖关系
[root@localhost centos]# rpm -ivh Packages/httpd-tools-2.4.6-88.el7.centos.x86_64.rpm 
错误：依赖检测失败：
	libapr-1.so.0()(64bit) 被 httpd-tools-2.4.6-88.el7.centos.x86_64 需要
	libaprutil-1.so.0()(64bit) 被 httpd-tools-2.4.6-88.el7.centos.x86_64 需要

#解决依赖关系（www.rpmfind.net官网查询提供libapr-1.so.0模块文件的软件包）
[root@localhost centos]# rpm -ivh Packages/apr-(tab键)
apr-1.4.8-3.el7_4.1.x86_64.rpm         apr-util-1.5.2-6.el7.x86_64.rpm        
apr-devel-1.4.8-3.el7_4.1.x86_64.rpm   apr-util-devel-1.5.2-6.el7.x86_64.rpm  
[root@localhost centos]# rpm -ivh Packages/apr-1.4.8-3.el7_4.1.x86_64.rpm 
准备中...                          ################################# [100%]
正在升级/安装...
   1:apr-1.4.8-3.el7_4.1              ################################# [100%]

#解决依赖关系（www.rpmfind.net官网查询提供libaprutil-1.so.0模块文件的软件包）
[root@localhost centos]# rpm -ivh Packages/apr-util-(tab键)
apr-util-1.5.2-6.el7.x86_64.rpm        apr-util-devel-1.5.2-6.el7.x86_64.rpm  
[root@localhost centos]# rpm -ivh Packages/apr-util-
apr-util-1.5.2-6.el7.x86_64.rpm        apr-util-devel-1.5.2-6.el7.x86_64.rpm  
[root@localhost centos]# rpm -ivh Packages/apr-util-1.5.2-6.el7.x86_64.rpm 
准备中...                          ################################# [100%]
正在升级/安装...
   1:apr-util-1.5.2-6.el7             ################################# [100%]

#安装依赖关系
[root@localhost centos]# rpm -ivh Packages/httpd-tools-2.4.6-88.el7.centos.x86_64.rpm 
准备中...                          ################################# [100%]
正在升级/安装...
   1:httpd-tools-2.4.6-88.el7.centos  ################################# [100%]

#安装httpd主包
[root@localhost centos]# rpm -ivh Packages/httpd-(tab键)
httpd-2.4.6-88.el7.centos.x86_64.rpm         httpd-manual-2.4.6-88.el7.centos.noarch.rpm
httpd-devel-2.4.6-88.el7.centos.x86_64.rpm   httpd-tools-2.4.6-88.el7.centos.x86_64.rpm
[root@localhost centos]# rpm -ivh Packages/httpd-2.4.6-88.el7.centos.x86_64.rpm 
准备中...                          ################################# [100%]
正在升级/安装...
   1:httpd-2.4.6-88.el7.centos        ################################# [100%]
```
