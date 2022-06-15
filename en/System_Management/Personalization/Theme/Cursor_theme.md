---
title: Cursor_theme
description: 
published: true
date: 2022-05-31T03:15:54.243Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:54:11.416Z
---

## Summary

This entry gives brief introduction to installation of cursor themes in deepin.

## Installation

### Quick install

After downloading and installing the deb file of the theme, you can open deepin control center -> Personalization -> Theme, and choose the installed theme.

### Manual install

Open file manager with root permission

    sudo dde-file-manager /usr/share/icons

and copy all theme files here, according to the directory hierarchy of the original archive file.

Then go to /usr/share/icons/default/, open file "index.theme" (create one if it does not exists), delete all existing content and add a new line to it:

    Inherits=XXX

where "XXX" is the directory name of the theme.

Now you can open deepin control center -> Personalization -> Theme, and choose the installed theme.

## Uninstall

Open file manager with root permission

    sudo dde-file-manager /usr/share/icons

and remove the files introduced by the theme during installation.

## Reference

[鼠标主题资源下载](http://gnome-look.org/index.php?xcontentmode=36)
