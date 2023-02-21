---
title: Linux更新内核设备驱动中“可加载固件”的方法
description: 
published: true
date: 2022-06-28T06:19:26.731Z
tags: 插件 加固 linux
editor: markdown
dateCreated: 2022-06-28T06:19:24.232Z
---

# Linux更新内核设备驱动中“可加载固件”的方法
“可加载固件”是Linux内核设备驱动的非开源部分，由设备厂商提供，在设备使用时由内核设备驱动的开源部分加载。

可加载固件通常由发行版提供，但发行版提供的版本通常不新，缺少很多新设备的可加载固件，或者与新内核不兼容。

如果你有不能驱动的设备，或者安装新版本内核后设备工作不正常，可以尝试更新可加载驱动。更新方法：

1. 安装`git`和`make`，请自行搜索你的发行版的安装方法。下面给一些参考命令

```ruby
# Debian, Ubuntu, Deepin, UOS
sudo apt update
sudo apt install git make

# Fedora
sudo dnf install git make

# Archlinux
sudo pacman -Sy git make
```

2. 在终端执行以下命令安装最新可加载固件：

```bash
git clone --depth=1 https://mirrors.tuna.tsinghua.edu.cn/git/linux-firmware.git
cd linux-firmware
sudo make install
```

3. 在终端执行命令更新启动映像文件，每个发行版的操作方法都不同。下面给一些参考命令

   ```perl
   # Debian, Ubuntu, Deepin, UOS
   sudo update-initramfs -k all -u
   
   # Fedora
   sudo dracut --force --no-hostonly
   
   # Archlinux
   sudo mkinitcpio -P
   ```

4. 重启即可生效。