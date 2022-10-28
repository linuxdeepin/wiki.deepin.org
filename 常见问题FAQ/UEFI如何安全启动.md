---
title: UEFI如何安全启动
description: UEFI引导从入门到重装
published: true
date: 2022-10-21T16:49:34.261Z
tags: uefi
editor: markdown
dateCreated: 2022-06-28T07:50:35.714Z
---

# 前言

我发现，大部分玩linux的人，出现问题的主要原因都是卡在安装系统这个门槛上。因此，决定彻底的研究和解决这个问题，让大家能够愉快的使用linux，投入开源的怀抱。

本文假设你具备如下知识：

1. 懂得进入bios
2. 懂得百度
3. 懂得用工具翻译简单的英语单词
4. 具有基本的电脑常识

# 一、历史

硬件开机需要经过自检过程，自检程序bios固化到主板里面，从bios时代 到uefi 的转变，让启动系统变得更加强大（虽然也不是很智能）。

CSM 兼容性支持模块，uefi兼容老bios的手段，应该关闭，因为这个会出现问题。

UEFI很多改进，但对我们关心的启动问题而言，它比BIOS进步的地方是：

1. 简单的引导菜单
2. 没有512字节引导扇区限制
3. BIOS支持的主引导记录MBR只有4个分区，并且同时只能选中一个活动分区，UEFI没有这个限制 （关键改进）
4. 从不限大小的文件启动，而非看不见的扇区 （关键改进）
5. 更多引导选项
6. 基于unicode 的输入、输出、错误控制台
7. 众多shell 工具和支持.nsh 脚本

# 二、UEFI 原理

启动管理器功能：

1. 控制操作系统和efi应用程序引导顺序和位置
2. EFI驱动程序
3. 引导管理器选项
4. NVRAM维护菜单（存储在主板中）
5. flash更新
6. 硬件诊断测试

启动步骤：

1. 主板通过菜单读取设备上的引导文件
2. 如果没，读取bootia64.efi（现bootx64.efi)
3. 如果没，通过网络读取PXE 服务器引导
4. 如果没，启动efi shell （如果有）

# 三、UEFI 引导windows

windows 支持uefi的版本是从win7开始，记住只选64位。现在很多整合的启动u盘印象还是32位的，不建议使用。

## 3.1 用到的工具：

1. bcdboot.exe ：可以设定主板引导菜单，可以复制引导文件到esp引导区（不需要手动复制，以免不完整导致bug），推荐使用！
2. bootmgfw.efi ：这是windows的核心引导程序,在\Windows\Boot\EFI目录可以找到,安装光盘其实也有,你可以搜索看看
3. bootx64.efi ：这是uefi默认加载的程序。
4. bcd ：这是windows引导菜单文件,
5. diskpart ： 这是一个磁盘命令行工具，uefi引导需要磁盘的mbr分区表格式转化为gpt分区表格式。（注意涉及这些涉及硬件操作的命令，应该用管理员权限，并注意备份数据）
6. bcdedit : 这是一个编辑bcd菜单的命令行工具


## 3.2 基本策略：

1. 在主板中建立启动菜单，而不是争夺bootx64.efi的使用权。windows很野蛮，他会将自己的启动文件bootmgfw.efi覆盖这个文件。如果你的linux依赖这个文件来启动，再次安装windows的时候就会启动不了。但是如果你是在主板里面建立的直接引导linux启动文件的菜单，它就是独立不被干涉的。
2. 建立uefi shell工具，这个工具可以帮助修复引导菜单，或者加载进入系统，再通过系统使用修复菜单。

## 3.3 附加工具（图形化）：

命令工具虽然能够使用，但是不太友好，要求用户背指令，因此可以选择一些图形化的工具来辅助解决问题。

1. DiskGenius 磁盘管理
2. BOOTICE 这是bcd菜单编辑工具，版本1.3.4 x64。

## 3.4 演练

### 3.4.1 确认主板是否支持uefi

1. 如果是全新安装，进入bios选择uefi模式，关闭csm兼容模式，关闭安全启动，安装系统即可。
2. 如果已经安装，确认是否以uefi模式安装的，还是mbr模式安装。如果是uefi模式安装，可以关闭csm兼容模式，关闭安全启动。
3. 如果是mbr模式安装，需要转换成uefi模式（下一步）

### 3.4.2 调整磁盘，将mbr转换为gpt，建立esp分区，调整分区留出空间给下一个系统

这一步用diskgenius操作比较舒服.下面用diskpart 演示一下:

1. 先以管理员方式启动cmd

![以管理员方式启动cmd](https://gitee.com/deepinwiki/wiki/raw/master/pics/用管理员启动cmd.png)

2. 录入
   `diskpart` 进入磁盘管理模式

`list disk` 列出磁盘,可以查看磁盘信息:

![磁盘信息](https://gitee.com/deepinwiki/wiki/raw/master/pics/磁盘信息.png)

我的磁盘都是Gpt格式的了,只有磁盘4不是,磁盘4是我刚插进用来演示的u盘.diskpart的使用逻辑是你想操作什么,要先选中该对象,从选中磁盘,到选中分区,一层一层进行.转换分区是对整个磁盘的分区表进行操作的，所以应该选中磁盘。

`select disk 4` 选中磁盘4.

`convert gpt` 将选中的磁盘转换为gpt分区表格式。

![u盘转换gpt错误](https://gitee.com/deepinwiki/wiki/raw/master/pics/u盘转换gpt错误.png)

看来u盘是无法演示下去了。在磁盘管理模式用`exit`可以退出.

那么用diskgenius继续演示：

![dg磁盘工具主界面](https://gitee.com/deepinwiki/wiki/raw/master/pics/dg磁盘工具主界面.png)
![dg右键菜单](https://gitee.com/deepinwiki/wiki/raw/master/pics/dg右键菜单.png)

对磁盘4右键菜单，选中“**转换分区类型为GUID格式**”这个菜单项，即是转换gpt的另一种说法。

![转换提示空间不足](https://gitee.com/deepinwiki/wiki/raw/master/pics/转换提示空间不足.png)

遇到问题不要惊慌，都是很正常的。转换需要空间，怎么腾出空间？这个都是dg磁盘工具可以实现的功能。

用过磁盘工具的朋友都知道，磁盘是以一个陌生的单位来计算容量的，那就是扇区，那么一个扇区等于多少个字节呢？一般是512Byte，也就是半K，逼死强迫症。33除以2等于20k左右，非常小的一个容量需求。只要调整分区大小，前后腾出空间即可。

![调整分区大小](https://gitee.com/deepinwiki/wiki/raw/master/pics/调整分区大小.png)

先划分1000mb空余在前端，100mb空余在后端，压压惊。

顺利转换后，新手朋友一定要学会观察信息，不要因为陌生就忽略：

![转换后的磁盘信息](https://gitee.com/deepinwiki/wiki/raw/master/pics/转换后的磁盘信息.png)

可以看到磁盘类型变成gpt了，理论上，这时候你的磁盘就能被uefi识别，并用来引导。

但是，你应该建立一个专门用来放引导文件的分区，它叫esp分区，格式是非常通用的fat格式，因为不同系统支持的分区格式不相同，uefi不可能每一种都能操作，因此它就选择了最简单的分区格式，让操作系统们将引导文件放里面。

因为我们刚才预留了很多前端空间，这时候就派上用场了。右键选择磁盘4下面的F:分区，选择菜单：建立ESP/MSR分区：

![998esp分区](https://gitee.com/deepinwiki/wiki/raw/master/pics/998esp分区.png)

因为刚才gpt分区表用了一点点，所以分了个998mb的esp分区。

最后是保存改动，一切才会生效。如果你误操作，可以别保存关闭，再打开重来一遍。

到此，磁盘转换完毕！

### 3.4.3 复制启动文件到esp分区

如果不复制启动文件到磁盘4的esp分区，是否就无法启动磁盘4上面的系统？

非也！ 因为你可以在其他磁盘，甚至是主板上面的引导菜单（需要用工具建立）启动磁盘4。因此最关键是磁盘的gpt格式。那么esp分区相对一个磁盘来说它是怎样的作用呢？它是为了让该磁盘独立引导的时候，uefi可以在此找到bootx64.efi 文件。

esp默认是被系统隐藏的，diskpart可以解除这个限制：

同理，`diskpart`进入磁盘工具，`select disk 4`选择磁盘4，`list partition`查看分区信息：

![u盘分区信息](https://gitee.com/deepinwiki/wiki/raw/master/pics/u盘分区信息.png)

显示*系统*即esp分区，这时候应该先选中这个分区，然后才能进行操作：`select par 2`,然后分配个z:盘符给它`assign letter=z`。

这时候，你实际上可以访问它，但是默认权限不是管理员权限的，用管理员权限打开cmd，通过命令行来打开z:盘,或者,直接用diskgenius来操作。

其实你需要分配一个盘符，也能通过diskgenius操作esp盘，问题你只能手工复制相关文件，其实还是用命令行高效，而且有保证一些，因为你不清楚下一代windows会怎么修改启动文件的配置，用命令行可以保证你的操作兼容性更高一些。

**重要命令**:

`bcdboot d:\windows /l zh-cn /s z: /f UEFI /addlast /d /v` 基本格式就是bcdboot 系统 /l 语言 /s esp所在分区 /f uefi启动 /addlast 添加菜单项放在后面 /d 保留默认启动项 /v输出详细信息。当然这里面有些参数不是必要的。

这个命令最终的结果是什么？

他就是复制了 bootx64.efi、bcd、bootmgfw.efi、bootmgr.efi、memtest.efi还有一堆语言包。

![](https://gitee.com/deepinwiki/wiki/raw/master/pics/esp内容1.png)

![](https://gitee.com/deepinwiki/wiki/raw/master/pics/esp内容2.png)

前面也说了，最关键的是  bootmgfw.efi和bcd。bootx64.efi 只是一个占位文件，bcdboot直接就是把bootmgfw.efi改名成bootx64.efi而已。其他都不是关键的。

但是，放的位置是至关重要！

- efi/boot/bootx64.efi
- efi/microsoft/boot/bootmgfw.efi
- efi/microsoft/boot/bcd

只需要如此安放，即可完成启动文件的配置了。

但是，uefi怎么知道我们需要启动哪个硬盘的esp分区呢？经过我测试，如果是外置的u盘，uefi会自动增加启动项，如果是内置硬盘，uefi就比较傻，这里面的设计似乎不是很完善，也许不同厂家有不同的设计方案，所以下一步，我们来实现怎样增加启动选项在主板中，彻底解决些模糊的地方。

### 3.4.4 编辑引导菜单

这里应该明确两个引导菜单，一个是主板里面的，我的主板需要按F11才会弹出这个菜单，或者直接进入bios内部设置启动顺序。这个我称为主板引导菜单。

另一个是操作系统生成的引导菜单，在这里就是存放在esp分区里面的bcd文件。这个bcd是windows专用的引导菜单，如果用linux，它可能用的是grub的引导菜单。windows的引导菜单是不会理会linux的，非常傲娇，而linux的grub引导菜单则可以引导windows。

明显的，我们如果操作的是主板的引导菜单，无疑是最高层级的，因为它是uefi自身的引导菜单，而不是某种操作系统自带的。只要兼容uefi，不管是linux，或者windows，或者chinaOS都没问题的。

然后，我这里首先介绍的是怎么操作bcd引导菜单：

`bcdedit /store z:\efi\microsfot\boot\bcd` 这里的bcdedit就是操作bcd菜单的系统自带工具，/store是指定bcd文件，如果你不是修改当前系统的。整个命令会列出bcd菜单的内容：

![](https://gitee.com/deepinwiki/wiki/raw/master/pics/bcdedit列出bcd内容.png)

第一条{bootmgr}是windows 启动管理器，即bootmgfw.efi。 第二条是windows 启动加载器，即\windows\system32\winload.efi。比如你装了两个windows，一个在c盘，一个在d盘，那么两个系统的加载器就分别在自己的windows目录下面。而device指定了盘符。

总的流程就是bootmgfw.efi 打印出引导菜单bcd，用户选择某个菜单项，它即调用该项的系统加载器来装载系统。

明白这个过程，对于若干个windows来说，我们只需要添加对应的菜单项即可，用bcdboot 可以实现，因为它添加新菜单项并不会覆盖旧的bcd，而会智能处理每一次的变更。

如果不想使用命令行，可以借助bootice这个工具来编辑菜单，新版1.3.4甚至还能修改主板的菜单，非常强大。

![](https://gitee.com/deepinwiki/wiki/raw/master/pics/bootice修改bcd.png)

选择修改z:\efi\microsoft\boot\bcd 引导菜单。

![](https://gitee.com/deepinwiki/wiki/raw/master/pics/修改bcd菜单.png)

我现在c盘里面有一个win10，d盘有个win7，e盘有个deepin，现在我增加win10的启动项：

![](https://gitee.com/deepinwiki/wiki/raw/master/pics/win10启动项1.png)
![](https://gitee.com/deepinwiki/wiki/raw/master/pics/win10启动项2.png)

发现还支持所谓的实模式，试一下能不能加载deepin：

![](https://gitee.com/deepinwiki/wiki/raw/master/pics/deepin启动项.png)

选择uefi页，修改启动序列，可以对主板菜单做修改，推荐这个方式去实现引导！：

![](https://gitee.com/deepinwiki/wiki/raw/master/pics/uefi启动菜单.png)

## 3.5 演练结果

1. 用bcdboot更新菜单项是最稳定的,如果bcd有问题,先删了再用bcdboot添加.注意,bcdboot 先加入低版本windows,后加入高版本windows,这样高版本可以兼容低版本。另外,经测试win7不是完美兼容uefi的。
2. bootice虽然是一个很老的工具，连win10的选项也没有，但是大多数情况都能使用。
3. 实模式无法引导linux efi文件
4. UEFI菜单需要添加bootmgfw.efi路径。添加esp分区中的bootmgfw.efi。
5. 主板会自动添加硬盘的windows boot manager选项,索引到c盘的esp分区\EFI\Microsoft\Boot\bootmgfw.efi(这个应该每个厂家不一定相同),就算你删了或者改名,他自动重新生成.并且，如果发生引导失败，windows boot manager就会自动抢占第一。
6. u盘有点特殊，第一次生成：UEFI：（FAT）SanDisk菜单项，但是并不能进入u盘的引导，然后会生成第二个同名菜单项，就能引入。莫名其妙！也许是因为我u盘弄了两个盘，让主板傻了，两个盘都是uefi可以识别的，如果只有一个盘应该会比较正常。

好，uefi引导windows部分到此结束。

# 四、UEFI 引导linux

因为，linux一般能够比较好的协调和windows的并存关系，当你正确安装windows（用第三节的知识），linux的安装就是水到渠成的事情。

如果是全新安装，同样的，进入bios改成uefi启动，关闭csm兼容模式，关闭安全启动secure boot。

注意磁盘分区表格式为gpt。安装镜像版本比较新，能够支持uefi，然后正常安装即可。

如果你想深入定制linux的引导，请参考第七节 grub的使用说明。

# 五、UEFI 做引导盘

## 5.1 引导设备的文件系统

有一点需要明确，u盘，光盘等即插即用设备，它的引导方式和硬盘有所区别。

尤其是光盘，它使用的是专门的光盘格式，即.iso结尾的镜像，而硬盘的镜像是没有这种法则的。

当然，现在（2019年）我们更加常见的设备是u盘。因此我们主要是和各种硬盘镜像，或者虚拟机硬盘镜像打交道。

当然，我们下载的镜像，却经常是.iso结尾的。因此也要求我们对此有所掌握。

硬盘分区结构有bmr和gpt两种，文件系统有windows 的 fat，ntfs；linux 的ext；苹果Mac OS的hfs。

光盘文件格式：

- cdfs iso-9660 level 1 支持8.3短文件名 
- cdfs iso-9660 level 2 支持长文件名
- udf ios官方改进  
- joliet 微软对iso-9660的改进  
- hfs 苹果的改进  
- rock ridge 操作系统unix的改进  

## 5.2 用到的工具

以linux为例：

1. mkfs
2. cp
3. dd
4. mount
5. losetup
6. mkisofs

## 5.3 总体思路

光盘也好，u盘也好，总的来说，只需要借助工具生成可引导格式便可。对u盘来说，基本就是复制硬盘启动的思路，只有细微差别。对于光盘来说，需要满足iso格式的特殊规定，启动区空间和容量有所限制。

所谓可引导设备，说到底就是让uefi能够在特定位置读取里面的引导管理器，如bootx64.efi等文件，所做的必要布局而已。

1. 准备数据文件
2. 准备引导文件
3. 镜像制作工具
4. 写入设备

对不同文件系统之间的数据操作过程是：

1. 加载镜像a
2. 加载镜像b
3. 文件从a复制到b

整体来说，数据操作就是简单的复制文件，而启动镜像的制作在准备数据文件的基础上，使用特定的制作工具，加特定的引导文件即可。

## 5.4 u盘启动镜像

其实可以直接对u盘进行操作。如第三节演示的那样。但这里还是按照 先准备数据，启动文件，然后制作镜像，然后写入u盘这个思路进行演练。

以debian linux 最小镜像**mini.iso**为例，下载地址：<http://ftp.nl.debian.org/debian/dists/stretch/main/installer-amd64/current/images/netboot/> 。

![](https://gitee.com/deepinwiki/wiki/raw/master/pics/miniiso.png)

这是一个可以启动的光盘镜像，里面的内容是这样的：

![](https://gitee.com/deepinwiki/wiki/raw/master/pics/mini镜像内容.png)

当然，我们可以简单的提取文件，但是linux支持挂载iso文件系统，先挂载了再操作也是挺方便的。

```bash
sudo apt install udftools #安装udf光盘文件系统工具套件，安装完就有mkfs.udf了

sudo apt install mkisofs #安装通用光盘制作工具，比上一个强大，建议使用

```

`file mini.iso` 显示这个镜像并非光盘镜像，而是一个硬盘镜像，很多时候下载的镜像，虽然是iso结尾，但是却并不一定是光盘文件系统的，这有点奇葩。

![](https://gitee.com/deepinwiki/wiki/raw/master/pics/查看miniiso类型.png)

为了提取里面的引导文件，我想提取里面的启动数据，因此就必须要挂载这个硬盘镜像，根据信息，是这个硬盘有三个分区，如果你用`mount`直接挂载它，是挂载第一个分区，其他两个分区需要使用`losetup`命令。

这里需要了解一下linux的设备和文件系统基本知识。

linux中的设备被虚拟成一个文件，放在/dev/目录下，通过一个/dev/loop# （#是数字）的设备，可以将一个普通文件模拟成硬盘，然后对该文件进行读写，分区，格式化文件系统等操作。

因此，通过这个手段，可以不刻录到实际设备上，也能够装载镜像文件。但是如何读取镜像中的分区信息，并挂载其中一个分区呢？这就是losetup工具的能力。

```bash
losetup -f # 查看还没有被使用的loop设备

fdisk -l mini.iso #fdisk是一个分区工具，这命令列出镜像的分区情况

sudo losetup -o 122880 /dev/loop0 mini.iso #根据信息，扇区240开始是efi分区，每个扇区512字节，240*512=122880 ，设置装载的偏移开始点在这里。然后这个loop0就是建立在该分区上的虚拟设备了

sudo mount /dev/loop0 mini #将设备装载入本地文件系统中的mini目录，mini目录内的内容就是分区的内容。然后就能通过普通的文件操作，执行数据任务了。

```

这个分区只有一个文件：efi/boot/bootx64.efi . 接下来就是准备好我们要建立的镜像的内容。

总结一下，因为这部分的操作细节和顺序很重要！很容易出错。如果你想打开镜像的某一个分区：

1. 用fdisk 或者gdisk查看分区类型，分区起始扇区，扇区大小，计算好偏移量（字节）
2. 用losetup装载指定偏移位置（字节）的镜像到设备/dev/loop#
3. 用mount装载设备内容到指定的目录下
4. 卸载过程正好相反，umount-》losetup -d 

如果你想制作镜像：

1. 用`dd` 建立一个大文件，可以装载整个文件系统大小的。
2. 用`fdisk` 或者 `gdisk` 分区，设置好分区类型（注意），记住每个分区的起始位置，和计算偏移量（字节）
3. 用`losetup` 装载指定偏移位置（字节）的镜像到设备/dev/loop#，记住这个设备和分区的对应关系
4. 用`mkfs -t 文件系统`格式化设备，即是格式化了对应分区
5. 用`mount`装载设备内容到指定的目录下，然后就可以复制到这个分区了。
6. 卸载过程相反顺序进行。




# 六、UEFI shell

EDK（EFI开发工具包）子项目。shell即命令行。uefi shell很重要，因为不需要启动进入系统，它就可以让你执行很多命令，从而可以修复引导程序。

1. Shell.efi 精简版
2. Shell_full.efi 全功能版
3. shellenv.efi 扩展nshell 

```shell
memmap 1> Foo.txt #标准输出重定向，unicode编码
memmap >a Foo.txt #标准输出重定向，ASCII编码
memmap 2>> Foo.txt #错误输出追加，unicode编码

```

shell 命令：
`dmen` `drivers` `dh` `dmpstore` `devtree` `help` `?` `err 错误级别` `devices` `map 映射` `guid 显示guid` `connect` `mount 块设备安装文件系统` `pci 配置pci` `disconnect` `load 加载驱动` `mm 设置mem/io/pci` `openinfo` `unload 卸载映像` `reset 重置系统` `reconnect` `loadbmp` `stall 停顿` `drvcfg` `nshell` `getmtc` `drvdiag` `ver 版本` `hexedit` `loadpcirom` `memmap 内存映射` `Setsize` `bcfg 显示配置` `Set 设置环境变量` `Dblk 显示块设备` `alias 别名`

shell 内部命令：
`help` `dh 显示句柄` `map` `guid` `alias` `set` `exit` `cd 进入目录` `type`

shell 外部命令：

1. 类dos：
   `attrib 属性` `date 时间` `edit` `time` `break` `cls 清屏` `mode` `vol` 
2. 类Unix：
   `ls 列出文件` `rm 删除` `cp 复制` `mv 移动` `mkdir 新建文件夹` `type` `alias` `set` `touch`

.nsh 批处理命令：
`echo 回显` `exit 退出` `for` `goto` `if` `pause 暂停` 
`startup.nsh 自动运行批处理`

## 6.1 efi shell环境使用

技巧：

1. 向上箭头 ： 上一个命令
2. 向下箭头 ： 下一个命令
3. TAB ：补全
4. PgUp ： 向上滚动屏幕
5. pgDown： 向下滚动屏幕
6. -b 参数： 分页显示信息
7. -q 参数： 取消确认
8. help 命令： 命令的使用帮助
9. * : 匹配任意字符串
10. cls 7：清屏，并设置背景色为浅灰
11. -sfo 参数： 标准格式化输出信息
12. mode 80 25 : 将控制台设置为80×25大小。
13. reset： 重启
14. set ： 查看和设置环境变量

命令：

1. ver 显示shell的版本
2. 文件管理：`cp`复制/`rm`删除/`del`删除/`ls`显示文件列表/`comp`比较文件内容/`edit`编辑/`eficompress`压缩/`efidecompress`解压缩/`mkdir`新建文件夹/`mv` 移动/`parse`检索内容/`setsize`设置文件大小/`touch`跟新文件时间/`type`显示内容/
3. `map -r`：刷新识别的fat文件系统
4. `BLK0:` :进入BLK0盘
5. `cd`： 进入目录
6. `connect 17 19` 将19驱动连接到17设备
7. `dblk blk0 2 1` 打印块设备第2块的1块内容
8. `devices` 输出设备列表：1.设备编号/2.设备类型(R.控制器、B.总控制器、D.设备控制器)/3.配置协议/4.诊断协议/5.父控制器数/6.设备数量/7.子控制器数/8.设备名称
9. `devtree`  树状图输出设备
10. `dh 52` 显示52号设备信息。-p 协议，-d 显示驱动， -v 详细信息。
11. `disconnect` 断开驱动
12. `dmem 2 1` 显示内存地址2的1byte内容
13. `drivers` 驱动列表：1.驱动编号/2.版本/3.类型/4.配置协议/5.驱动协议/6.设备数/7.子设备数/8.驱动名称/9.设备路径
14. `ifconfig -l`列出网络配置
15. `load file` 加载驱动，-nc 不自动连接设备。
16. `memmap` 内存信息
17. `openinfo 90` 显示和句柄关联的信息
18. `pci` 列出pci信息：1.段号/2.总线号/3.设备号/4.功能号/名称-类别 设备
19. `smbiosview` 显示smbios信息
20. `vol` 文件系统的卷信息


# 七、GRUB 和其他引导系统

grub 是开源世界一个流行的引导管理器，它有点过于强大，不但能引导各种各样的linux，还能引导windows。即支持bmr又支持uefi，甚至支持各种各样的文件系统，还能增加字体，美化菜单。这简直就是个微型操作系统，也正是如此，导致它也过于复杂。但不管怎么说，grub还是开源世界最流行的引导管理器。

## 7.1 安装

主文件夹：

- /boot/grub/ 
- /etc/grub.d/
- /etc/default/grub/
- /usr/lib/grub

默认菜单：

- /boot/grub/grub.cfg 主菜单由下面脚本合成
- /etc/grub.d/00_header 环境变量
- ……/05_debian_theme 背景，颜色，主题
- ……/10_linux 内核菜单项
- ……/20_memtest86 内存测试菜单项
- ……/30_os-prober 其他系统菜单项
- ……/40_custom 用户菜单项
- /etc/default/grub 默认环境设置

Linux内核和镜像：

- vmlinuz 内核常见文件名
- initrd.img 映像常见文件名

grub能力列表：

- grub-mkconfig 菜单生成
- save_env、load_env、grub-editenv 配置存储
- 通过文件系统标签和UUID识别设备
- 支持pc efi，pc coreboot
- 支持ext4，hfs+和ntfs
- 支持LVM、RAID设备
- 提供图形终端和图形菜单
- 支持翻译、国际化
- 提供启动映像
- 支持动态加载的模块，和灵活构建核心映像
- 多引导规范：multiboot、chainload、loopboot
- 识别可执行格式：a.out 和 elf
- 支持解压缩gzip和xz文件
- 传统磁盘调用CHS（最大1024柱面）和逻辑块地址LBA
- 远程终端
- grub-mkrescue 制作grub救援cd

设备和路径语法：

- `(fd0)` 软盘驱动设备，第一个编号0
- `(hd0,msdos2)` 硬盘1，msdos分区2，分区从1开始编号，扩展分区从5开始编号。msdos是一个分区方案，另一个是gpt
- `(hd1,gpt2)/boot/a.txt` 路径格式
- `/boot/` unix 路径
- `\\?\c:\` windows UNC表示路径
- (cdrom) 光盘
- (ahci0)
- (usb0) usb设备
- (memdisk) 内存盘
- (host) 网络主机
- 0+100 读取0到99块
- (hd0)+1 读取硬盘1的第一个块，即开始的512字节


grub引导镜像：

- `/usr/lib/grub/x86_64-efi*` 新式efi
- `/usr/lib/grub/i386-pc` 传统bios
- boot.img 512b大小的第一段镜像，支持mbr
- cdboot.img 制作可引导cd的引导镜像，需添加iso9660和biosdisk模块
- diskboot.img 硬盘启动版
- pxeboot.img 网络启动版
- lnxboot.img 模拟linux内核，兼容lilo
- kernel.img grub核心功能
- core.img 整合后的核心镜像，由grub-mkimage构建，推荐小于32kb
- *.mod 大部分功能以模块文件提供

网络引导：

- grub-mknetdir --net-directory=/srv/tftp --subdir=/boot/grub -d /usr/lib/grub/x86_64-efi

com串行终端：

- serial --unit=0 --speed=9600
- terminal_input serial;terminal_output serial

构建命令：

1. `grub-install 设备`： `--boot-directory=`启动根目录，grub默认复制内容到此 `--recheck` 重建设备映射，/boot/grub/device.map；`--no-rs-codes` 关闭bios启动模式的引导记录备份；`--compress=no|xz|gz|lzo` 压缩选项；`--directory=`模块目录；`--fonts=`字体文件名；`--install-modules=`安装的模块；`--pubkey=`嵌入的公钥；`--locale-directory=/usr/share/locale`本地化文件；`--modules=`预载模块；`--themes=starfield`主题；`--allow-floppy`软盘引导格式；`--bootloader-id=`efi下的子目录名；`--core-compress=xz|none|auto`内核映像压缩选项；`--disk-module=`bios模式下的磁盘选项；`--efi-directory=`efi根目录；`--force`强制；`--force-extra-removable`强制安装到移动设备；`--force-file-id`强制不使用uuid；`--label-bgcolor=`背景色；`--label-color=`前景色；`--label-font=`字体；`--macppc-directory=`mac ppc；`--no-bootsector=`不要安装启动扇区，bios；`--no-nvram`不要更新主板的启动菜单；`--no-uefi-secure-boot` 不要安装安全启动映像；`--product-version=`发行版本；`--removable` 启动镜像可用于移动设备；`--skip-fs-probe`不检索其他系统；`--target=x86_64-efi`镜像类型；`--uefi-secure-boot` 安装安全启动镜像。 
2. `grub-mkconfig -o /boot/grub/grub.cfg`: 生成配置文件，通过系统/etc/grub.d/下的脚本，和/etc/default/grub脚本生成。
3. `grub-mkpasswd-pbkdf2`:生成密码
4. `grub-mkrelpath 路径`：生成相对启动根的路径。
5. `grub-mkrescue -o grub.iso` 生成grub急救盘，通过调用grub-mkimage，xorriso，mkisofs生成。`--modules=`模块；`--xorriso=xorriso`；`--grub-mkimage=grub-miimage`；
6. `grub-mount 设备 挂载点` 可用来检查镜像内容
7. `grub-probe 路径|-d 设备` 得到设备的一些信息，`--target=fs`文件系统，`fs_uuid`文件系统通用唯一标识，`fs_label`分区标签，`drive` grub表示的设备名，`device`系统设备名，`partmap`分区映射模块，`hints_string`适用search的搜索提示。`--device-map=`/boot/grub/device.map
8. `grub-script-check /boot/grub/grub.cfg` 检查脚本是否存在语法错误
9. `grub-editenv 文件名 命令`：修改环境块，用来保存用户设置。`create` 新建空环境块；`list`列出当前环境块。`set 变量=值` 设置变量；`unset 变量` 删除变量；
10. `grub-file --is-x86_64-efi 文件`：测试文件是否为参数指定类型
11. `grub-fstest 映像 命令`：`blocklist 文件` 显示块列表；`cat 文件` ；`cmp 文件 本地文件` 比较；`cp 文件 本地文件`；`crc 文件`；`hex 文件`；`ls 路径`；`xnu_uuid 设备`。--diskcount=；--crypto 加密；--zfs-key；--length=；--root=根设备；--uncompress；
12. `grub-glue-efi` 转换成apple格式
13. `grub-kbdcomp` 制作键盘映射
14. `grub-macbless` 苹果系统bless
15. `grub-menulst2cfg` 旧版菜单转为新版
16. `grub-mkdevicemap -m /boot/grub/device.map` 输出磁盘映射文件
17. `grub-mkfont -o grub.pf2 字体文件`：将普通字体转换为grub专用pf2格式字体文件
18. `grub-mkimage -o 镜像 模块`:制作grub镜像。`--config=`内嵌配置文件；`--compression=xz|none|auto` 压缩；`--directory=/usr/lib/grub/x86_64-efi`；`--publikey=`内嵌签名文件；`--memdisk=`内嵌内存盘文件；`--note` IEEE1275使用；`--format=`镜像格式（x86_64-efi等）；`--prefix=/boot/grub`，主目录；
19. `grub-mklayout` 键盘映射
20. `grub-mknetdir` 创建网络镜像目录，TFTP使用
21. `grub-mkstandalone -o 镜像 源` ：创建全模块版本镜像
22. `gurb-reboot 菜单项`: 设置下次启动的菜单项。--boot-directory=/boot/gurb
23. `gurb-render-label` 苹果.disk_label
24. `gurb-set-default 菜单项`：设置默认启动项
25. `gurb-syslinux2cfg` 转换syslinux菜单成grub菜单。
26. `os-prober` 搜索其他系统信息
27. `efibootmgr` 

演练：

```bash
#1. 安装
sudo apt install grub-efi-amd64 #uefi版grub
sudo apt install efibootmgr #编辑主板中的UEFI引导菜单

#2. 查看信息
lsblk #查看分区和挂载（比较清晰）
sudo blkid #查看设备和分区信息
sudo fdisk -l #查看设备和分区情况
df -Th #查看空间和挂载点
sudo grub-probe -t device /boot/grub #查找grub 位置
#根据大小等信息获取各个系统所在分区，并记录下对应信息，系统：分区：uuid：文件系统。

#3. 简易安装
sudo grub-install --target=x86_64-efi /dev/sda1 #将grub安装到串口硬盘1分区1，默认安装目录是/boot下的grub文件夹，efi镜像默认安装在/boot/efi下的efi/grub文件夹。可以用--boot-directory=参数指定boot根文件夹，--efi-directory=参数指定efi根文件夹。对移动设备需用--removable并指定两者。grub-install实际通过调用其他工具来完成任务。

sudo update-grub #自动更新grub引导信息，这实际是一个脚本，它调用grub-mkconfig更新/boot/grub/grub.cfg

```

## 7.2 构建

## 7.3 配置文件

变量(/etc/default/grub):

- GRUB_DEFAULT 默认启动项
- GRUB_SAVEDEFAULT 保存启动项
- GRUB_TIMEOUT 选择倒计时
- GRUB_TIMEOUT_STYLE 是否展示菜单
- GRUB_DISTRIBUTOR 发行商
- GRUB_TERMINAL_INPUT 输入设备
- GRUB_TERMINAL_OUTPUT 输出设备
- GRUB_CMDLINE_LINUX 内核参数(全部)
- GRUB_CMDLINE_LINUX_DEFAULT 内核参数（标准）
- GRUB_DISABLE_LINUX_UUID 使用UUID
- GRUB_DISABLE_RECOVERY 禁用恢复
- GRUB_VIDEO_BACKEND 图形支持
- GRUB_GFXMODE 分辨率，vesa bios extensions
- GRUB_BACKGROUND 背景图像
- GRUB_THEME 主题
- GRUB_GFXPAYLOAD_LINUX 显示模式
- GRUB_DISABLE_OS_PROBER 是否调用grub-prober发现其他操作系统
- GRUB_OS_PROBER_SKIP_LIST 忽略空格
- GRUB_DISABLE_SUBMENU 是否使用二级菜单
- GRUB_ENABLE_CRYPTODISK 加密
- GRUB_INIT_TUNE 音效
- GRUB_BADRAM 过滤内存坏区
- GRUB_PRELOAD_MODULES 预载模块

脚本语法：

- `{} | & $ ; <>` 元字符
- `! [[ ]] {} case do done elif else esac fi for function if in menuentry select then time until while` 关键字
- `\ '' ""` 转义序列
- `$(变量)` 得到变量的值,`$1`第一个参数
- `#注释内容` 注释
- for
- if
- while、until
- function
- menuentry: title --class --users --unrestricted --hotkey --id $2...{}
- submenu
- break
- continue
- return
- setparams
- shift
- serial: --unit --prot --speed --word --parity --stop
- terminal_input: --append|--remove 终端1 终端2...
- terminal_ouput
- terminfo -a|-u|-v 

```bash
menuentry “win10”{
    insmod ntfs
    search --set=root --label win10 --hint hd2,gpt2
    configfile /boot/grub/grub.cfg
}
menuentry "winxp"{
    insmod ntfs
    search --set=root -fs-uuid {} winxp --hint hd0,msdos2
    ntldr /ntldr
}
```

主题资源：

- 颜色 #8899ff
- 字体 pff2， 用loadfont命令
- title-text 标题
- title-font 标题字体
- title-color 标题颜色
- message-font 
- message-color
- message-bg-color
- desktop-image 背景
- desktop-image-scale-method 缩放
- desktop-image-h-align 水平对齐
- desktop-image-v-align 垂直对齐
- desktop-color 背景颜色
- terminal-box 终端窗口外框
- terminal-border 终端窗口边框宽度
- terminal-left 终端窗口左坐标
- terminal-top 终端窗口顶坐标
- terminal-width 终端窗口宽度
- terminal-height 终端窗口高度
- label： id text font color align visible 组件和属性
- image： file
- progress_bar: id fg_color bg_color text_color bar_style highlight_style highlight_overlay font text
- circular_progress: id center_bitmap tick_disappear start_angle
- boot_menu: item_font selected_item_font item_color selected_item_color icon_width icon_height item_height item_padding item_icon_space item_spacing menu_pixmap_style item_pixmap_style selected_item_pixmap_style scrollbar scrollbar_frame scrollbar_thumb scrollbar_thumb_overlay scrollbar_slice scrollbar_left_pad scrollbar_right_pad scrollbar_top_pad scrollbar_bottom_pad visible
- canvas: id left top width height
- hbox:
- vbox:
- `__timeout__` id值为这个将由grub更新
- cmd颜色名称：black，blue，green，cyan，red，magenta，brown，light-gray，dark-gray，light-blue，light-green，light-cyan，light-red，light-magenta，yellow，white

对grub.cfg脚本的简单分析：

1. 操作环境块grubenv
2. 定义函数：加载视频模块
3. 用uuid搜索字体文件/usr/share/grub/unicode.pf2, 设置gfxpayload=auto，加载视频，加载字体，加载gfxterm图形终端，设置terminal
   _output gfxterm终端输出.
4. 设置菜单显示时间 ，颜色。然后进入实际菜单项
5. linux菜单项：加载视频，加载解压缩gzio，判断是否xen模式，加载part_gpt分区模块，加载ext2文件系统，根据uuid搜索设置root位置，加载linux内核，内核参数root= ro quiet；加载linux启动映像盘initrd。
6. 其他模式的linux系统项就内核参数不同，root= ro single 或 recovery
7. uefi系统安装菜单项：fwsetup
8. windows菜单项：加载 part_gpt，加载fat文件系统，用uuid搜索设置root，用chainloader 链式加载 efi/microsoft/boot/bootmgfw.efi微软系统启动管理器。


## 7.4 引导命令行

默认的启动过程是：

1. normal.mod 模块
2. $prefix/grub.cfg 应先设置set root/set prefix变量


引导进入grub命令行，即出现`grub>`可以输入grub 命令行工具，来修复引导。类似bash，可以使用TAB补全和上下按键寻找历史命令。

- `ls` 列出分区。
- `ls (hd2,gpt1)`  显示分区类型
- `ls (hd2,gpt1)/` 显示分区内容
- `set pager=1` 设置信息滚动一屏暂停
- `set` 显示环境变量，重要信息
- `set root=(hd2,gpt1)` 设置根目录位置
- `insmod 路径` 装载模块，模块一般在/boot/grub/x86_64-efi/目录下，如果没有装载模块，很多命令都不能正常使用
  - `minicmd.mod` 很多命令在这个模块
  - `normal.mod` 基本模块，增加normal命令，进入正常模式。 
  - `linux.mod` linux内核装载模块
- `congfigfile 路径` 装载grub目录文件，确保首先正确加载了相关mod，否则启动菜单也无法引导系统
- `linux 路径` 装载linux内核
- `linux16 路径` 旧款linux内核
- `initrd 路径` 装载系统映像
- `initrd16 路径` 配合linux16使用
- `set prefix=(hd2,gpt1)/boot/grub/` 设置grub目录位置
- `boot` 启动linux系统映像，多重引导规范
- `chainloader (hd2,gpt2) +1` 调到下一个引导管理器，链式引导
- `chainloader /efi/microsoft/boot/bootmgfw.efi` 启动gpt模式的windows引导管理器。
- `drivemap -s (hd0) (hd1)` 安装dos需要在第一个设备，drivemap交换映射
- `parttool` 隐藏分区，安装dos用

可用命令列表：

- $? 返回值，0为true，非0为false
- [ : 检查表达式，[ -d 目录是否存在；[ -e 是否存在；[ -f 文件是否存在；[ -s 非空文件；[ -n 非空字符串；[ -z 空字符串；[ (布尔表达式)； -a 与； -o或； ==； !=； <; <=; >; >=； -eq 整数等；-ge 整数大于等于； -gt 整数大于； -le 整数小于等于； -lt整数小于； -ne 整数不等；-pgt; -plt; -nt;-ot
- acpi: 加载acpi表
- authenticate: 检查用户是否在用户列表中
- background_color: 设置活动终端背景颜色
- background_image: 活动终端的背景图
- badram: 过滤掉内存坏区
- blocklist: 块表
- boot: 启动
- cat: 显示内容 --dos
- chainloader: 链式加载引导管理器
- clear: 清屏
- cmosclean: 清CMOS
- cmosdump: 转储CMOS内容
- cmostest: CMOS测试
- cmp: 比较两文件不同
- configfile: 加载配置菜单文件
- cpuid: 检查cpu功能
- crc: 计算crc32校验和
- cryptomount: 挂载加密设备，luks.mod,geli.mod
- date: 时间
- devicetree: 加载设备树
- distrust: 不信任的公钥
- drivemap: 驱动器映射
- echo: 回显
- eval: 执行参数表示的命令
- export: 导出环境变量
- false: 假
- gettext: 翻译字符串,locale_dir/*.mod
- gptsync: 根据GPT填充MBR
- halt: 关机
- hashsum: 计算哈希校验和 --hash --keep-going --uncompress --check --prefix
- help: 显示帮助信息
- initrd: 加载linux映像包
- initrd16: 
- insmod: 加载模块
- keystatus: 检查按键状态
- linux: 加载linux内核，其余参数作为内核参数添加
- linux16:
- list_env: 列出环境块中的变量
- list_trusted: 列出可信公钥
- load_env: 加载环境储存块
- loadfont: 加载字体
- loopback: 从映像中创建设备，-d删除设备
- ls: 列出设备或文件
- lsfonts: 列出加载的字体
- lsmod: 列出加载的模块
- md5sum: 计算md5哈希
- module: 多引导内核加载模块
- multiboot: 加载多引导兼容内核
- nativedisk: 本机磁盘驱动
- normal: 进入正常模式，自动执行脚本，装载模块
- normal_exit: 退出正常模式
- parttool: 修改分区表条目
- password: 密码
- password_pbkdf2: 哈希密码
- play: 播放曲子
- probe: 检索设备信息
- pxe_unload: 卸载PXE环境
- read: 读入用户输入
- reboot: 重启
- regexp: 测试正则表达式
- rmmod: 删除模块
- save_env: 保存变量到储存块
- search: 搜索设备，--file|--label|--fs-uuid --set --no-floppy
- sendkey: 模拟按键
- set: 设置环境变量
- sha1sum: 计算sha1哈希
- sha256sum: 计算sha256哈希
- sha512sum: 计算sha512哈希
- sleep: 暂停指定秒,--verbose 显示倒计时
- source: 读取配置文件
- test: 检查文件类型并比较值
- true: 真
- trust: 可信秘钥
- unset: 取消环境变量
- uppermem: 高端内存大小
- verify_detached: 验证分离的数字签名
- videoinfo: 列出可用的视频模式
- xen_hypervisor: 加载xen
- xen_linux: 加载xen
- xen_initrd: 加载xen
- xen_xsm: 加载xen

特殊环境变量：

- `biosnum:` bios启动设备号
- `check_signatures:` 数字签名验证
- `chosen:` 选择的菜单项
- `cmdpath:` 定位于引导映像启动的路径
- `color_highlight:` 高亮时前景色/背景色
- `color_normal:` 前景色/背景色
- `config_directory:` 配置文件目录
- `config_file:` 配置文件
- `debug:` 启用调试信息输出
- `default:` 默认菜单项,参考grub-set-default/grub-reboot
- `fallback:` 备用菜单项
- `gfxmode:` 使用gfxterm图形终端
- `gfxpayload:` linux内核视频模式
- `gfxterm_font:` gfxterm字体
- `grub_cpu:` 构建的cpu类型
- `grub_platform:` pc/efi
- `icondir:` 图标目录
- `lang:` zh_CN
- `locale_dir:` 本地化目录/boot/grub/locale
- `menu_color_highlight:` 菜单项高亮模式前景色/背景色
- `menu_color_normal:` 菜单项前景色/背景色
- `net_<interface>_boot_file:`	
- `net_<interface>_dhcp_server_name`:	
- `net_<interface>_domain:`
- `net_<interface>_extensionspath:`
- `net_<interface>_hostname:`
- `net_<interface>_ip:`
- `net_<interface>_mac:`
- `net_<interface>_next_server:`	
- `net_<interface>_rootpath:`
- `net_default_interface:`
- `net_default_ip:`
- `net_default_mac:`
- `net_default_server:`
- `pager:` 显示信息长度（/页）暂停
- `prefix:` /boot/grub
- `pxe_blksize:`
- `pxe_default_gateway:`
- `pxe_default_server:`
- `root:` 根目录所在位置
- `superusers:` 超级用户
- `theme:` 主题
- `timeout:` 倒计时
- `timeout_style:` 倒计时显示方式

环境储存块：

- /boot/grub/grubenv 1k,load_env,save_env

## 7.5 busyBox

busyBox 是一个微型linux系统，一般用在救援系统。

## 7.6 演练

grub的内容相当杂乱，庞大而复杂，所以整理起来也是非常艰难。如果实在看不懂，那么就照以下步骤进行即可（所有步骤皆实际操作）：

```bash
#系统默认已经安装grub，uefi环境下，也就是我推荐的方式，安装的grub组件包括：grub-common、grub-efi-amd64、grub-efi-amd64-bin、grub2-common
# 首先切换到root 管理账号，得到管理权限
sudo passwd root
su

apt list grub* #列出grub相关包，可以看出已经安装的grub包，确认是否uefi环境，然后继续

#1. 简单安装
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=mygrub #x86_64-efi就是grub 的uefi引导框架，--efi-directory这个指示了esp挂载的根在哪里。因为我的esp分区挂载在/boot/efi目录，所以选择在哪里，因此你也可以根据自己的实际情况设置esp分区的根。--bootloader-id 这个参数会在esp分区的/efi/目录下生成对应的文件夹。和mircosoft，ubuntu等文件夹平级。这个命令并不会生成esp分区的/efi/boot/bootx64.efi 默认启动器，只会生成对应文件夹的/efi/mygrub/grubx64.efi启动器，这个时候就没有默认的启动器，如果你想要,可以复制：

mkdir /boot/efi/efi/boot
mv /boot/efi/efi/mygrub/grubx64.efi /boot/efi/efi/boot


#2. 定制安装
# 自动生成的启动器，符合大多数要求，为了演示，接下来对其中某些过程进行定制
# 首先要理解grub-install干了什么，它首先调用了grub-mkconfig生成了菜单，然后调用grub-mkimage生成启动映像

#2.1 生成菜单，会自动调用os-prober搜索磁盘的其他系统，生成菜单
mkdir -p /boot/efi/efi/grub2
grub-mkconfig -o /boot/efi/efi/grub2/grub.cfg

# 自动生成的菜单是根据/etc/grub.d/ 下的脚本，和/etc/default/grub来生成的。因为要定制，所以可以对这个生成后的脚本直接进行编辑。

cd /boot/efi/efi/grub2
nano grub.cfg
#一般只需要增加或删减菜单项即可
```

![](https://gitee.com/deepinwiki/wiki/raw/master/pics/grub.cfg.png)

```bash
#2.2 生成启动映像
grub-mkimage -o grub2.efi --prefix=/efi/grub2 --format=x86_64-efi fat.mod part_gpt.mod #必须要带这俩个模块，否则无法读取gpt分区和fat文件系统的内容，映像必须要读取菜单文件grub.cfg，--prefix指明grub目录位置，另外，菜单用的命令模块也在x86_64-efi目录下面。

cp grub2.efi /boot/efi/efi/boot/bootx64.efi #这个是磁盘默认启动的efi
cp grub2.efi /boot/efi/efi/grub2 #我自定义的grub目录，即现在这个目录已经有grub2.efi 和grub.cfg ,注意grub.cfg的名字是不能改的，已经被固定了，grub2.efi会在设定的目录自动找这个文件。
cp /usr/lib/grub/x86_64-efi /boot/efi/efi/grub2 -r #复制所有模块到目录下面
#重启即可使用自定义菜单启动系统。
cp /boot/vmlinuzxxx /boot/efi/efi/debian/vmlinuz
cp /boot/initrd.imgxxx /boot/efi/efi/debian/initrd.img 
#复制默认在/boot目录下的内核和映像盘到我想要的目录下，因为我在grub.cfg新增了一个测试菜单项，路径就指向那里，演示的是debian系统，所以里面已经有debian目录，就复制到那里即可。

#2.3 修改主板菜单
# 最好是从主板菜单启动，因为bootx64.efi会被其他系统抢用。
efibootmgr -c -d /dev/sda -p 1 -l /efi/grub2/grub2.efi -L grub2-debian -g #-c新建，-d 磁盘；-p 分区；-l 启动路径；-L 菜单标题； -g gpt分区。

efibootmgr -b 0005 -B #删除编号0005的菜单项

#以上在debian虚拟机中测试通过，但是这里是手写的，所以也许有纰漏。

```

## 7.7 其他

# 参考资料

1. UEFI论坛： <http://www.uefi.org>
2. EFI SHELL： <http://www.tianocore.org>
3. ubuntu uefibooting: <https://help.ubuntu.com/community/UEFIBooting>
4. ubuntu grub help: <https://help.ubuntu.com/community/Grub2>
5. archlinux grub wiki: <https://wiki.archlinux.org/index.php/GRUB_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)>
6. gurb官方文档： <https://www.gnu.org/software/grub/manual/grub.html>
7. arch linux boot loaders: <https://wiki.archlinux.org/index.php/Category:Boot_loaders_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)>
8. grub for windows中文项目:<https://github.com/beatfan/UEFI_grub2>
9. IBM 中文社区：<https://www.ibm.com/developerworks/cn/linux/l-grub2/#ibm-pcon>
10. gurb 模块： <https://blog.csdn.net/zuosifengli/article/details/7321142>
11. 编译UEFI版本Grub2引导多系统文件efi: <https://www.jianshu.com/p/326e71f67d58>
12. mkisofs命令制作iso文件： <https://blog.csdn.net/halazi100/article/details/45601239>
13. linux 制作iso 和 刻录DVD: <https://blog.csdn.net/jxm_csdn/article/details/52537831>
14. 文件系统： <https://wiki.archlinux.org/index.php/File_systems_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)>
15. 金步国linux文档： <http://www.jinbuguo.com/>
16. 关于Windows Boot Manager、Bootmgfw.efi、Bootx64.efi、bcdboot.exe 的详解: <https://blog.csdn.net/lindexi_gd/article/details/50392343/>
17. 系统启动流程: https://wiki.deepin.org/zh/03_%E6%8C%89%E7%9F%A5%E8%AF%86%E7%82%B9%E7%AD%89%E7%BA%A7%E5%88%92%E5%88%86/01_%E4%B8%AD%E9%98%B6/05_%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F/%E7%B3%BB%E7%BB%9F%E5%90%AF%E5%8A%A8%E6%B5%81%E7%A8%8B
18. 系统安装-000 基础三：Windows与Linux的UEFI引导修复教程: <https://blog.csdn.net/qq_15304853/article/details/79054734>
19. 制作一个能从各种ISO镜象启动的U盘: <https://noodlefighter.com/post/note_udisk-for-repair/>
20. hpe 精简微服务器UEFI Shell参考：<http://h17007.www1.hpe.com/docs/iss/proliant_uefi/UEFI_TM_030617/GUID-D7147C7F-2016-0901-0A69-000000001B9F.html>