---
title: ArchLinux中用于包管理的图形化应用
description: 
published: true
date: 2022-10-21T06:33:16.803Z
tags: arch
editor: markdown
dateCreated: 2022-10-21T02:26:26.295Z
---

# Arch Linux中用于包管理的图形化应用
![2022-10-21_2080.jpg](/2022-10-21_2080.jpg)

安装Arch Linux有一些挑战性。这就是为什么 有几个基于 Arch 的发行版通过提供图形化的安装程序使事情变得简单。

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

![2022-10-21_1219.png](/2022-10-21_1219.png)

它是用 Tcl 编写的 Tk pacman 封装。界面类似于 Synaptic 包管理器

由于没有 GTK/Qt 依赖，它非常轻量级，因为它使用 Tcl/Tk GUI 工具包。

它不支持 AUR，这很讽刺，因为你需要从 AUR安装它。你需要事先安装一个 AUR 助手🔗 itsfoss.com，如 yay。

`yay -Syu tkpacman`

## 7、Octopi

![2022-10-21_94780.png](/2022-10-21_94780.png)


可以认为它是 tkPacman 的更好看的表亲。它使用 Qt5 和 Alpm，还支持 Appstream 和 AUR（通过 yay）。

你还可以获得桌面通知、仓库编辑器和缓存清理器。它的界面类似于 Synaptic 包管理器。

要从 AUR 安装它，请使用以下命令。

`yay -Syu octopi`

## 8、Pamac

![2022-10-21_75236.png](/2022-10-21_75236.png)

Pamac 是 Manjaro Linux 的图形包管理器。它基于 GTK3 和 Alpm，支持 AUR、Appstream、Flatpak 和 Snap。

Pamac 还支持自动下载更新和降级软件包。

它是 Arch Linux 衍生版中使用最广泛的应用。但因为 DDoS AUR gitlab.manjaro.org而臭名昭著。

在 Arch Linux 上安装 Pamac有几种方法。最简单的方法是使用 AUR 助手。

`yay -Syu pamac-aur`

## 总结

要删除任何上面图形化包管理器以及依赖项和配置文件，请使用以下命令将packagename替换为要删除的包的名称。

`sudo pacman -Rns packagename`

这样看来，Arch Linux 也可以在不接触终端的情况下使用合适的工具。

还有一些其他应用程序也使用终端用户界面（TUI）。一些例子pcurses、cylon、pacseek和yup但是，这篇文章只讨论那些有适当的GUI的软件。

注意： PackageKit默认打开系统权限，因而不推荐用于一般用途。因为如果用户属于 wheel组，更新或安装任何软件都不需要密码。

你看到了在 Arch Linux 上使用图形化软件中心的几种选择。现在是时候决定使用其中一个了。你会选择哪一个？Pamac 或 OctoPi 还是其他？现在就在下面留言吧。

