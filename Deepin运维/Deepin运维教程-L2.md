---
title: Deepin运维教程-L2
description: 
published: true
date: 2022-06-28T05:59:52.730Z
tags: 运维
editor: markdown
dateCreated: 2022-06-28T05:51:31.455Z
---

# Deepin运维教程-L2
#### 内部命令与外部命令

- 什么是命令：用来实现某一种功能的指令或程序
- 命令的执行依赖于解释器（例如：/bin/bash），/etc/shells文件存放系统可用的shell
  - 用户——解释器（shell外壳）——内核
- [root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# shell 终端 交互接口 用户接口

```shell
#搜索命令所在的绝对路径
[root@localhost ~]# which ls
alias ls='ls --color=auto'
	/usr/bin/ls

root@localhost ~]# ls /usr/bin/ls
/usr/bin/ls

#直接运行程序文件
[root@localhost ~]# /usr/bin/ls
[root@localhost ~]# /usr/bin/ls /opt
hello.hard  hello.soft	t1  test1  test.txt

[root@localhost ~]# which cat
[root@localhost ~]# ls /usr/bin/cat
/usr/bin/cat

[root@localhost ~]# which pwd
/usr/bin/pwd
```

#### Linux命令的分类

- 内部命令：shell程序自带的基本管理命令
- 外部命令：有独立的外部可执行程序文件命令
- type 用于区别内部命令与外部命令
- which 用于查找可以执行程序文件位置

```shell
[root@localhost opt]# type ls

[root@localhost opt]# type cat

[root@localhost opt]# type hash

[root@localhost ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin

[root@localhost ~]# hash
命中	命令
   1	/usr/bin/cat
   1	/usr/bin/ls


[root@localhost opt]# hash -r
[root@localhost opt]# 
[root@localhost opt]# hash
hash: 哈希表为空

[root@localhost opt]# ls
hello.hard  hello.soft  t1  test1  test.txt
[root@localhost opt]# hash
命中	命令
   1	/usr/sbin/ls
```

- 总结：
  - shell程序是用户和系统之间的接口，用于解释用户的命令
  - 查找命令对应的程序文件所在位置：which 命令
  - shell程序大多数存放在/etc/shells文件中
  - 系统默认使用的shell为/bin/bash
  - 查看当前使用的shell：echo $SHELL
  - 区别内部命令与外部命令的方式：typt 命令
  - shell程序查找可执行程序文件路径定义在$PATH环境变量中
  - shell查找的外部命令路径结果会记录到缓存的hash表中

#### help 命令帮助手册

- help命令用于查看shell内部命令的帮助信息，包括使用方法、选项等…
- 命令格式：help [选项] 命令

```shell
#获取内部命令帮助信息
[root@localhost etc]# help cd

#help无法获取外部命令的帮助信息
root@localhost etc]# help ls
bash: help: 没有与 `ls' 匹配的帮助主题。尝试 `help help' 或者 `man -k ls' 或者 `info ls'。

[root@localhost etc]# type help
help 是 shell 内嵌

#获取help命令本身的帮助信息
[root@localhost etc]# help help

[root@localhost etc]# type cat
cat 是 /usr/bin/cat

[root@localhost etc]# help cat
bash: help: 没有与 `cat' 匹配的帮助主题。尝试 `help help' 或者 `man -k cat' 或者 `info cat'。

#查看命令帮助手册（命令自带）
[root@localhost etc]# cat --help
[root@localhost etc]# ls --help
```

#### man 获取命令帮助手册

- man 命令用于查看系统命令的帮助信息，包括使用方法、选项、使用例子等…，对比--help ，mna 输出的信息更加详细
- 命令格式：man [-选项] 命令
- 常用快捷操作
  - 向下键向下移一行
  - 向上键向上移一行
  - [Page Down] 向下翻一页
  - [Page Up] 向上翻一页
  - /关键字 #搜索关键字，配合n（向下查询）、N（向上查询）
  - q 退出

```shell
[root@localhost etc]# man ls
[root@localhost etc]# man cat
[root@localhost etc]# man touch
[root@localhost etc]# man mkdir
[root@localhost etc]# info ls
```

Google/百度（Google，算法）

#### Linux系统的运行级别

Linux系统运行级别：linux系统有7个运行级别，不同的运行级别运行的程序和功能都不一样，而Linux系统默认是运行在一个标准的级别上，系统运行级别文件/etc/inittab文件

运行级别 0：所有进程被终止，机器将有序的停止，关机时系统处于这个运行级别（关机）

运行级别 1：单用户模式，（root用户进行系统维护），系统里运行的所有服务也都不会启动

运行级别 2：多用户模式（网络文件系统NFS服务没有被启动）

运行级别 3：完全多用户模式，（有NFS网络文件系统）标准的运行级别

运行级别 4：系统未使用

运行级别 5：登录后，进入带GUI的图形化界面，标准的运行级别

运行级别 6：系统正常关闭并重启

```shell
#查看当前系统运行级别
[root@localhost etc]# runlevel
N 5
#解释；当前系统处于的运行级别
#解释：N代表没有从任何级别跳转过来

#切换系统运行级别
[root@localhost ~]# init N

#查看运行级别文件内容
[root@localhost ~]# cat /etc/inittab 
# inittab is no longer used when using systemd.
#
# ADDING CONFIGURATION HERE WILL HAVE NO EFFECT ON YOUR SYSTEM.
#
# Ctrl-Alt-Delete is handled by /usr/lib/systemd/system/ctrl-alt-del.target
#
# systemd uses 'targets' instead of runlevels. By default, there are two main targets:
#
# multi-user.target: analogous to runlevel 3   #运行级别3
# graphical.target: analogous to runlevel 5    #运行级别5
#
# To view current default target, run:
# systemctl get-default    				 #查看当前系统默认的运行级别
#
# To set a default target, run:
# systemctl set-default TARGET.target    #修改当前系统默认运行级别  

#查看默认运行级别
[root@localhost ~]# systemctl get-default
graphical.target  #默认运行级别为5

 #修改默认运行级别为3
[root@localhost ~]# systemctl set-default multi-user.target 
[root@localhost ~]# systemctl get-default
multi-user.target

#修改默认运行级别为5
[root@localhost ~]# systemctl set-default graphical.target
[root@localhost ~]# systemctl get-default
graphical.target
```

#### 关机与重启

- linux下常用的关机命令有：shutdown、halt、poweroff、init
  - init 0 关机
  - halt #立刻关机
  - poweroff #立刻关机 （记这个）
  - shutdown –h now #立刻关机
  - shutdown -h 10 #10分钟后自动关机

```shell
[root@localhost ~]# poweroff
```

- 重启命令：reboot shutdown
  - reboot #立刻重启 （记这个）
  - shutdown -r now #立刻重启
  - shutdown -r 10 #过十分钟后重启

```shell
[root@localhost ~]# reboot
```

#### 课后作业

1.请在/tmp目录下创建student目录，并在student目录下同时创建t1、t2、t3文件

mkdir /tmp/student

cd /tmp/student/

touch t1 t2 t3

touch /tmp/student/t1 /tmp/student/t2 /tmp/student/t3

2.请在/tmp目录下递归创建test1/test2/test3目录

mkdir -p /tmp/test1/test2/test3

3.切换到/tmp/test1/test2/test3目录下，并打印(查看)当前所在目录

cd /tmp/test1/test2/test3

pwd

4.请同时在/opt、/media目录下创建upload文件

touch /opt/upload /media/upload

5.请将/opt目录下的upload文件移动至/tmp/test1/test2/test3目录下，并改名为upload.bak

mv /opt/upload /tmp/test/1/test/2/test3/upload.bak

6.请将/etc/passwd文件拷贝至/opt目录下，改名为passwd.bak，并保持属性不变

cp -p /etc/passwd /opt/passwd.bak

7.请将/etc/fstab文件拷贝至/opt目录下，并改名为fstab.bak

cp -p /etc/fstab /opt/fstab.bak

8.请将/etc/sysconfig/network-scripts/ifcfg-ens32 文件拷贝至/opt目录下，并改名为ens32.bak

cp /etc/sysconfig/network-scripts/ifcfg-ens32 /opt/ens32.bak

9.请删除/etc/yum.repos.d/目录下所有内容

rm -rf /etc/yum.repos.d/*

10.请在/etc/yum.repos.d/目录下创建local.repo文件

touch /etc/yum.repos.d/local.repo

11.请查看/etc/sysconfig/network-scripts/ifcfg-ens32文件末尾5行内容

tail -5 /etc/sysconfig/network-scripts/ifcfg-ens32

tail -n 5 /etc/sysconfig/network-scripts/ifcfg-ens32

12.请查看/etc/passwd文件第1行内容

head -n 1 /etc/passwd

head -1 /etc/passwd

13.请查看/etc/hostname文件内容

cat /etc/hostname

14.请查看/etc/hosts文件内容

cat /etc/hosts

15.请说出软连接与硬连接的特点

软连接：可以跨分区，可以对目录链接，源文件删除后链接文件不可用

硬连接：不可以跨分区，不可以对目录进行连接，源文件删除后，链接文件以然可用

16.请在/opt目录下创建hello.soft文件，并创建软连接到/tmp目录下

touch /opt/hello.soft

ln -s /opt/hello.soft /tmp

17.请在/opt目录下创建hello.hard文件，并创建硬连接到/tmp目录下，并查看连接文件详细属性

touch /opt/hello.hard

ln /opt/hello.hard /tmp

18.如何获取ls命令的帮助信息？

man ls

ls --help

19.请说出Linux系统的运行级别

0：关机

1：单用户模式

2：多用户模式（没有NFS）

3：完全多用户模式，标准运行级别

4：保留

5：带GUI图形化界面，标准的运行级别

6：系统关闭并重启

20.如何重启Linux系统？

reboot

init 6

#### 计算机硬件组成部分

- 输入设备：键盘、鼠标、触控屏等

- 主机设备：主板、中央处理器（CPU）、主存储器（内存）、网卡、声卡、显示卡等

- 输出设备：屏幕、耳机、打印机、投影仪等

- 外部存储设备：硬盘、软盘、光盘、U盘等、蓝光光驱

- 硬盘：传统硬盘(HDD)==固态硬盘(SSD)

- CPU缓存

- CPU比较主流的厂商

  - AMD公司
  - Interl公司

- CPU架构

  - x86架构，8086架构，80286，80386，x86称号
  - 8位、16位、32位、64位，CPU一次可以处理的数据量，
  - 32位CPU一次可以从内存中读取大约3.25G左右的数据量
  - 64位CPU一次可以从内存中读取大约128G左右的数据量

- CPU核心

  - 单核心，一颗CPU只能有一个运算单元
  - 多核心，一颗CPU里边有两个以上的运算单元

- 计算机什么最重要？想要让计算机运行起来，足够的电力

  ![1616909159062](https://hu60.cn/q.php/link.img.html?url64=QzpcVXNlcnNcemhpeV9cQXBwRGF0YVxSb2FtaW5nXFR5cG9yYVx0eXBvcmEtdXNlci1pbWFnZXNcMTYxNjkwOTE1OTA2Mi5wbmc.)

#### Linux系统目录介绍

- /（根）:系统所有数据都存放在根目录下
- /bin：存放用户和管理员必备的可执行的二进制程序文件
- /boot：存放Linux系统内核及引导系统程序所需要的文件目录
- /dev：存放硬件设备的目录，如键盘、鼠标、硬盘、光盘等等
- /etc：存放服务的配置文件，用户信息文件
- /root：超级管理员的家目录
- /home：系统普通用户的家目录
- /lib：存放系统中的程序运行所需要的共享库及内核模块
- /opt：额外安装的可选应用程序包所放置的位置
- /srv：服务启动之后需要访问的数据目录
- /tmp：一般用户或正在执行的程序临时存放文件的目录,任何人都可以访问,重要数据不可放置在此目录下
- /var：存放系统执行过程中经常变化的文件，如随时都在变化的日志文件就存放/var/log/下
- /mnt、/media ：光盘和镜像等预设的挂载点
- /proc：Linux伪文件系统，该目录下的数据存在于内存当中，不占用磁盘空间
- /lib64 ：存放函式库
- /run ：程序或服务启动后，存放PID的目录
- /sys：存放被建立在内存中的虚拟文件系统
- /usr：操作系统软件资源所放置的目录
  - /usr/bin：与/bin目录相同，存放用户可以使用的命令程序
  - /usr/lib：与/lib目录相同，存放系统中的程序运行所需要的共享库及内核模块
  - /usr/etc：用于存放安装软件时使用的配置文件
  - /usr/games：与游戏比较相关的数据放置处
  - /usr/include：c/c++等程序语言的档头(header)与包含档(include)放置处
  - /usr/lib64：与/lib64目录相同，存放函式库
  - /usr/libexec：不经常被使用的执行程序或脚本会放置在此目录中
  - /usr/local： 额外安装的软件存放目录
  - /usr/sbin：该目录与/sbin目录相同，存放用户可执行的二进制程序文件
  - /usr/share： 放置只读架构的杂项数据文件
  - /usr/src：一般软件源代码建议存放该目录下

#### 查看内核信息

- uname 命令用于显示系统内核信息
- 命令格式：uname [-选项...]
- 常用选项：
  - -s ：显示内核名称
  - -r ：显示内核版本

```shell
[root@localhost ~]# uname
Linux

[root@localhost ~]# uname -rs
Linux 3.10.0-957.el7.x86_64
#解释：
Linux 	#内核名称
3		#主版本
10		#次版本
0		#修改版本
957		#补丁次数
el7		#Enterprise Linux（企业版Linux）
x86_64	#CPU架构

#Linux内核官网
https://www.kernel.org/
```

#### 查看CPU信息

- /proc/cpuinfo文件用于存放系统CPU信息
- lscpu 用于显示CPU架构信息
- 命令格式：lscpu [-选项]

```shell
#查看/proc/cpuinfo文件内容
[root@localhost ~]# cat /proc/cpuinfo 
processor　：#系统中逻辑处理核的编号。对于单核处理器，则可认为是其CPU编号，对于多核处理器则可以是物理核、或者使用超线程技术虚拟的逻辑核
vendor_id　：   #CPU制造商     
cpu family　：  #CPU产品系列代号
model　　　：    #CPU属于其系列中的哪一代的代号
model name：    #CPU属于的名字及其编号、标称主频
stepping　  ：   #CPU属于制作更新版本
cpu MHz　  ：    #CPU的实际使用主频
cache size   ：  #CPU二级缓存大小
physical id   ： #单个CPU的标号
siblings       ：#单个CPU逻辑物理核数
core id        ：#当前物理核在其所处CPU中的编号，这个编号不一定连续
cpu cores    ：  #该逻辑核所处CPU的物理核数
apicid          ：#用来区分不同逻辑核的编号，系统中每个逻辑核的此编号必然不同，此编号不一定连续
fpu             ： #是否具有浮点运算单元（Floating Point Unit）
fpu_exception  ：  #是否支持浮点计算异常
cpuid level   ：   #执行cpuid指令前，eax寄存器中的值，根据不同的值cpuid指令会返回不同的内容
wp             ：  #表明当前CPU是否在内核态支持对用户空间的写保护（Write Protection）
flags          ：   #当前CPU支持的功能
bogomips   ：       #在系统内核启动时粗略测算的CPU速度（Million Instructions Per Second）
clflush size  ：    #每次刷新缓存的大小单位
cache_alignment ：  #缓存地址对齐单位
address sizes     ：#可访问地址空间位数
power management ： #对能源管理的支持，有以下几个可选支持功能：
#使用lscpu查看cpu信息
[root@localhost ~]# lscpu
　Architecture: #架构 
　　CPU(s): #逻辑cpu颗数 
　　Thread(s) per core: #每个核心线程 
　　Core(s) per socket: #每个cpu插槽核数/每颗物理cpu核数 
　　CPU socket(s): #cpu插槽数 
　　Vendor ID: #cpu厂商ID 
　　CPU family: #cpu系列 
　　Model: #型号 
　　Stepping: #步进 
　　CPU MHz: #cpu主频 
　　Virtualization: #cpu支持的虚拟化技术 
　　L1d cache: #一级缓存（google了下，这具体表示表示cpu的L1数据缓存） 
　　L1i cache: #一级缓存（具体为L1指令缓存） 
　　L2 cache: #二级缓存
```

#### 查看系统内存信息

- /proc/meminfo文件用于存放系统内存信息
- free 用于查看内存使用情况
- 命令格式：free [-选项]
- 常用选项：-h #以人类易读方式显示大小（KB，MB，GB）

```shell
#查看/proc/meminfo文件内容
[root@localhost ~]# cat /proc/meminfo 
MemTotal:          995896 kB    #所有可用的内存大小，物理内存减去预留位和内核使用。系统从加电开始到引导完成，firmware/BIOS要预留一些内存，内核本身要占用一些内存，最后剩下可供内核支配的内存就是MemTotal。这个值在系统运行期间一般是固定不变的，重启会改变。
MemFree:            244196 kB   #表示系统尚未使用的内存。
MemAvailable:       435080 kB   #真正的系统可用内存，系统中有些内存虽然已被使用但是可以回收的，比如cache/buffer、slab都有一部分可以回收，所以这部分可回收的内存加上MemFree才是系统可用的内存
Buffers:             2132 kB   #用来给块设备做缓存的内存，(文件系统的 metadata、pages)
Cached:             314632 kB  #分配给文件缓冲区的内存,例如vi一个文件，就会将未保存的内容写到该缓冲区
SwapCached:            0 kB    #被高速缓冲存储用的交换空间（硬盘的swap）的大小
Active:            295908 kB    #经常使用的高速缓冲存储器页面文件大小
Inactive:          271552 kB    #不经常使用的高速缓冲存储器文件大小
Active(anon):      251528 kB    #活跃的匿名内存
Inactive(anon):     13044 kB    #不活跃的匿名内存
Active(file):       44380 kB    #活跃的文件使用内存
Inactive(file):    258508 kB   #不活跃的文件使用内存
Unevictable:           0 kB    #不能被释放的内存页
Mlocked:               0 kB    #系统调用 mlock 家族允许程序在物理内存上锁住它的部分或全部地址空间。这将阻止Linux 将这个内存页调度到交换空间（swap space），即使该程序已有一段时间没有访问这段空间
SwapTotal:             0 kB    #交换空间总内存
SwapFree:              0 kB    #交换空间空闲内存
Dirty:                 4 kB    #等待被写回到磁盘的
Writeback:             0 kB    #正在被写回的
AnonPages:         15100 kB    #未映射页的内存/映射到用户空间的非文件页表大小
Mapped:             7160 kB    #映射文件内存
Shmem:               100 kB    #已经被分配的共享内存
Slab:               9236 kB    #内核数据结构缓存
SReclaimable:       2316 kB    #可收回slab内存
SUnreclaim:         6920 kB    #不可收回slab内存
KernelStack:        2408 kB    #内核消耗的内存
PageTables:         1268 kB    #管理内存分页的索引表的大小
NFS_Unstable:          0 kB    #不稳定页表的大小
Bounce:                0 kB    #在低端内存中分配一个临时buffer作为跳转，把位于高端内存的缓存数据复制到此处消耗的内存
WritebackTmp:          0 kB    #FUSE用于临时写回缓冲区的内存
CommitLimit:       22980 kB    #系统实际可分配内存
Committed_AS:     536244 kB    #系统当前已分配的内存
VmallocTotal:     892928 kB    #预留的虚拟内存总量
VmallocUsed:       29064 kB    #已经被使用的虚拟内存
VmallocChunk:     860156 kB    #可分配的最大的逻辑连续的虚拟内存
#使用free命令查看内存使用情况
[root@localhost ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           972M        344M        238M         13M        389M        424M
Swap:          2.0G          0B        2.0G
#解释：Mem 物理内存统计信息
total：		#物理内存总量
used：		#以使用的内存总量
free：		#空闲内存总量
shared：		#共享内存总量
buff/cache：	#块设备与普通文件占用的缓存数量
available：	#还可以被应用程序使用的物理内存大小

#解释：Swap 内存交换空间，当物理内存不足时，可以使用硬盘空间充当内存使用
total：		#交换分区内存总量
used：		#正在使用的交换分区内存
free：		#空闲交换分区内存
```

#### 查看网卡信息

- 网卡配置文件地址： /etc/sysconfig/network-scripts/网卡名
- ifconfig 用于显示和设置网卡的参数
- 命令格式： ifconfig [网卡名]

```shell
[root@localhost ~]# cat /etc/sysconfig/network-scripts/ifcfg-ens32
TYPE=“Ethernet“			#网卡类型=以太 ※
PROXY_METHOD=“none“		#代理方式=关闭
BROWSER_ONLY="no“		#只是浏览器=否
BOOTPROTO=“none“		#获取IP地址的方式=固定IP ※
DEFROUTE=“yes“			#是否设置默认路由=是
IPV4_FAILURE_FATAL=“no“	#是否开启ipv4致命检测=否（如果ipv4配置失败禁用设备）
NAME=“ens32“			#物理网卡设备名字 ※
UUID=“3ef0d258-f9a4-49e5-a9da-7b47bc98daa0	“#网卡UUID
DEVICE=“ens32“			#网卡名字 ※
ONBOOT=“yes“			#开机或重启时是否启动网卡  ※
IPADDR=“192.168.0.210“	#IP地址  ※
PREFIX=“24“				#子网掩码 ※
GATEWAY=“192.168.0.254“	#网关	※
DNS1=“8.8.8.8“			#dns服务器IP地址 ※
DNS2=8.8.4.4			#备用dns服务器IP地址 ※
#使用ifconfig命令查看网卡信息
[root@localhost ~]# ifconfig
ens32: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.29  netmask 255.255.255.0  broadcast 192.168.0.255
        inet6 fe80::8d50:c4d5:97b0:9d64  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:b0:cf:c8  txqueuelen 1000  (Ethernet)
        RX packets 3948  bytes 1811465 (1.7 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2538  bytes 459113 (448.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
#解释：
ens32:		 #网卡名称  ※
flags=4163：	#标志
UP：			#网卡处于活跃状态  ※
BROADCAST：	#支持广播
RUNNING：	#网线已接入
MULTICAST：	#支持组播
mtu 1500：	#最大传输单元（字节），表示此网卡一次能传输的最大数据包 ※
inet 192.168.0.29		#IPV4地址 ※
netmask 255.255.255.0	#子网掩码 ※
broadcast 192.168.0.255	#广播地址 ※
inet6 fe80::8d50:c4d5:97b0:9d64		#IPV6地址
prefixlen 64  scopeid 0x20<link>	#前缀 64 作用域 0x20
ether 00:0c:29:b0:cf:c8	#网卡MAC地址 ※
xqueuelen 1000			#网卡设置的传送队列长度
(Ethernet)				#网卡连接类型
RX packets 3948			#接收正确的数据包 ※
bytes 1811465 (1.7 MiB)	#接收的数据量与字节 ※
RX errors 0  dropped 0  overruns 0  frame 0 #接收到的错误包、丢弃的数据包数、由于速度过快而丢失的数据包、发生frame错误而丢失的数据包数 ※
TX packets 100 		#发送的正确的数据包数 ※
bytes 8116 (7.9 KiB)#发送的数据量、字节  ※
TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0 #发送时产生错误的数据包数、丢弃的数据包数、由于速度过快而丢失的数据包数、发生carrier错误而丢失的数据包数、冲突信息包的数目 ※

#只查看指定的网卡
[root@localhost ~]# ifconfig ens32


lo: 本地回环网卡，不是物理网卡，通过软件虚拟出来的一个网卡，127.0.0.1，用于测试本机的联通性
[root@localhost ~]# ping 127.0.0.1

virbr0: 虚拟化的网络接口，通过软件技术虚拟出来的一个网卡，192.168.122.1，KVM虚拟化技术的时候
```

#### 查看主机名及修改主机名

- /etc/hostname文件用于存放主机名
- hostname 命令用于显示和设置主机名
- 命令格式：hostname [-选项] [新名称]

```shell
#查看主机名
[root@localhost ~]# hostname
localhost.localdomain

#查看主机名配置文件
[root@localhost ~]# cat /etc/hostname 
localhost.localdomain

#临时修改主机名（立刻生效，服务器重启以后失效）
[root@localhost ~]# hostname test
[root@localhost ~]# hostname
test

#exit/loguot登出系统
[root@localhost ~]# exit
[c:\~]$ ssh 192.168.0.50
[root@test ~]# 

[root@test ~]# hostname fhsd.jhglshdjkghjkdfhgkjhgdsahgjklhdsfjghsdhgjlhsd
[root@test ~]# logout

[root@fhsd ~]# hostname sdhjghsdfjkhgkjdfshkgljhsdjfhgjksdhgjkhsdjgjkl
[root@fhsd ~]# exit

#命令行永久修改主机名（立刻生效，不需要重启系统）
[root@localhost ~]# hostnamectl set-hostname test
[root@localhost ~]# exit
```
