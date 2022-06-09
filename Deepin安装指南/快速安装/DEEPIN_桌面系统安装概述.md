---
title: Deepin安装概述
description: 
published: true
date: 2022-06-09T01:15:24.932Z
tags: 
editor: markdown
dateCreated: 2022-05-12T05:47:26.110Z
---

# 计算机引导过程

## 传统 BIOS 引导

所谓 BIOS 或 Basic Input-Output System, 就是开机时第一个被执行的程序，又名固件。一般来说它储存在主板上的一块闪存中，与硬盘彼此独立。

BIOS 被启动后，会按启动顺序加载磁盘的前 512 字节，即主引导记录，前 440 字节包含某个启动引导器，像 GRUB、Syslinux 和 LILO 之类的第一启动阶段代码。因为空间太小了，后续的启动代码保存在磁盘上，最后启动引导器又通过「链式引导」，或是直接加载内核，以加载一个操作系统。

整个过程如下：

开机时加电自检。
加电自检后，BIOS 初始化一些必要的硬件以准备引导，比如硬盘和键盘等。
BIOS 执行在「BIOS 硬盘顺序」中的第一块硬盘上的前 440 字节代码，即主引导记录。
MBR 接管后，执行它之后的第二阶段代码，如果后者存在的话，它一般就是启动引导器。
第二阶段代码会读取它的支持文件和配置文件。

## UEFI 引导

UEFI 不仅能读取分区表，还能自动支持文件系统。所以它不像 BIOS，已经没有仅仅 440 字节可执行代码即 MBR 的限制了，它完全用不到 MBR。

UEFI 主流都支持 MBR 和 GPT 分区表。Apple-Intel Macs 上的 EFI 还支持 Apple 专用分区表。绝大部分 UEFI 固件支持软盘上的 FAT12，硬盘上的 FAT16、FAT32 文件系统，以及 CD/DVDs 的 IS09660 和 UDF。Intel Macs 的 EFI 还额外支持 HFS/HFS+ 文件系统。

不管第一块上有没有 MBR，UEFI 都不会执行它。相反，它依赖分区表上的一个特殊分区，叫 EFI 系统分区，里面有 UEFI 所要用到的一些文件。计算机供应商可以在 <EFI 系统分区>/EFI/<VENDOR NAME>/文件夹里放官方指定的文件，还能用固件或它的 shell，即 UEFI shell，来启动引导程序。EFI 系统分区一般被格式化成 FAT32，或比较非主流的 FAT16。

## UEFI 的多重引导

因为每个操作系统或者提供者都可以维护自己的 EFI 系统分区中的文件，同时不影响其他系统，所以 UEFI 的多重启动只是简单的运行不同的UEFI 程序，对应于特定操作系统的引导程序。这避免了依赖 chainloading 机制（通过一个启动引导程序加载另一个引导程序，来切换操作系统）。

UEFI 引导的过程如下：

- 系统开机 - 上电自检（Power On Self Test 或 POST）。
- UEFI 固件被加载，并由它初始化启动要用的硬件。
- 固件读取其引导管理器以确定从何处（比如，从哪个硬盘及分区）加载哪个 UEFI 应用。
- 固件按照引导管理器中的启动项目，加载UEFI 应用。
- 已启动的 UEFI 应用还可以启动其他应用（对应于 UEFI shell 或 rEFInd 之类的引导管理器的情况）或者启动内核及initramfs（对应于GRUB之类引导器的情况），这取决于 UEFI 应用的配置。

# 常见 BIOS 设置

## 常见启动引导器

BIOS 或 UEFI 加载并初始化硬件完成后，会启动的一个启动引导器来接管硬件设备，引导操作系统启动的工作将有启动引导器来完成。

引导程序引导方式及程序视应用机型种类而不同。例如在普通的个人电脑上，引导程序通常分为两部分：第一阶段引导程序位于主引导记录（MBR），用以引导位于某个分区上的第二阶段引导程序，如 NTLDR、BOOTMGR 和 GNU GRUB 等。

## NTLDR/BOOTMGR

NTLDR（NT loader 的缩写）是微软的Windows NT系列操作系统（包括Windows XP和Windows Server 2003）的引导程序。

NTLDR 可以从硬盘以及 CD-ROM、U盘等移动存储器运行并引 Windows NT 系统的启动。如果要用 NTLDR 启动其他操作系统，则需要将该操作系统所使用的启动扇区代码保存为一个文件，NTLDR 可以从这个文件加载其它引导程序。

NTLDR 主要由两个文件组成，这两个文件必须放在系统分区（大多数情况下都是C盘）：

NTLDR，这是引导程序本身
boot.ini，这是引导程序的配置文件 当 boot.ini 丢失时，NTLDR 会启动第一块硬盘第一个分区上的 \Windows 目录中的系统。
bootmgr (Windows Boot Manager) 是从 Windows Vista 开始引进的新一代开机管理程序，用以替换 NTLDR 。

当电脑运行完 POST (Power On Self Test) 后，传统型 BIOS 会根据引导扇区查找开机硬盘中标记"引导"分区下的 bootmgr 文件；若是 UEFI 则是 bootmgr.efi 文件，接着管理程序会读取开机配置数据库 BCD (Boot Configuration Database) 内的引导数据，接着根据其中的数据加载与默认或用户所选择的操作系统。

## GNU GRUB 及其使用

GNU GRUB（简称“GRUB”）是一个来自 GNU 项目的启动引导程序。GRUB 是多启动规范的实现，它允许用户可以在计算机内同时拥有多个操作系统，并在计算机启动时选择希望运行的操作系统。GRUB 可用于选择操作系统分区上的不同内核，也可用于向这些内核传递启动参数。

GRUB Legacy / GURB2

新的GRUB2（GRUB第二版）為GRUB的重写版本，它是GRUB的大革新。GRUB2对Linux系統做了更多的优化，支持更多的功能，如动态的载入模块（而在之前的GRUB中，新增或刪除模块要重新编译GRUB）等。GRUB2的版本号为0.98或更高；旧的GRUB的版本号则为0.97或更低，也被称为“GRUB Legacy”或“GRUB1”等。GRUB2的配置、命令等较GRUB Legacy有一定的不同。

GRUB2 的配置文件

GRUB2 配置文件的文件名和位置随系统的不同而不同；常见为/boot/grub/grub.conf。

修改GRUB的配置文件后，不必把GRUB重新安装到MBR或者某个分区中。在Linux中，“grub-install”命令是用来把GRUB的步骤1安装到MBR或者分区中的。GRUB的配置文件、步骤2以及其它文件必须安装到某个可用的分区中。如果这些文件或者分区不可用，步骤1将把用户留在命令行界面。

除了硬盘外，GRUB也可安装到光盘、软盘和U盘等移动介质中，这样就可以带起一台无法从硬盘启动的系统。

使用 GRUB Shell

# LINUX 启动过程

## VMLINUZ

vmlinux：

一个非压缩的，静态链接的，可执行的，不能bootable的Linux kernel文件。是用来生成vmlinuz的中间步骤。

vmlinuz：

一个压缩的，能bootable的Linux kernel文件。vmlinuz是Linux kernel文件的历史名字，它实际上就是zImage或bzImage：

zImage： 仅适用于640k内存的Linux kernel文件。

bzImage： Big zImage，适用于更大内存的Linux kernel文件。

对于目前的 Linux 桌面系统，vmlinuz 实际上即 bzImage kernel 文件。

## INITRD

initrd 的英文含义是 initialized RAM disk，就是由 bootloader 初始化的内存盘。在 linux内核启动前， bootloader 会将存储介质中的 initrd 文件加载到内存，内核启动时会在访问真正的根文件系统前先访问该内存中的 initrd 文件系统。

initrd 分 image-initrd 及 cpio-initrd 两种：

2.4 及以前的内核只支持 image-initrd，其核心文件是/linuxrc。
2.6 及以后的内核两种格式的 initrd 都支持，并且目前的 Linux 发行版使用的几乎都是 cpio-initrd，其核心文件是/init。
在 bootloader 配置了 initrd 的情况下，内核启动被分成了两个阶段，第一阶段先执行 initrd 文件系统中的/init(或早期的/linuxrc)，完成加载驱动模块等任务，第二阶段才会挂载真正的根文件系统中，并且 chroot 到真正的根文件系统 （例如硬盘上的某个分区）来完成系统的启动。

## 启动参数

# DEEPIN 桌面系统的安装

## 安装环境

请确保您的电脑满足以下的配置要求，如果您的电脑配置低于以下的要求，将无法完美的体验深度操作系统：

处理器：Intel Pentium IV 2GHz 或更快的处理器
内存：至少 2G 内存(RAM)，4G 以上是达到最佳性能的推荐值
硬盘：至少 10 GB 的空闲空间硬件要求
同时，您可能还需要一张光盘以及光驱，如果您的电脑无光驱设备，可登陆深度科技官方网站下载镜像文件并制作启动盘。

## 安装介质的准备

### 下载系统安装镜像

请使用浏览器打开 系统镜像下载页面，下载深度操作系统系统最新版本的镜像文件（以便您能够体验到最新特性）。

注意：

如果电脑只有2G或以下的内存空间，推荐下载32位版本。如果电脑拥有超过2G以上的内存空间，推荐下载64位版本。
安装64位需要CPU支持x86-84或AMD64指令集支持，您可以使用CPU-Z（Windows软件）查看当前电脑是否支持该指令集。
校验镜像：

下载深度操作系统镜像完成后，需要对其进行校验，错误的镜像将不能用于深度操作系统的安装，校验方法如下：

Windows系统：

下载Hash软件，校验您下载的镜像的MD5值与下载页面提供的MD5值是否一致

Linux系统：

在镜像文件目录打开终端，然后执行：

md5sum deepin-xx-xx.iso
deepin-xx-xx.iso即为您下载的系统镜像文件名，可使用Tab键自动补全文件名。请确认您下载的镜像的MD5值与下载页面提供的MD5值是否一致

### 刻录安装光盘

暂略

### 启动U盘的制作

请使用深度科技团队开发的深度启动盘软件制作工具制作启动U盘，你可使用压缩软件打开深度操作系统镜像提取。

请将U盘插入电脑后，运行深度启动盘制作工具，选择深度操作系统镜像开始制作启动盘，制作期间请不要移除U盘，制作完成请选择重启电脑。

注意：

- 制作前请提前转移U盘中重要数据，制作时可能会清除U盘所有数据
- 制作前建议讲U盘格式化为FAT32格式，以提高识别率
- 部分U盘实则为移动硬盘，因此无法识别，请更换为正规U盘
- U盘容量大小不得小于2G，否则无法成功制作启动盘
- 制作过程中请不要触碰U盘，以免因为写入不全导致制作失败

## 几种安装方案

### 全新安装 DEEPIN 单系统

所谓安装 deepin 单系统，即计算机上不保留其他操作系统，并且使用单独的分区格式化后安装 deepin。

准备工作：

如果是全新的电脑，或者硬盘中的文件数据均已备份无需保留，则直接使用光盘或U盘启动电脑进入安装操作即可。

如果电脑中已有文件数据，则可以在现有系统（如 Windows）下将文件移动或备份，留出至少一个20G的空白分区；或者使用磁盘工具（推荐分区助手，下载地址）并选择一个剩余空间合适的分区进行大小调整，使磁盘中有20G以上的未分配空间或空白分区。

### 与 WINDOWS 共存安装双/多系统

在计算机上已经安装后 Windows 操作系统的情况下，如果想要保留已有 Windows 系统，则可安装双/多系统，实现 deepin 与 Windows 的共存。

与全新安装一样的，保证磁盘上有20G以上的未分配空间或空白分区即可。

### 在 WINDOWS 下安装体验版

暂略

安装过程

一般情况下电脑默认是从硬盘启动，因此，在使用光盘安装系统之前，您需要先进入电脑的BIOS界面将光盘设置为第一启动项。

台式机一般为 <kbd>Delete</kbd> 键、笔记本一般为 <kbd>F2</kbd> 或 <kbd>F10</kbd> 或 <kbd>F12</kbd> 键，即可进入 BIOS 设置界面。

您只需在享受一杯咖啡的时间，便可完成系统的安装。

1. 将深度操作系统光盘插入电脑光驱中。
2. 启动电脑，将光盘设置为第一启动项。
3. 进入安装界面，选择需要安装的语言。
4. 进入账户界面，输入系统用户名和密码。
5. 点击 下一步。
6. 选择文件格式、挂载点、分配空间等。
7. 挂载点 挂载点中文名 文件系统大小
8. / 根分区（必选） EXT4（推荐） 最少10G
9. /home 主目录（推荐） EXT4（推荐） 最少10G
10. swap 交换分区（可选） 不设置 4G内存以下分配2G，4G以上可不分配
11. 点击 安装。
12. 在弹出的确认安装窗口中，点击确定。
13. 将开始自动安装深度操作系统。
14. LIVE 环境与引导修复

## 进入 LIVE 环境

使用系统修复光盘或U盘启动计算机即可进入 Live 桌面环境。

## 进入 CHROOT 环境

在 Live 桌面环境中打开终端

确定硬盘上系统的各个挂载点的分区，以下表所示情况举例：

|   挂载点   |    分区    |     备注     |
|-----------|-----------|--------------|
| /boot/efi | /dev/sda1 |  UEFI 下特有  |
|     /     | /dev/sda2 |              |
|   /home   | /dev/sda3 |如未单独分区则忽略|
|   /var    | /dev/sda4 |如未单独分区则忽略|

挂载文件系统

    mkdir /tmp/mnt #建立临时挂载目录
    cd /tmp/mnt #进入目录
    sudo mount /dev/sda2 ./ #挂载根分区
    sudo mount /dev/sda1 ./boot/efi #仅在UEFI模式下挂载EFI分区
    sudo mount /dev/sda3 ./home #如果仅修复引导此分区可不挂载
    sudo mount /dev/sda4 ./var #如果/var单独分区，必须挂载
    sudo mount --bind /sys ./sys
    sudo mount --bind /proc ./proc
    sudo mount --bind /dev ./dev
    sudo mount --bind /dev/pts ./dev/pts

进入 chroot 环境

    sudo chroot /tmp/mnt

修复 GRUB 引导

切记，修复本机 GURB 引导需要在 chroot 环境下操作！

传统 BIOS 下：

    grub-install --boot-directory=/boot /dev/sda
    update-grub

UEFI 模式下：

    grub-install --boot-directory=/boot --efi-directory=/boot/efi
    update-grub

# 参考资料

[ArchWiki:Arch boot process](https://wiki.archlinux.org/index.php/Arch_boot_process_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

[维基百科：Windows Boot Manager](https://zh.wikipedia.org/wiki/Windows_Boot_Manager)

[维基百科：NTLDR](https://zh.wikipedia.org/wiki/NTLDR)

[ArchWiki:GRUB](https://wiki.archlinux.org/index.php/GRUB_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

[维基百科：GNU GRUB](https://zh.wikipedia.org/wiki/GNU_GRUB)

[IBM developerWorks：Linux2.6 内核的 Initrd 机制解析](https://www.ibm.com/developerworks/cn/linux/l-k26initrd/)
