---
title: Linux系统内核概述
description: 
published: true
date: 2023-06-13T01:30:25.110Z
tags: 
editor: markdown
dateCreated: 2023-06-13T01:28:19.881Z
---

> **Linux 内核是一种开源的类 Unix 操作系统宏内核。**

`Linux` 内核是 `Linux` 操作系统的主要组件，也是计算机硬件与其进程之间的核心接口。它负责两者之间的通信，还要尽可能高效地管理资源。之所以称为内核，是因为它在操作系统中就像果实硬壳中的种子一样，并且控制着硬件的所有主要功能。内核的用途主要有以下 `4` 项工作：

- 内存管理：追踪记录有多少内存存储了什么以及存储在哪里
- 进程管理：确定哪些进程可以使用中央处理器、何时使用以及持续多长时间
- 设备驱动程序：充当硬件与进程之间的调解程序/解释程序
- 系统调用和安全防护：从流程接受服务请求

在正确实施的情况下，内核对于用户是不可见的，它在自己的小世界(称为内核空间)中工作，并从中分配内存和跟踪所有内容的存储位置。用户所看到的内容则被称为用户空间。这些应用通过系统调用接口(`SCI`)与内核进行交互。

![2023-6-13_40624.png](/2023-6-13_40624.png)

Linux系统内核概述

## 1. 内核简介

> **单内核体系设计、但充分借鉴了微内核设计体系的优点，为内核引入模块化机制。**

`Linux` 内核的重要组成部分，主要有以下几部分：

- `kernel`

- - 内核核心，一般为 `bzImage`
  - 通常在 `/boot` 目录下，名称为 `vmlinuz-VERSION-RELEASE`

- `kernel object`

- - 内核对象，一般放置于 `/lib/modules/VERSION-RELEASE/`
  - `[ ]` ==> `N` ==> 不编译进内核
  - `[M]` ==> `M` ==> 编译为模块文件
  - `[*]` ==> `Y` ==> 编译进内核

- 辅助文件(`ramdisk`)

- - `initrd`
  - `initramfs`

## 2. 内核模块

### 2.1 uname 命令

**使用格式**

- `uname [OPTION]...`

**参数解释**

- `-n` 显示节点名称
- `-r` 显示`VERSION-RELEASE`
- `-s` 内核名称
- `-v` 内核版本
- `-n` 节点名
- `-m` 硬件名称
- `-i` 硬件平台
- `-p` 处理器类型
- `-o` 操作系统

```
# uname -m
i686

# uname -r
2.6.32-573.22.1.el6.i686

# uname -a
Linux MyServer 2.6.32-573.22.1.el6.i686 ... i686 i386 GNU/Linux
```

### 2.2 lsmod 命令

> **显示由核心已经装载的内核模块**

**命令定义**

- 显示的内容来自于: `/proc/modules` 文件。
- 使用 `lsmod` 命令时，常会采用类似 `lsmod | grep -i ext4` 这样的命令来查询系统是否加载了某些模块。

```
# cat /proc/modules
iptable_filter 2173 0 - Live 0xed9b2000
ip_tables 9567 1 iptable_filter, Live 0xed9a9000
ext3 203718 1 - Live 0xed962000
jbd 65315 1 ext3, Live 0xed904000
xenfs 4360 1 - Live 0xed8e6000
ipv6 271097 14 - Live 0xed88e000
xen_netfront 15871 0 - Live 0xed7d9000
ext4 339812 2 - Live 0xed764000
jbd2 75927 1 ext4, Live 0xed6d9000
mbcache 6017 2 ext3,ext4, Live 0xed6b7000
xen_blkfront 19209 5 - Live 0xed69f000
dm_mirror 11969 0 - Live 0xed68d000
dm_region_hash 9644 1 dm_mirror, Live 0xed67e000
dm_log 8322 2 dm_mirror,dm_region_hash, Live 0xed672000
dm_mod 84711 11 dm_mirror,dm_log, Live 0xed64e000
# lsmod | grep ext4
ext4                  339812  2
jbd2                   75927  1 ext4
mbcache                 6017  2 ext3,ext4
```

**字段含义**

- 第 1 列：表示模块的名称
- 第 2 列：表示模块的大小
- 第 3 列：表示依赖模块的个数
- 第 4 列：表示依赖模块的内容

```
# lsmod
Module                  Size  Used by
iptable_filter          2173  0
ip_tables               9567  1 iptable_filter
ext3                  203718  1
jbd                    65315  1 ext3
xenfs                   4360  1
ipv6                  271097  14
xen_netfront           15871  0
ext4                  339812  2
jbd2                   75927  1 ext4
mbcache                 6017  2 ext3,ext4
xen_blkfront           19209  5
dm_mirror              11969  0
dm_region_hash          9644  1 dm_mirror
dm_log                  8322  2 dm_mirror,dm_region_hash
dm_mod                 84711  11 dm_mirror,dm_log
```

### 2.3 modinfo 命令

> **显示模块的详细描述信息**

**命令定义**

- `modinfo` 列出 `Linux` 内核中命令行指定的模块的信息。
- `modinfo` 能够查询系统中未安装的模块信息。
- 若模块名不是一个文件名，则会在 `/lib/modules/version` 目录中搜索，就像 `modprobe` 一样。
- `modinfo` 默认情况下，为了便于阅读，以下面的格式列出模块的每个属性：`fieldname : value`。

**语法**

- `modinfo [选项] [ modulename|filename... ]`

**选项**

- `-n` 只显示模块文件路径
- `-p` 显示模块参数
- `-a` author
- `-d` description
- `-l` license
- `-0` 使用’\0’字符分隔 field 值，而不是一个新行，对脚本比较有用

**实战演示**

```
# modinfo ext4
filename:       /lib/modules/2.6.32-573.22.1.el6.i686/kernel/fs/ext4/ext4.ko
license:        GPL
description:    Fourth Extended Filesystem
author:         Remy Card, Stephen Tweedie, Andrew Morton, Andreas Dilger, Theodore and others
srcversion:     CB1B990F5A758DFB0FB12F1
depends:        mbcache,jbd2
vermagic:       2.6.32-573.22.1.el6.i686 SMP mod_unload modversions 686

# modinfo btrfs
filename:       /lib/modules/2.6.32-573.22.1.el6.i686/kernel/fs/btrfs/btrfs.ko
license:        GPL
alias:          devname:btrfs-control
alias:          char-major-10-234
srcversion:     B412C18B0F5BF7F1B3C941A
depends:        libcrc32c,zlib_deflate,lzo_compress,lzo_decompress
vermagic:       2.6.32-573.22.1.el6.i686 SMP mod_unload modversions 686
```

### 2.4 modprobe 命令

> **装载或卸载内核模块**

**命令定义**

- 配置文件

- - `/etc/modprobe.conf`
  - `/etc/modprobe.d/*.conf`

- 解决依赖

- - `modprobe`需要一个最新的`modules.dep`文件，可以用`depmod`来生成
  - 该文件列出了每一个模块需要的其他模块，`modprobe`使用这个去自动添加或删除模块的依赖

```
# modules.dep为解决依赖的配置文件，modules.dep.bin二进制文件运行
# ls /lib/modules/2.6.32-358.6.1.el6.i686/
build              modules.block    modules.ieee1394map  modules.ofmap     modules.symbols.bin  weak-updates
extra              modules.ccwmap   modules.inputmap     modules.order     modules.usbmap
kernel             modules.dep      modules.isapnpmap    modules.pcimap    source
modules.alias      modules.dep.bin  modules.modesetting  modules.seriomap  updates
modules.alias.bin  modules.drm      modules.networking   modules.symbols   vdso
```

**语法**

- `modprobe [ -c ]`
- `modprobe [ -l ] [ -t dirname ] [ wildcard ]`
- `modprobe [ -r ] [ -v ] [ -n ] [ -i ] [ modulename … ]`

**选项**

- `-v`

- - 显示程序在干什么，通常在出问题的情况下，`modprobe` 才显示信息

- `-C`

- - 重载，默认配置文件(`/etc/modprobe.conf` 或 `/etc/modprobe.d`)

- `-c`

- - 输出配置文件并退出

- `-n`

- - 可以和 `-v` 选项一起使用，调试非常有用

- `-i`

- - 该选项会使得 `modprobe` 忽略配置文件中的，在命令行上输入的 `install` 和 `remove`

- `-q`

- - 一般 `modprobe` 删除或插入一个模块时，若没有找到会提示错误。使用该选项，会忽略指定的模块，并不提示任何错误信息。

- `-r`

- - 该选项会导致 `modprobe` 去删除，而不是插入一个模块
  - 通常没有没有理由去删除内核模块，除非是一些有 `bug` 的模块

- `-f`

- - 使用该选项是比较危险的
  - 和同时使用 `–force-vermagic`，`–force-modversion` 一样

- `-l`

- - 列出所有模块

- `-a`

- - 插入所有命令行中的模块

- `-t`

- - 强制 `-l` 显示 `dirname` 中的模块

- `-s`

- - 错误信息写入 `syslog`

### 2.5 depmod 命令

> **内核模块依赖关系文件及系统信息映射文件的生成工具**

**语法**

- `depmod [-adeisvV][-m <文件>][--help][模块名称]`

**参数**

- `-a` 分析所有可用的模块
- `-d` 执行排错模式
- `-e` 输出无法参照的符号
- `-i` 不检查符号表的版本
- `-m<文件>` 使用指定的符号表文件
- `-s` 在系统记录中记录错误
- `-v` 执行时显示详细的信息
- `-V` 显示版本信息
- `--help` 显示帮助

### 2.6 insmod 和 rmmod 命令

> 装载或卸载内核模块
>
> - 不解决依赖关系，需要自己手动卸载

**`insmod`命令**

- 向 `Linux` 内核中插入一个模块
- `insmod` 是一个向内核插入模块的小程序
- 大多数用户使用 `modprobe` 因为它比较智能化
- `insmod [ filename ] [ module options... ]`

**`rmmod`命令**

- 命令解析

- - 删除内核中的一模块
  - `rmmod` 是一个可以从内核中删除模块的小程序，大多数用户使用`modprobe -r`去删除模块

- 语法格式

- - `rmmod [ modulename ]`

- 参数选项

- - `-f`

  - - 除非编译内核时 `CONFIG_MODULE_FORCE_UNLOAD` 被设置该命令才有效果，否则没效果
    - 用该选项可以删除正在被使用的模块，设计为不能删除的模块，或者标记为 `unsafe` 的模块

  - `-w`

  - - `rmmod` 拒绝删除正在被使用的模块
    - 使用该选项后，指定的模块会被孤立起来，直到不被使用

  - `-s`

  - - 将错误信息写入 `syslog`，而不是标准错误(`stderr`)
  
## 3. /proc 目录

> **内核把自己内部状态信息及统计信息，以及可配置参数通过 proc 伪文件系统加以输出。**

```
# ls /proc/
1     1173  22     29855  35  47   60   973          filesystems  loadavg       scsi           version
10    12    23     3      36  48   600  buddyinfo    fs           locks         self           vmallocinfo
1071  13    232    30     37  49   61   bus          interrupts   mdstat        slabinfo       vmstat
1082  14    234    31     38  5    62   cgroups      iomem        meminfo       softirqs       xen
1085  15    24     31314  39  528  7    cmdline      ioports      misc          stat           zoneinfo
11    16    25     317    4   531  739  cpuinfo      irq          modules       swaps
1150  17    252    318    40  543  8    crypto       kallsyms     mounts        sys
1162  18    253    32     41  56   808  devices      kcore        mtd           sysrq-trigger
1163  19    26     320    42  566  830  diskstats    keys         net           sysvipc
1165  1908  27     33     43  567  853  dma          key-users    pagetypeinfo  timer_list
1167  2     28     330    44  57   9    driver       kmsg         partitions    timer_stats
1169  20    29     334    45  59   94   execdomains  kpagecount   sched_debug   tty
1171  21    29853  34     46  6    95   fb           kpageflags   schedstat     uptime
```

### 3.1 sysctl 命令

**语法格式**

- `sysctl(选项)(参数)`

**命令参数**

- `-n` 打印值时不打印关键字
- `-e` 忽略未知关键字错误
- `-N` 仅打印名称
- `-w` 当改变 `sysctl` 设置时使用此项
- `-p` 从配置文件 `/etc/sysctl.conf` 加载内核参数设置
- `-a` 打印当前所有可用的内核参数变量和值
- `-A` 以表格方式打印当前所有可用的内核参数变量和值

**默认配置文件**

- `/etc/sysctl.conf`

**命令使用方式**

- (1) 设置某参数

- - `sysctl -w parameter=VALUE`

- (2) 通过读取配置文件设置参数

- - `sysctl -p [/path/to/conf_file]`

**参数说明**

- 只读：输出信息
- 可写：可接受用户指定“新值”来实现对内核某功能或特性的配置`/proc/sys`

**两种修改方式**

- (1) sysctl 命令用于查看或设定此目录中诸多参数

- - `sysctl -w path.to.parameter=VALUE`
  - `sysctl -w kernel.hostname=mail.escapelife.com`

- (2) echo 命令通过重定向的方式也可以修改大多数参数的值

- - `echo "VALUE" > /proc/sys/path/to/parameter`
  - `echo "www.escapelife.com" > /proc/sys/kernel/hostname`

**配置文件中常用的几个参数**

- `net.ipv4.ip_forward`
- `/proc/sys/net/ipv4/ip_forward`
- `vm.drop_caches`
- `/proc/sys/vm/drop_caches`
- `kernel.hostname`
- `/proc/sys/kernel/hostname`

### 3.2 修改配置文件

```
# cat /etc/sysctl.conf
# Kernel sysctl configuration file for Red Hat Linux

# Controls IP packet forwarding
net.ipv4.ip_forward = 0

# Controls source route verification
net.ipv4.conf.default.rp_filter = 1

# Do not accept source routing
net.ipv4.conf.default.accept_source_route = 0

# Controls the System Request debugging functionality of the kernel
kernel.sysrq = 0

# Controls whether core dumps will append the PID to the core filename.
# Useful for debugging multi-threaded applications.
kernel.core_uses_pid = 1

# Controls the use of TCP syncookies
net.ipv4.tcp_syncookies = 1

# Disable netfilter on bridges.
net.bridge.bridge-nf-call-ip6tables = 0
net.bridge.bridge-nf-call-iptables = 0
net.bridge.bridge-nf-call-arptables = 0

# Controls the default maxmimum size of a mesage queue
kernel.msgmnb = 65536

# Controls the maximum size of a message, in bytes
kernel.msgmax = 65536

# Controls the maximum shared segment size, in bytes
kernel.shmmax = 4294967295

# Controls the maximum number of shared memory segments, in pages
kernel.shmall = 268435456
# Auto-enabled by xs-tools:install.sh
net.ipv4.conf.all.arp_notify = 1
```

### 3.3 实战演示

```
# 查看所有可读变量
sysctl -a

# 修改对应参数
sysctl -w kernel.sysrq=0
sysctl -w kernel.core_uses_pid=1
sysctl -w net.ipv4.conf.default.accept_redirects=0

# 如果希望屏蔽别人 ping 你的主机，配置文件修改
net.ipv4.icmp_echo_ignore_all = 1

# 编辑完成后，请执行以下命令使变动立即生效
/sbin/sysctl -p
/sbin/sysctl -w net.ipv4.route.flush=1
```

## 4. /sys 目录

> **sysfs 伪文件系统，输出内核识别出的各硬件设备的相关属性信息，也有内核对硬件特性的设定信息。有些参数是可以修改的，用于调整硬件工作特性。**

### 4.1 udev

- `udev` 是运行用户空间程序。
- `udev` 通 `/sys/` 路径下输出的信息动态为各设备创建所需要设备文件。
- `udev` 是 `Linux` 内核的设备管理器，它取代了 `udevadmin` 和  `hotplug`，负责管理  `/dev` 中的设备节点。
- `udev` 也处理所有用户空间发生的硬件添加、删除事件，以及某些特定设备所需的固件加载。
- `udev` 为设备创建设备文件时，会读取其事先定义好的规则文件，一般在 `/etc/udev/rules.d` 及 `/usr/lib/udev/rules.d` 目录下。

### 4.2 ramdisk 文件的制作

**方法一**

- `mkinitrd` 命令
- 为当前正在使用的内核重新制作 `ramdisk` 文件
- `mkinitrd /boot/initramfs-$(uname -r).img $(uname -r)`

```
# 移动ramdisk文件到/root目录下
mv /boot/initramfs-2.6.32...img /root

# 为当前正在使用的内核重新制作ramdisk文件
mkinitrd /boot/initramfs-$(uname -r).img $(uname -r)
```

**方法二**

- `dracut` 命令
- 为当前正在使用的内核重新制作 `ramdisk` 文件
- `dracut /boot/initramfs-$(uname -r).img $(uname -r)`

```
# 移动ramdisk文件到/root目录下
mv /boot/initramfs-2.6.32...img /root

# 为当前正在使用的内核重新制作ramdisk文件
dracut /boot/initramfs-$(uname -r).img $(uname -r)
```

### 4.3 查看 ramdisk

```
# 使用file命令查看ramdisk文件发现是以gz压缩存放的
file /boot/initramfs-2.6.32-504.el6.x86_64.img

# 改名称，解压
cd /boot/
mv initramfs-2.6.32-504.el6.x86_64.img initramfs-2.6.32-504.el6.x86_64.img.gz
gzip -d initramfs-2.6.32-504.el6.x86_64.img.gz

# 使用file命令查看发现是以cpio存放的文本文件
file initramfs-2.6.32-504.el6.x86_64.img

# 解压这个文本文件
# 之后会在initrd目录下生成相应的文件，一个微型的/root
mkdir initrd
cd initrd
cpio -id < ../initramfs-2.6.32-504.el6.x86_64.img

# 这个时候就可以查看init脚本文件了
cat init

# 在sbin文件中存放着相关的命令
ls sbin
```

## 5. 编译内核

### 5.1 前提准备

**(1) 准备好开发环境**

- 包组(CentOS 6)
- `Server Platform Development`
- `Development Tools`

**(2) 获取目标主机上硬件设备的相关信息**

- CPU

- - `cat /proc/cpuinfo`
  - `x86info -a`
  - `lscpu`

- PCI 设备

- - `lspci`
  - `-v`
  - `-vv`
  - `lsusb`
  - `-v`
  - `-vv`
  - `lsblk`

- 了解全部硬件设备信息

- - `hal-device`

**(3) 获取到目标主机系统功能的相关信息**

**(4) 获取内核源代码包**

- `www.kernel.org`

### 5.2 简易安装内核

**简易安装**

- 获取当前系统的安装文件作为模块安装较为方便
- 修改相应的参数即可
- 只适用于当前特定的内核版本
- 当前系统的安装文件在 `config-2.6.32-504.el6.x86_64`

**简单依据模板文件的制作内核**

```
# 下载对应的Linux内核版本进行解压缩
# 会在/usr/src目录下创建debug、kernels和linux-3.10.67目录
tar xf linux-3.10.67.tar.xz -C /usr/src

# 为了方便多内核共存，使用连接指向
# 会在当前目录下创建一个链接文件 linux -> linux-3.10.67
cd /usr/src
ln -sv linux-3.10.67 linux

# 创建模板
cd linux
# 查看链接指向的文件内容
ls
# 拷贝系统自带的模板文件
cp /boot/config-$(uname -r) .config

# 打开图形界面配置内核选项，选择添加、删除内核模块
# 添加的默认选项来自.config配置文件
make menuconfig

# 使用screen来不中断安装
screen
# 采用几个线程进行编译
make -j n

# 安装内核
make modules_install
# make install中将会安装内容
# 安装bzImage为/boot/vmlinuz-VERSION-RELEASE
# 生成initramfs文件
# 编辑grub的配置文件
make install

# 重启系统，并测试使用新内核，不是默认启动内核
init 6
```


### 5.3 详解编译内核

**(1) 配置内核选项**

- 支持“**更新**”模式进行配置
- (a) `make config`：基于命令行以遍历的方式去配置内核中可配置的每个选项
- (b) `make menuconfig`：基于 `curses` 的文本窗口界面
- (c) `make gconfig`：基于 `GTK` 开发环境的窗口界面
- (d) `make xconfig`：基于 `Qt` 开发环境的窗口界面
- 支持“**全新配置**”模式进行配置
- (a) `make defconfig`：基于内核为目标平台提供的“**默认**”配置进行配置
- (b) `make allnoconfig`: 所有选项均回答为”`no`“

**(2) 编译** - `make [-j #]`

- **如何只编译内核中的一部分功能**

```
# (a)只编译某子目录中的相关代码
cd /usr/src/linux
make dir/

# (b)只编译一个特定的模块
cd /usr/src/linux
make dir/file.ko

# 例如：只为e1000编译驱动
make drivers/net/ethernet/intel/e1000/e1000.ko
```

- **如何交叉编译内核**

```
# 编译的目标平台与当前平台不相同；
make ARCH=arch_name

# 要获取特定目标平台的使用帮助
make ARCH=arch_name help
```

- **如何在已经执行过编译操作的内核源码树做重新编译**

```
# 事先清理操作

# 清理大多数编译生成的文件，但会保留config文件等
make clean

# 清理所有编译生成的文件、config及某些备份文件
make mrproper

# mrproper、patches以及编辑器备份文件
make distclean
```