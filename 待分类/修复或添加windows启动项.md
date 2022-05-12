---
title: 修复或添加windows启动项
description: 
published: true
date: 2022-05-07T07:49:25.974Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:46:23.191Z
---

## 简介

使用U盘安装linux（ubuntu、deepin）后，windows系统启动/引导项丢失，可使用下面的方法恢复了win7启动/引导项。

## 工具/原料

- 计算机
- windows、linux（ubuntu、deepin）

## 方法/步骤

1. 在终端中输入：`sudo gedit /boot/grub/grub.cfg`
   回车，然后输入密码，就打开了grub.cfg文件。

2. 用下面的代码替代 `### BEGIN /etc/grub.d/40_custom ###` 和 `### END /etc/grub.d/40_custom ###` 之间原有的代码即可：

```
menuentry "Windows 7" {
insmod part_msdos
insmod ntfs
set root='(hd0,msdos1)'
chainloader +1
}
```

之后，保存，重启，即可发现你的windows就回来啦！

3. 这里 `set root='(hd0,msdos1)'` 中的 `'(hd0,msdos1)'` 为你windows系统所在分区，该方法适用于添加windows7和window10开机引导项，windows8没有测试过。

4. 其中 `menuentry "Windows 7"{` 里的 `Windows 7` 就是出现在开机引导/启动项中的名字，可以用你喜欢的名字替换！

## 注意事项

注意你的windows系统所在分区！

该方法仅在ubuntu和deepin下测试过！
