---
title: Eclipse_java_提示Picked_up_JAVA_OPTIONS:
description: 
published: true
date: 2022-04-21T03:33:53.676Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:33:51.735Z
---

## 在java程序执行完之后都会报一个提示一下方法可以去掉提示
[来自论坛](https://bbs.deepin.org/forum.php?mod=viewthread&tid=138244&extra=)
## 终端提示：
> Picked up _JAVA_OPTIONS:    -Dawt.useSystemAAFontSettings=gasp
## 解决方法：
> mv /etc/profile.d/java-awt-font-gasp.sh /etc/profile.d/java-awt-font-gasp.sh.bak
> 之后重启或注销