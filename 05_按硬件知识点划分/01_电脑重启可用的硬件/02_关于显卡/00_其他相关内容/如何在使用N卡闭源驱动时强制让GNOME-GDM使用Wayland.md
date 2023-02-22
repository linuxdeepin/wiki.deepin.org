---
title: 如何在使用N卡闭源驱动时强制让GNOME-GDM使用Wayland
description: 
published: true
date: 2022-10-18T00:56:56.200Z
tags: nvidia, wayland, 显卡
editor: markdown
dateCreated: 2022-06-28T06:22:45.409Z
---

# 如何在使用N卡闭源驱动时强制让GNOME-GDM使用Wayland

## 适用范围

以下教程适用于以下桌面环境：

1. 使用GNOME桌面
2. 使用GDM显示管理器
3. 安装了Nvidia闭源驱动

**不适用**以下桌面环境：

1. 使用LightDM或者其他显示管理器
2. 不使用GNOME桌面，比如使用的是KDE、DDE等
3. 未安装Nvidia闭源驱动

## 已知问题

- 由于Nvidia闭源驱动还没有支持XWayland 3D加速，并且Wine也还没有原生支持Wayland（只能使用XWayland渲染），所以目前Wine游戏助手无法在这种情况下启动3D游戏。我给出这个方案只是为了探索Wayland的支持情况。

------

转自 [https://medium.com/@alex285/how-to-enable-eglstreams-on-fedora-29-30-gnome-nvidia-wayland-fa28a67663bf](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9tZWRpdW0uY29tL0BhbGV4Mjg1L2hvdy10by1lbmFibGUtZWdsc3RyZWFtcy1vbi1mZWRvcmEtMjktMzAtZ25vbWUtbnZpZGlhLXdheWxhbmQtZmEyOGE2NzY2M2Jm) ，有改动。

我在Arch Linux中测试成功，驱动安装的是AUR包`nvidia-beta`。

------

在使用N卡闭源驱动时，GDM会自动禁用GNOME的Wayland支持，但是可以通过以下步骤启用：

1. `sudo vim /etc/gdm/custom.conf`
   确保`WaylandEnable=false`被注释（前面有`#`），如果前面没有`#`则加一个`#`

```csharp
[daemon]
# Uncomment the line below to force the login screen to use Xorg
#WaylandEnable=false
```

2. `sudo vim /usr/lib/udev/rules.d/61-gdm.rules`
   在`Driver=="nvidia" ……`前面加`#`

```shell
# disable Wayland when using the proprietary nvidia driver
#DRIVER=="nvidia", RUN+="/usr/libexec/gdm-disable-wayland"
```

如果只在`Driver=="nvidia" ……`前面加`#`没效果，就在所有行前面都加`#`。

3. `sudo vim /etc/default/grub`
   在`GRUB_CMDLINE_LINUX_DEFAULT="……"`的引号里面，末尾添加空格和`nvidia-drm.modeset=1`

```ini
GRUB_CMDLINE_LINUX_DEFAULT="原有的参数 nvidia-drm.modeset=1"
```

1. 运行`sudo update-grub`命令，或者其他适用于你的发行版的`grub.cfg`更新命令，比如`sudo grub-mkconfig -o /boot/grub/grub.cfg`

2. 重启以使配置生效，然后在选择用户后，点击右下角的齿轮图标。如果你看到里面有“运行于 Xorg 的 GNOME”，说明Wayland已启用，选择另外两个都是Wayland会话。如果没有和`Xorg`有关的条目，则Wayland可能未启用。

3. 登陆后，运行以下命令确认是否处于Wayland会话：

   ```perl
   env | grep XDG
   ```

   如果看到`XDG_SESSION_TYPE=wayland`则处于Wayland会话。
   如果是`XDG_SESSION_TYPE=x11`则处于Xorg会话。

4. 注意：由于Nvidia闭源驱动还没有支持XWayland 3D加速，并且Wine也还没有原生支持Wayland（只能使用XWayland渲染），所以目前Wine游戏助手无法在这种情况下启动3D游戏。我给出这个方案只是为了探索Wayland的支持情况。

5. 如果想打游戏，请注销并选择`运行于 Xorg 的 GNOME`。如果坚持使用Wayland会话，则很多游戏都无法启动，或者你需要使用[wine-wayland](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9naXRodWIuY29tL3Zhcm1kL3dpbmUtd2F5bGFuZA..)。