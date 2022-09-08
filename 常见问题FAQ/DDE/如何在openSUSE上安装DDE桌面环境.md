---
title: 如何在openSUSE上安装DDE桌面环境
description: DDE桌面环境在opensuse leap 15.3及以上版本是有较好适配，为openSUSE用户多提供一个桌面环境的选择。
published: true
date: 2022-09-08T08:46:23.082Z
tags: linux, opensuse, dde桌面环境
editor: markdown
dateCreated: 2022-09-08T08:46:23.082Z
---

# 如何在openSUSE上安装DDE桌面环境

参考wiki[《Portal:Deepin/Installation》](https://en.opensuse.org/Portal:Deepin/Installation)，目前，DDE桌面环境在opensuse leap 15.3及以上版本是有较好适配的。

**警告！ Deepin Desktop 有一些[安全问题](http://en.opensuse.org/Portal:Deepin/Security_Issues)，请不要在重要的地方部署 Deepin Desktop 。**

#### 1. openSUSE 镜像安装

openSUSE镜像下载地址：https://download.opensuse.org/distribution/leap/15.3/iso/openSUSE-Leap-15.3-3-DVD-x86_64-Build38.1-Media.iso

安装镜像时，可先选择GNOME桌面版系统（建议不要安装KDE桌面版系统，防止与DDE桌面环境冲突）。如下图安装时系统版本选择：

![1.png](/for_trans/opensuse和dde/1.png)

#### 2. 安装DDE桌面环境

Deepin Desktop 已经被 openSUSE 官方所支持，你可以先在 Leap 上配置好仓库。终端中使用命令：

```linux
sudo zypper ar -f https://download.opensuse.org/repositories/X11:/Deepin/openSUSE_Leap_15.3/X11:Deepin.repo
```

安装DDE桌面环境，终端中使用命令：

```linux
sudo zypper in -t pattern deepin
```
安装完成后，重启，进入登录界面，点击设置按钮，选择deepin桌面环境。
![2.jpg](/for_trans/opensuse和dde/2.jpg)

#### 3. 登录

默认的显示管理器是SDDM ，你可以在菜单中选择 "Deepin" 来登录Deepin桌面。此外，你也可以使用其他显示管理器。

lightdm是上游的默认显示管理器，但它不能在Leap 15.3和更高版本上启动Deepin Desktop。SDDM和GDM在 Deepin Desktop上则可以正常启动Deepin桌面。

#### 4. Dbus 和 Policykit 特色

为了确保你的 openSUSE 处于[安全状态](http://en.opensuse.org/Portal:Deepin/Security_Issues)，我们默认禁用了 deepin-api 和 deepin-daemon 的所有 dbus 和 policykit 特性，以及 deepin-file-manager。这导致 Deepin Desktop 不能完整地正常工作，一些功能会失效。

- deepin-lock 上的壁纸是无效的。你看不到任何按钮，但它们确实在屏幕的中间。在屏幕中间左右移动光标，你可以看到按钮；
- 初始屏幕上的壁纸是无效的。你看不到任何按钮，但它们确实在屏幕的中间；
- lightdm-deepin-greeter 是无效的；
- deepin-lock无法启动，无法通过gui关机重启；
- 无法通过控制中心管理用户和网络。

如果你想启用 Dbus 和 Policykit ，并且不关心安全问题，你可以用 root 权限手动运行 deepin-daemon-dbus-installer 和 deepin-daemon-polkit-installer或安装deepin-feature-enable来启用这些功能。具体终端命令参考如下：

```linux
sudo deepin-daemon-dbus-installer
```

```linux
sudo deepin-daemon-polkit-installer
```

```linux
sudo zypper in deepin-feature-enable
```



