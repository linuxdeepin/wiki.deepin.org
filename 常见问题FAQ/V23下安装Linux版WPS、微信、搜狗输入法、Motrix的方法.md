---
title: V23下安装Linux版WPS、微信、搜狗输入法、Motrix的方法
description: 
published: true
date: 2022-11-15T07:51:02.910Z
tags: v23
editor: markdown
dateCreated: 2022-11-15T07:48:41.507Z
---

# V23下安装Linux版WPS、微信、搜狗输入法、Motrix的方法

https://bbs.deepin.org/zh/post/241881

昨天晚上将Deepin V23 preview安装到了我的一台联想Miix520实体机上，手动分区（分区方案：efi-300MB，/boot-512MB，/-80GB，/home-剩余空间，/recovery-15GB，linux-swap-11GB），安装顺利。硬件驱动正常、系统使用正常。在玲珑网页商店安装QQLinux版、QQ音乐、VLC、迅雷、谷歌浏览器、wine版TIM。然后自己手动安装了几款在玲珑网页商店找不到的应用（WPSLinux版、搜狗拼音Linux版、向日葵远程、微信Linux版、Motrix下载器），效果：

![2022-11-15_28082.png](/2022-11-15_28082.png)

在此把安装方法分享给大家:

一、WPSLinux版

终端输入：

ll-cli install cn.wps.wps-office
二、搜狗拼音Linux版

搜狗拼音官网下载Linux-x86_64版本的deb包，双击运行deb包即可。

![2022-11-15_35373.png](/2022-11-15_35373.png)


![2022-11-15_82879.png](/2022-11-15_82879.png)
三、向日葵远程

向日葵官网下载Linux-UOS版-amd64版的deb包，双击运行即可完成安装。

![2022-11-15_67718.png](/2022-11-15_67718.png)
这里提醒一下，安装好之后在启动器和桌面看不到向日葵的图标，你只需要在/opt/apps/com.oray.sunlogin.client/entries/applications里找到向日葵的图标，复制到以下两个文件夹内即可：

~/Desktop

/usr/share/applications

注：估计大部分deb格式安装包在v23下安装不显示桌面和启动器图标的问题，按前面的方法找到图标复制到~/Desktop和/usr/share/applications即可。

四、微信Linux版

1、先下载星火商店里的Linux版微信的deb包（原始下载链接：linux微信星火版）；

2、双击运行deb包，即可完成安装；

3、同样的情况，在启动器和桌面看不到微信的图标，你只需要在/opt/apps/store.spark-app.wechat-linux-spark/entries/applications

里找到微信的图标文件，复制到以下两个文件夹内即可：

~/Desktop

/usr/share/applications

4、微信图标在桌面和启动器有了，可以正常双击使用。但是有个问题，就是微信图标显示不正常，强迫症患者需要修复一下图标图片的路径。方法是：

将/opt/apps/store.spark-app.wechat-linux-spark/entries/icons/hicolor里相应尺寸的png文件（或者它们同名的快捷方式）复制到/persistent/linglong/entries/share/icons/hicolor对应尺寸的文件夹下的apps文件夹下（我只截图展示48×48尺寸的，实际上16×16、32×32、128×128、256×256尺寸都需要做同样的操作）。

![2022-11-15_31756.png](/2022-11-15_31756.png)

弄好之后重新登录或者重启一下系统，图标就显示正常了。

五、Motrix下载器

1、在Motrix官网下载Motrix的deb包（我把Motrix的deb包以及几个依赖deb包打包给大家，请自取：https://cowtransfer.com/s/34dd3f4062804d 点击链接查看 [ motrix.zip ] ，或访问奶牛快传 cowtransfer.com 输入传输口令 5g51sk 查看；）

2、双击安装Motrix的deb包时，会提示缺依赖，套娃式缺依赖。。。依赖已经打包给大家，下载下来直接用，缺哪个依赖就先安装哪个依赖，最后才是安装Motrix的deb包。