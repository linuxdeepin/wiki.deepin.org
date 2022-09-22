---
title: deepin无法登录的问题分析及解决
description: 
published: true
date: 2022-07-07T08:52:38.567Z
tags: deepin, 无法登录
editor: markdown
dateCreated: 2022-07-07T08:42:08.119Z
---

# deepin无法登录问题分析及解决
### 一. 在登陆界面，或者解锁界面无法登陆

1. 常见情况是从dde-daemon升级到新的deepin-authenticate认证框架之后需要重启来完成认证框架的启动和关闭
查看/etc/pam.d/common-auth下面的文件是否包含deepin-authenticate的字样，表示文件已经被替换为了新的认证框架配置文件。
如果已经安装deepin-authenticate但是/etc/pam.d/common-auth下面的配置文件是dde-daemon覆盖的将导致无法登陆。
2. 查看/var/log/authinfo.log中给出的信息
3. 快速解决办法:切换到tty,然后重新安装deepin-authenticate 即  sudo apt reinstall deepin-authenticate
4. deepin-desktop-schemas安装过程中出现异常，导致配置没有跑完
查看`vim ~/.xsession-errors`中报错“com.deepin.dde.display”缺少”color-template-mode“；
通过`gsettings list-keys com.deepin.dde.display`
发现列出来的方法正好缺少上面报错的那个”color-template-mode“；
搜索“com.deepin.dde.display”发现在startdde中定义的，查看startdde版本是否正确；
用`dpkg -S com.deepin.dde.display.gschema.xml`查看这个属于deepin-desktop-schemas；
用`apt policy deepin-desktop-schemas`查看版本是否正确；
查看`vim com.deepin.dde.display.gschema.xml`文件内容是否正确；
用`sudo glib-compile-schemas /usr/share/glib-2.0/schemas/`
将「/usr/share/glib-2.0/schemas/」里面的「*.gschema.xml」编译成「/usr/share/glib-2.0/schemas/gschemas.compiled」，再用gsettings list-keys，就正常了
### 二. 登陆界面关闭了，但马上又显示

此问题在登陆界面已经认证成功，开启x11 session失败导致无法进入桌面环境;
具体需要查看当前登陆用户目录下的～/.xsession-errors. 具体查看的内容是看startdde是否拉起dde-desktop, dde-dock等基础桌面环境。
经常出现的问题是：更新后startdde被卸载导致无法进入桌面环境，需要重新安装startdde。
另一种情况是startdde调用的gsettings文件不存在，导致startdde退出从而无法进入桌面环境。