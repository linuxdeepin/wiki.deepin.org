---
title: 解决N卡闭源驱动不能调亮度的问题
description: 
published: true
date: 2022-06-28T03:13:58.301Z
tags: nvidia, 显卡
editor: markdown
dateCreated: 2022-06-28T03:13:56.281Z
---

# 解决N卡闭源驱动不能调亮度的问题
以deepin为例，这里使用`dedit`编辑器。如果你没有`dedit`，请换成自己的编辑器，比如`gedit`，`vim`，`nano`等。

以下要修改的文件，如果存在则修改，如果不存在则忽略。

1. 如果你安装了`deepin-nvidia-prime`，则修改这个文件：

   ```bash
   sudo dedit /usr/share/X11/xorg.conf.d/11-nvidia-prime.conf
   ```

   在`Driver "nvidia"`后面添加一行并保存：

   ```vbnet
   Option "RegistryDwords" "EnableBrightnessControl=1"
   ```

   ![image.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzYyOWU1OTcxNTFmYTFjZjhhZGEzNzQ1ZDBmOTdkYjQ5MjY5ODMucG5n)

2. 再编辑这个文件：

   ```bash
   sudo dedit /usr/share/X11/xorg.conf.d/11-nvidia-prime.conf
   ```

   在`Driver "nvidia"`后面添加一行并保存：

   ```vbnet
   Option "RegistryDwords" "EnableBrightnessControl=1"
   ```

   ![image.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzQ5ZGI0ZWRkODMxYmVjZmJkYmUwY2FjZDk2YmM4NDAxMzYxMTgucG5n)

3. 如果存在`/etc/X11/xorg.conf`，则编辑它：

   ```bash
   sudo dedit /etc/X11/xorg.conf
   ```

   在`Driver "nvidia"`后面添加一行并保存：

   ```vbnet
   Option "RegistryDwords" "EnableBrightnessControl=1"
   ```

   每个人的`xorg.conf`都不一样，有的人没有该文件（比如我就没有），我就不截图了。

4. 如果以上步骤中的文件都不存在，请运行以下命令创建一个`/etc/X11/xorg.conf`，然后执行步骤3。

   ```undefined
   sudo nvidia-xconfig
   ```

重启后应该就能调亮度了。

之所以默认不能调，应该是NVIDIA为了避免兼容性问题，默认没有开启调亮度功能。

注意：强行开启后确实可能遇到兼容性问题，比如有时候亮度可以调到0，导致黑屏。调整时请小心。