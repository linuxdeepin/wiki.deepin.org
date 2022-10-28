---
title: linux内核启动参数
description: 
published: true
date: 2022-10-25T01:42:00.747Z
tags: 
editor: markdown
dateCreated: 2022-07-06T02:33:08.477Z
---

# linux 内核启动参数

## 前言

linux 启动问题，安装问题，驱动问题，是很多linux用户特别烦恼的。但事实上，有补救的方法，那就是通过修改内核启动参数，兼容不同硬件环境下，以求进入系统进行下一步操作。

## 显卡问题

### 一、modeset （KMS：kernel mode set)

这是一个新型的显示模式，即内核在启动阶段调用更先进的图形显示技术，以达到更好的显示效果。**副作用就是兼容性低**。

1. 可以设置`nomodeset` 或者`modeset=0`关闭这项功能，至少保证进入字符界面。
2. 更进阶一点的写法是：`i915.modeset=0`,其中i915是intel的显示驱动，该命令可以单独关闭该驱动的KMS。

### 二、动态模块（.ko）

很多视频驱动都是以动态模块的方式链接到内核的，内核在启动过程中会加载这些驱动。

1. `modprobe.blacklist=nouveau` 就禁止加载该模块，这可以屏蔽有问题的驱动程序。

那么究竟有多少个显卡驱动，他们叫什么名字？

1. `nvidia` ： /lib/modules/4.15.0-30deepin-generic/kernel/drivers/video/nvidia.ko ，nvidia官方驱动，性能棒棒的，不过默认没有安装，要自己装，对一般用户是一个挑战
2. `nvidiafb` ： /lib/modules/4.15.0-30deepin-generic/kernel/drivers/video/fbdev/nvidia/nvidiafb.ko ，一个老式的驱动，估计性能很差，兼容性如何还不清楚，因为我没用过。
3. `nvidia_drm` ： /lib/modules/4.15.0-30deepin-generic/kernel/drivers/video/nvidia-drm.ko ，DRM是“直接渲染管理器”的缩写，是一个替代“fb帧缓冲区”的现代版本，包括“GEM图形执行管理器”和“KMS内核模式设置”技术。其作用就是更好的协作多应用和多GPU。其中多GPU技术类似"Nvidia Optimus"，称为"prime"。好吧，说到底就是nvidia驱动组成的一部分。
4. `nouveau` ： 可以理解为nvidia的第三方开源驱动，因为是第三方，所以兼容性差，性能低。因为是开源，和linux的适配度高，喜忧参半的一款驱动，也是nvidia显卡在linux的默认驱动。
5. `i915` ： intel的官方驱动。
6. radeon : 旧的amd显卡设备的开源驱动
7. amdgpu : 新的amd显卡设备的开源驱动
8. amdgpu-pro ： 官方闭源驱动

### 三、initramfs

initramfs 是linux的一个纯净的启动映像环境，简而言之就是制作一个内存中的启动硬盘。

### 四、内核引导参数的使用方法

linux系统大多数通过grub来引导，即开机出现的引导菜单。出现菜单读秒时，按e进入编辑状态，按F10执行编辑后的菜单。

找到`linux`开始的一行，该行格式一般如下：

```bash
linux /.../vmlinuz... root=UUID=xxx ...
```

翻译过来就是：

“引导linux 内核文件 启动分区 内核参数”

我们要修改的就是内核参数部分。

参数格式：

1. 模块名.参数=值
2. modprobe 动态模块名 参数=值
3. 参数

获取参数的方法：

1. /sys/module/模块名/parameters
2. /proc/cmdline 启动参数
3. `modinfo -p 模块名`

常用模块：

- `ACPI` 高级电源配置接口
- `APIC` 高级可编程中断控制器
- `DRM`  直接渲染管理器
- `FB`   帧缓冲
- `IOMMU`   设备内存相关
- `KVM`  虚拟机
- `LOOP` 回环设备
- 显卡驱动：i915、nvidia、nouveau等

常用参数：

- `quiet` 安静模式
- `debug` 详细信息
- `memtest=1` 测试一次内存
- `erst_disable` 解决ERST故障
- `hest_disable` 解决HEST故障
- `nosoftlockup`  禁止软死锁检测
- `nowatchdog`  禁止硬死锁检测
- `noapic` 禁止IO-APIC输入输出高级可编程中断控制器
- `nolapic` 禁止cpu-apci
- `acpi=off` 禁止acpi高级电源配置接口
- `acpi=strict` 严格的acpi
- `acpi_backlight=vendor` 使用厂家的屏幕亮度调节器
- `acpi_os_name=\"Linux\"` 告知acpi系统名称，欺骗有问题的BIOS。
  - `\"Windows 2001\"` xp
  - `\"Windows 2006\"` vista
  - `\"Windows 2009\"` win7
  - `\"Windows 2012\"` win8
  - `\"Windows 2013\"` win8.1
- `acpi_osi="Linux"` 添加字符串
- `acpi_osi=!` 删除所有字符串
- `pnpacpi=off` 关闭acpi的即插即用功能
- `resume=/dev/swap` 休眠启动的分区
- `resume_offset=xxx` 休眠文件的偏移量
- `resumedelay=6` 等待5秒启动休眠设备
- `resumewait` 等待直到休眠设备准备好
- `kvm-amd.nested=0` 关闭amd cpu的嵌套虚拟化
- `root=xxx` 启动设备或文件
- `rootfstype=xxx`  启动设备的文件系统类型
- `rotflags=xxx` 挂载选项
- `ro` 只读
- `rw` 读写
- `init=路径` 启动程序
- `rdinit=路径`  内存盘中的启动程序
- `nomodule` 禁用内核模块加载
- `3` 启动命令行和非图形界面
- `splash` 启动画面

## 参考

1. Linux 内核引导参数简介: <https://www.cnblogs.com/wangjuneng/p/4510082.html>