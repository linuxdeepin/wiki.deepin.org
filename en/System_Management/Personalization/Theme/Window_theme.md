---
title: Window_theme
description: 
published: true
date: 2022-05-31T03:18:46.733Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:49.676Z
---

## Summary

This entry give brief introduciton to settings of GTK theme and window them in Deepin desktop environment.

## Description of GTK theme

## Configuration files

There are two places where configuration files of GTK theme and window theme are stored:

- ~/.themes    (user customized settings)
- /usr/share/themes    (global settings）

## Installation

### Quick install

After downloading and installing the deb file of the theme, you can open deepin control center -> Personalization -> Theme, and choose the installed theme.

### Manual install

Open file manager with root permission

    sudo dde-file-manager /usr/share/themes

and copy all theme files here, according to the directory hierarchy of the original archive file.

Now you can open deepin control center -> Personalization -> Theme, and choose the installed theme.

## Uninstall

Open file manager with root permission

    sudo dde-file-manager /usr/share/themes

and remove the files introduced by the theme during installation.

## References

[GTK3.X主题资源下载](http://gnome-look.org/index.php?xcontentmode=167)

[GTK2/3 主题推荐](http://planet.linuxdeepin.com/2012/04/12/gtk-2-and-gtk-3-theme-for-linux-deepin/)
