---
title: 在Deepin自带的安卓模拟器uengine中安装apk的方法
description: 
published: true
date: 2022-06-28T03:16:29.082Z
tags: 
editor: markdown
dateCreated: 2022-06-28T03:16:27.133Z
---

# 在Deepin自带的安卓模拟器uengine中安装apk的方法
使用这个命令可以安装apk：

```bash
uengine install --apk=$PWD/xxx.apk
```

apk路径必须是绝对路径，`$PWD`表示当前路径。如果apk不在当前路径，请写全路径，比如`--apk=/home/hu60/Downloads/xxx.apk`

如果提示`uengine`命令不存在，或者安卓环境未启动，请先前往系统自带应用商店安装一个安卓应用，比如“儿歌点点”，或者在左侧的“手机应用”分类里随便选一个安装。如果应用商店左侧没有“手机应用”，请打开“控制中心”检查更新，安装更新并重启后，应用商店应该会变成有“手机应用”的版本。

然后使用这个命令启动装好的apk：

```kotlin
uengine launch --package=org.anbox.appmgr --component=org.anbox.appmgr.AppViewActivity
```

![file-hash-png-624b3d43fd8217d1008424a8f91220ec170357.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzYyNGIzZDQzZmQ4MjE3ZDEwMDg0MjRhOGY5MTIyMGVjMTcwMzU3LnBuZw..)

需要使用deepin/UOS官方源里的5.10内核才能运行安卓应用，其他内核可能导致安卓容器无法正常启动。

此外，可以使用上述命令也表示deepin/UOS的uengine是开源安卓应用容器anbox的修改版，anbox使用GPL-3进行许可。

------

你可以用这种方法安装微信32位版apk，还能运行在平板模式，手机电脑同时登陆。

参考教程：[在UOS/Deepin的安卓模拟器uengine上运行微信平板模式（手机不掉线）](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy4xMDA5MzAuMS5odG1s)

![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzA3YmQwMDAzMzNlNjI3MWZjNGU0M2ZlNzQxYTEwNTZmNDU5OTk4LnBuZw..)

![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzQ5ZTUxYjYyNWEzYWY3MThlNDhjOTY2MmRmNDM0MWI3NDQ4MzM2LnBuZw..)