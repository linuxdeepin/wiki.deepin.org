---
title: Modify_keyboard_mapping
description: 
published: true
date: 2022-05-07T02:30:11.783Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:56:12.333Z
---

Sometimes it is useful to re-map some keys (e.g. Ctrl, Alt) to make shortcut keys easy to use. We can employ "gsettings" command to do that.

## View current keyboard mapping

### View all mappings

    localectl list-x11-keymap-options

### View mappings concerning CapsLock

    localectl list-x11-keymap-options | grep caps:

## Modify keyboard mapping

Takes CapsLock as example. Vim users may want to swap CapsLock key and Esc key, so that they can easily press Esc in some key combinations.

Notice: following commands will take effects immediately. No reboot is needed.

### Disable OSD window when pressing CapsLock

    gsettings set com.deepin.dde.keybinding.mediakey capslock '[]'

### Swap CapsLock and Esc

    gsettings set com.deepin.dde.keyboard layout-options '["caps:swapescape"]'

### Disable CapsLock

    gsettings set com.deepin.dde.keyboard layout-options '["caps:none"]'

### Restore default settings for CapsLock

You can either restore settings for CapsLock only:

    gsettings reset com.deepin.dde.keybinding.mediakey capslock
    gsettings reset com.deepin.dde.keyboard layout-options

or restore setttings for all mappings:

    gsettings reset-recursively com.deepin.dde.keybinding.mediakey
    gsettings reset-recursively com.deepin.dde.keyboard

## Reference

[How to change capslock and other keymapping in deepin linux](https://bbs.deepin.org/forum.php?mod=viewthread&tid=143323)
