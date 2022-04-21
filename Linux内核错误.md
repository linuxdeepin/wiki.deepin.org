---
title: Linux内核错误
description: 
published: true
date: 2022-04-21T03:37:35.100Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:37:32.020Z
---

#简介

内核错误(Kernel panic)是指操作系统在监测到内部的致命错误，并无法安全处理此错误时采取的动作。这个概念主要被限定在Unix以及类Unix系统中；对于Microsoft Windows系统，等同的概念通常被称为蓝屏死机。

操作系统内核中处理Kernel panic的子程序（在AT&T派生类以及BSD类Unix中，通常称为panic()）通常被设计用来向控制台输出错误信息，向磁盘保存一份内核内存的转储，以便事后的调试，然后等待系统被手动重新引导，或自动重新引导。该程序提供的技术性信息通常是用来帮助系统管理员或者软件开发者诊断问题的。
操作系统试图读写无效或不允许的内存地址是导致内核错误的一个常见原因。内核错误也有可能在遇到硬件错误或操作系统BUG时发生。在许多情况中，操作系统可以在内存访问违例发生时继续运行。然而，系统处于不稳定状态时，操作系统通常会停止工作以避免造成破坏安全和数据损坏的风险，并提供错误的诊断信息。

#问题分析

kernel panic错误表现 kernel panic 主要有以下几个出错提示：

    Kernel panic-not syncing fatal exception in interrupt
    kernel panic - not syncing: Attempted to kill the idle task!
    kernel panic - not syncing: killing interrupt handler!
    Kernel Panic - not syncing：Attempted to kill init !


查看了一下 linux的源码文件，找到相关位置

    kernel/panic.c
    NORET_TYPE void panic(const char * fmt, ...)
    {
    static char buf[1024];
    va_list args;
    bust_spinlocks(1);
    va_start(args, fmt);
    vsnprintf(buf, sizeof(buf), fmt, args);
    va_end(args);
    printk(KERN_EMERG "Kernel panic - not syncing: %s/n",buf);
    bust_spinlocks(0);

    kernel/exit.c

    if (unlikely(in_interrupt()))
    panic("Aiee, killing interrupt handler!"); #中断处理
    if (unlikely(!tsk->pid))
    panic("Attempted to kill the idle task!"); #空任务
    if (unlikely(tsk->pid == 1))
    panic("Attempted to kill init!"); #初始化

从其他源文件和相关文档看到应该有几种原因：

1.硬件问题

使用了 SCSI-device 并且使用了未知命令

    #WDIOS_TEMPPANIC Kernel panic on temperature trip
    # 
    # The SETOPTIONS call can be used to enable and disable the card
    # and to ask the driver to call panic if the system overheats.
    # 
    # If one uses a SCSI-device of unsupported type/commands, one
    # immediately runs into a kernel-panic caused by Command Error. To better
    # understand which SCSI-command caused the problem, I extended this
    # specific panic-message slightly.
    # 
    #read/write causes a command error from
    # the subsystem and this causes kernel-panic

2.系统过热

如果系统过热会调用panci，系统挂起

    #WDIOS_TEMPPANIC Kernel panic on temperature trip
    # 
    # The SETOPTIONS call can be used to enable and disable the card
    # and to ask the driver to call panic if the system overheats.

3.文件系统引起

    #A variety of panics and hangs with /tmp on a reiserfs filesystem
    #Any other panic, hang, or strange behavior
    #
    # It turns out that there's a limit of six environment variables on the
    # kernel command line. When that limit is reached or exceeded, argument
    # processing stops, which means that the 'root=' argument that UML
    # usually adds is not seen. So, the filesystem has no idea what the
    # root device is, so it panics.
    # The fix is to put less stuff on the command line. Glomming all your
    # setup variables into one is probably the best way to go.

Linux内核命令行有6个环境变量。如果即将达到或者已经超过了的话 root= 参数会没有传进去 启动时会引发panics错误。

    vi grub.conf
    #####################
    title Red Hat Enterprise Linux AS (2.6.9-67.0.15.ELsmp)
    root (hd0,0)
    kernel /boot/vmlinuz-2.6.9-67.0.15.ELsmp ro root=LABEL=/
    initrd /boot/initrd-2.6.9-67.0.15.ELsmp.img
    title Red Hat Enterprise Linux AS-up (2.6.9-67.EL)
    root (hd0,0)
    kernel /boot/vmlinuz-2.6.9-67.EL ro root=LABEL=/
    initrd /boot/initrd-2.6.9-67.EL.img

应该是 其中的 root=LABEL=/ 没有起作用。

4.内核更新

网上相关文档多半是因为升级内核引起的，建议使用官方标准版、稳定版 另外还有使用磁盘的lvm 逻辑卷，添加CPU和内存。可在BIOS中禁掉声卡驱动等不必要的设备。
也有报是ext3文件系统的问题。 

解决： 手工编译内核，把 ext3相关的模块都编译进去。

5.处理panic后的系统自动重启

panic.c源文件有个方法，当panic挂起后，指定超时时间，可以重新启动机器

    if (panic_timeout > 0)
    {
    int i;
    /*
    * Delay timeout seconds before rebooting the machine.
    * We can't use the "normal" timers since we just panicked..
    */
    printk(KERN_EMERG "Rebooting in %d seconds..",panic_timeout);
    for (i = 0; i < panic_timeout; i++) {
    touch_nmi_watchdog();
    mdelay(1000);
    }
修改方法： /etc/sysctl.conf文件中加入

    kernel.panic = 30 #panic错误中自动重启，等待时间为30秒
    kernel.sysrq=1 #激活Magic SysRq！ 否则，键盘鼠标没有响应

内核错误之后的解决办法

Linux的稳定性勿容置疑，但是有些时候一些Kernel的致命错误还是会发生（有些时候甚至是因为硬件的原因或驱动故障），Kernel Panic会导致系统crash，并且默认的系统会一直hung在那里，直到你去把它重新启动！ 不过你可以在/etc/sysctl.conf文件中加入

    kernel.panic = 20

来告诉系统从Panic错误中自动重启，等待时间为20秒！这个由管理员自己设定！ 另外一个讨厌的事情是系统hung住之后，键盘鼠标没有响应，这个可以通过设置Magic SysRq来试着解决，也是在/etc/sysctl.conf中

    kernel.sysrq=1

来激活Magic SysRq！ 这样在挂住的时候至少还有一招可以使， 按住 [ALT]+[SysRq]+[COMMAND], 这里SysRq是Print SCR键，而COMMAND按以下来解释！

    b - 立即重启
    e - 发送SIGTERM给init之外的系统进程
    o - 关机
    s - sync同步所有的文件系统
    u - 试图重新挂载文件系统

当然，谁也不希望经常用到这些招数！:O，有备无患而已

Kernel panic问题如何调试 Linux kernel panic是很难定位和排查的重大故障,一旦系统发生了kernel panic，相关的日志信息非常少，而一种常见的排查方法—重现法–又很难实现，因此遇到kernel panic的问题，一般比较头疼。

没有一个万能和完美的方法来解决所有的kernel panic问题，这篇文章仅仅只是给出一些思路，一来如何解决kernel panic的问题，二来可以尽可能减少发生kernel panic的机会。



#案例分析

    Kernel Panic -- not syncing: attempted to kill idle task

出现这种错误是进入不了操作系统的，kernel panic的成因有多种多样，但这种情况是比较奇特的一种，因为它很可能不是软件的问题，而是硬件的问题。几年前我用带奔三的旧主板时遇到过，当时不知道如何解决，只知道它偶尔出现，放一放也会自行消失，所以当初没有重视。现在，当我重新用上旧主板，这种情况又出现了，而且这一次比较顽固，无论怎样重启，总是这条错误，不但硬盘上现有的两个操作系统都进不去，而且连光驱里的LiveCD也进不去了，这显然不是硬盘的问题，也不是内核的问题。以前我就明白应该是主板的问题，可能是主板太旧，电路信号不太通畅的原因，但不知道怎么办，害得我一天一宿没上网。今天早上去网吧，查了点资料，大体上有几种说法：

一种是在grub作内核引导时添加idle参数，这一种是国内网常见的一种说法；

第二个方法是注意一下bios中显示的CPU或者内存条的温度；

第三种是重新作initrd，即mkinitrd；

第四种是在grub中启动memtest86来测试内存，

这几个是外国人的论坛上说的。我回到家以后，先试了第一种，加了idle的各种参数后，毫无效果，关于第二种方法，我在bios中看到似乎硬件的温度不是可以调节的，但我从这个思路出发，考虑到，如果与内存有关，不妨把三个内存条互换一下位置，也许有效，于是，我把我的三个SD内存换了位置，然后开机，一切正常了。

    Kernel Panic -- not syncing: attempted to kill init

这一种情况的表现是系统的极不稳定。或者进入不了系统，syslog停止于kernel panic；或者重启后可以进入系统，但不久就死机，键盘上的Caps-Lock与Scroll-Lock两个灯在闪。这种错误与上面那个有相同的成因，解决方法也相同。
#相关链接
[维基百科:内核错误](http://zh.wikipedia.org/wiki/%E5%86%85%E6%A0%B8%E9%94%99%E8%AF%AF#Linux_kernel_oops)

[Linux kernel panic解决方法](http://blog.csdn.net/willand1981/article/details/5663356)