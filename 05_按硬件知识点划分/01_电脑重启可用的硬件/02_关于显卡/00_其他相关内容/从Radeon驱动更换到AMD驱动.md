---
title: 从Radeon驱动更换到AMD驱动
description: 
published: true
date: 2022-06-23T02:20:39.732Z
tags: 
editor: markdown
dateCreated: 2022-06-23T02:20:37.660Z
---

# 从Radeon驱动更换到AMD驱动
已验证在显卡 “Radeon HD 8570 /R7 240/340 /R520 OEM”上可用
## 参考了如下帖子

https://askubuntu.com/questions/1094443/ubuntu-18-04-1-lts-r9-390x-amdgpu-guide-testing-summary
##  安装，如果依赖有问题，不安装也可以
``` linux
sudo apt install mesa-vulkan-drivers mesa-vulkan-drivers:i386
```
## # 添加黑名单屏蔽radeon驱动
``` linux
touch /etc/modprobe.d/blacklist-radeon.conf

echo blacklist radeon >> /etc/modprobe.d/blacklist-radeon.conf

```
## 修改grub

``` linux
/etc/default/grub

GRUB_CMDLINE_LINUX_DEFAULT="amdgpu.si_support=1 amdgpu.cik_support=1 amdgpu.dc=1 amdgpu.dpm=1 amdgpu.modeset=1"
```
## 更新 grub
```  linux
sudo update-grub2 && sudo update-initramfs -u -k all
```
可能产生如下问题：注意那几个missing的文件，需要自己手动下载，并放入指定位置
下载地址： http://anduin.linuxfromscratch.org/sources/linux-firmware/amdgpu/
```
user@user-PC:~$ sudo update-initramfs -u -k all

update-initramfs: Generating /boot/initrd.img-4.19.0-desktop-amd64

cryptsetup: WARNING: The initramfs image may not contain cryptsetup binaries

nor crypto modules. If that's on purpose, you may want to uninstall the

'cryptsetup-initramfs' package in order to disable the cryptsetup initramfs

integration and avoid this warning.

setupcon is missing. Please install the 'console-setup' package.

W: plymouth: The plugin label.so is missing, the selected theme might not work as expected.

W: plymouth: You might want to install the plymouth-themes package to fix this.

W: Possible missing firmware /lib/firmware/amdgpu/oland_uvd.bin for module amdgpu

W: Possible missing firmware /lib/firmware/amdgpu/pitcairn_uvd.bin for module amdgpu

W: Possible missing firmware /lib/firmware/amdgpu/verde_uvd.bin for module amdgpu

W: Possible missing firmware /lib/firmware/amdgpu/tahiti_uvd.bin for module amdgpu

I: The initramfs will attempt to resume from /dev/nvme0n1p7

I: (UUID=00fdf415-1264-4ecd-bf8b-f6f31215a432)

I: Set the RESUME variable to override this.

live-boot: core filesystems devices utils udev blockdev dns.
```


## 重启
``` linux 
reboot
```

## 查看驱动是否更换成功
应有 ```  "Kernel driver in use: amdgpu"```

``` sudo lspci -v | grep amdgpu -B 19```

应有``` "configuration: driver=amdgpu"```

```sudo lshw -c video | grep amdgpu -B 10 -A 1```
