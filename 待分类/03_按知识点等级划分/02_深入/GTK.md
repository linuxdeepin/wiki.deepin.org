---
title: GTK
description: 
published: true
date: 2022-10-21T07:53:17.510Z
tags: gtk
editor: markdown
dateCreated: 2022-06-16T02:24:02.827Z
---

# GTK

[跳到导航](http://old.deepin.wiki/index.php?title=GTK#mw-head)[跳到搜索](http://old.deepin.wiki/index.php?title=GTK#searchInput)

摘自 [GTK 官方网站](https://www.gtk.org/)：



GTK，即 GIMP Toolkit，最初由 [GNU项目](http://old.deepin.wiki/index.php?title=GNU_Project&action=edit&redlink=1)为 [GIMP](http://old.deepin.wiki/index.php?title=GIMP&action=edit&redlink=1) 开发，但现在它已经是一个非常流行的工具包，绑定了很多语言。本文将探讨 GTK 主题、风格、图标、字体和字号的配置工具，也会详细介绍手动配置。



## 主题

GTK 3 和 GTK 4 的默认主题是 Adwaita，但也包含 HighContrast 和 HighContrastInverse 两个主题。 GTK 2 的默认主题是 Raleigh。

要强制使用一个特殊的主题，请设置如下的 [环境变量](http://old.deepin.wiki/index.php?title=环境变量):

- 对于 GTK 3 和 GTK 4，请使用 `GTK_THEME`。例如使用 Adwaita 的深色版本启动GNOME 计算器：

```
$ GTK_THEME=Adwaita:dark gnome-calculator
```

**Note:** To apply the above to desktop shortcuts (or launchers) see [Desktop entries#Modify environment variables](http://old.deepin.wiki/index.php?title=Desktop_entries&action=edit&redlink=1).

- 对于 GTK 2， 请使用 `GTK2_RC_FILES`。例如使用 Raleigh 主题启动 [GIMP](http://old.deepin.wiki/index.php?title=GIMP&action=edit&redlink=1)：

```
$ GTK2_RC_FILES=/usr/share/themes/Raleigh/gtk-2.0/gtkrc gimp
```

**Tip:** `gtkrc` can also be a custom file in your home directory created by any of the [#Configuration tools](http://old.deepin.wiki/index.php?title=GTK#Configuration_tools). See [#Examples](http://old.deepin.wiki/index.php?title=GTK#Examples).

一般情况下将主题文件解压在 `~/.themes/` 或 `~/.local/share/themes/` 目录。

### GTK 和 Qt

同时在系统上安装GTK和QT（通常是KDE组件）程序的人都知道，两者的外观并不怎么协调。关于两者外观统一的问题，参见：[Uniform look for Qt and GTK applications](http://old.deepin.wiki/index.php?title=Uniform_look_for_Qt_and_GTK_applications&action=edit&redlink=1)。

## 请参阅

https://wiki.archlinux.org/title/GTK