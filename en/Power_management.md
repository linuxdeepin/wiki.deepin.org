---
title: Power_management
description: 
published: true
date: 2022-04-21T03:56:42.624Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:56:39.948Z
---



## Summary

Power management concerns the distribution and allocation of the electric power to system components. This is critical to mobile devices that depend on batteries, as the battery life can be prolonged by cutting down power consumption when system components are idle.

There are mainly four types of operations in power management: suspend, hibernate, shut down and reboot.

## Operations

### Suspend

Suspending a system will cause all running programs are saved into memory, and all device except the memory and the processors shut down to save energy. To recover from a suspended state, simply press the power button.

Note: In most cases, "standby", "suspend" and "sleep" refer to a common state, that the memory and the processor are power-on, and the other device are power-off.

### Hibernate

Put a system into hibernation will cause all data in the memory are saved into the swap (either the swap file or the swap partition), and shut down the computer to save energy. To recover from the state of hibernation, press the power button, and the kernel will read the data in swap back to the memory.

Note: There must be enough space in swap to store the data in RAM. It is recommended to create a swap as large as the RAM for hibernation. For using swap file for hibernation, please refer to the [Debian Wiki](https://wiki.debian.org/Hibernation/Hibernate_Without_Swap_Partition).

### Shut down

Shutting down a system will cause all device to lose power. Users need to manually save their work before shutting down to avoid any data lose.

### Reboot

Rebooting a system will cause the computer first to power off all device except the motherboard, then power them on and boot the system again. Users need to manually save their work before rebooting to avoid any data lose.

### Difference between these operations

#### Data security

When a power failure happens during a suspense, all data in the memory will get lost. For a computer in hibernation, data are stored in the hard disk so that no data will be lost.

When shutting down or rebooting a computer, all data in memory will be lost if not saved by users manually.

#### Power consumption

A computer in hibernation state costs fewer energy than that in suspended state, as it does not require extra power to refresh data in the memory. Computers do not consume energy after they have been shut down.

#### Wake-up speed

When resuming from the suspended state, computers do not need to recover data from the hard disk, thus it costs less time than resuming from hibernation.

## How to do

### Using a graphical interface

Click on the power button in the task bar, then you can choose to shut down, restart or suspend the machine.

### Using commands

Execute in terminal:

    sudo pm-hibernate # Hibernate
    sudo pm-suspend  # Suspend
    sudo pm-suspend-hybrid # Do a hybrid suspense: first write data to hard disk, then suspend the computer
    sudo pm-powersave # Enter the power saving mode

## Energy saving settings

### Power plan

Go to the Control Center -> Power Management, and adjust the parameters according to your needs.

### Brightness

Go to the Control Center -> Display -> Brightness, and adjust the brightness to lower the power consumption.

## Processor

For daily usage, processors with high frequency may not be economical. You may lower the frequency of your CPU with the help of "cpufrequtils" or "cpupower" utility.

## Kernel

Running a streamline kernel may reduce the power cost of your computer.

Besides, you may try the dynamic power management for Radeon graphics card provided by Linux kernel 3.11+. To enable it, execute in terminal:

    sudo gedit /etc/default/grub

Find the following line

    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"

and change it to

    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash radeon.dpm=1"

Save your changes, and then execute:

    sudo update-grub

Reboot the machine for changes to take effect.

## Frequently asked quetions

### System cannot hibernate /  be waken up from hibernation

To put the computer into hibernation normally, you have to first set up a swap space of correct size. The recommended configurations are: 512 MB swap for memories less than 1 GB, and  1 GB swap (or the maximum memory usage on your computer) for memories larger than 1 GB.

If this does not work, then there might be some other problems. In this case, avoid hibernation instead.

Note: For old version of deepin installed as Deepwin mode (using wubi) cannot hibernate, as the root (/) is not mounted to a physical device.

### My computer has no function of hibernation

The power management module of Linux kernel detects automaticaally the available swap, and disable the function of hibernation if there is no swap space.

## References

[Arch Wiki:pm-utils](https://wiki.archlinux.org/index.php/Pm-utils)

[4G内存该划分多少swap呢？](http://www.linuxdeepin.com/forum/25/11948)

[Linux 休眠原理与实现](http://biancheng.dnbcw.info/linux/321766.html)