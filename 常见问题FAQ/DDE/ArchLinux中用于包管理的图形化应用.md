---
title: ArchLinux中用于包管理的图形化应用
description: 
published: true
date: 2022-10-21T04:14:36.474Z
tags: arch
editor: markdown
dateCreated: 2022-10-21T02:26:26.295Z
---

# Arch Linux中用于包管理的图形化应用
![2022-10-21_2080.jpg](/2022-10-21_2080.jpg)

有一些挑战性。这就是为什么 有几个基于 Arch 的发行版通过提供图形化的安装程序使事情变得简单。

即使你设法安装了Arch Linux，你也会注意到它严重依赖命令行。如果你需要安装应用或更新系统，那么必须打开终端。

是的！Arch Linux 没有软件中心。我知道，这让很多人感到震惊。

如果你对使用命令行管理应用感到不舒服，你可以安装一个 GUI 工具。这有助于在舒适的图形化界面中搜索包以及安装和删除它们。

想知道你应该使用 pacman 命令的哪个图形前端？我有一些建议可以帮助你。

请注意，某些软件管理器是特定于桌面环境的。

## 1、Apper
![2022-10-21_94026.png](/2022-10-21_94026.png)

Apper 是一个精简的 Qt5 应用，它使用 PackageKit 进行包管理，它还支持 AppStream 和自动更新。但是，没有 AUR 支持。

要从官方仓库安装它，请使用以下命令：

`sudo pacman -Syu apper`

## 2、深度应用商店
![2022-10-21_14303.png](/2022-10-21_14303.png)

深度应用商店是使用 DTK（QT5）构建的深度桌面环境的应用商店，它使用 PackageKit 进行包管理，支持 AppStream，同时提供系统更新通知。 没有 AUR 支持。

要安装它，请使用以下命令：

`sudo pacman -Syu deepin-store`

## 3、KDE 发现应用

![2022-10-21_39723.png](/2022-10-21_39723.png)


发现应用不需要为 KDE Plasma 用户介绍。它是一个使用 PackageKit 的基于 Qt 的应用管理器，支持 AppStream、Flatpak 和固件更新。

要在发现应用中安装 Flatpak 和固件更新，需要分别安装 flatpak 和 fwupd 包。它没有 AUR 支持

`sudo pacman -Syu discover packagekit-qt5`

## 44、GNOME PackageKit

![2022-10-21_93168.png](/2022-10-21_93168.png)

GNOME PackageKit 是一个使用 PackageKit 技术的 GTK3 包管理器，支持 AppStream。不幸的是，没有 AUR 支持。

要从官方仓库安装它，请使用以下命令：

`sudo pacman -Syu gnome-packagekit`


## 5、GNOME 软件应用

![2022-10-21_65909.png](/2022-10-21_65909.png)

GNOME 软件(Software) 应用不需要向 GNOME 桌面用户介绍。它是使用 PackageKit 技术的 GTK4 应用管理器，支持 AppStream、Flatpak 和固件更新。

它没有 AUR 支持。要安装来自 GNOME 软件应用的 Flatpak 和固件更新，需要分别安装 flatpak 和 fwupd 包。

安装它使用：

`sudo pacman -Syu gnome-software-packagekit-plugin gnome-software`

## 6、tkPacman


