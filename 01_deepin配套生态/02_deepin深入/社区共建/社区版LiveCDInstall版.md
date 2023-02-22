---
title: Deepin Community Live CD Install 新版本——一个定制的系统安装镜像
description: Deepin Community Live CD Install 新版本——一个定制的系统安装镜像
published: true
date: 2022-12-27T01:45:30.581Z
tags: deepin, live cd
editor: markdown
dateCreated: 2022-11-01T12:49:57.773Z
---

# 前言

> 我发稿前才发现镜像的 grub 忘记换了，也没时间重传了，毕竟 DDUC 只有一天，能用就行。
> :joy:
> 祝今天 DDUC 12 顺利进行，深度热爱，一同进化。希望今天 DDUC 12 有爆料。
> :blush:

# 介绍

这个镜像是接着 Deepin Community Live CD 系列的（Deepin Community Live CD 简称为 `DCLC`，Deepin Community Live CD 系统是什么？传送门：https://bbs.deepin.org/post/242933） ，自然此镜像会同时拥有原 Deepin Community Live CD 的功能和系统安装功能
也为了简化安装后的操作，会预装一些软件以及一些配置等等
***注意：这个和鸿玩并不一样，不会修改任何有关 deepin 的系统信息，包括但不限于系统版本、系统 logo 等等***
***且并不是 deepin 的下游发行版，只是一个定制的镜像***
***Live CD 模式下用户默认密码：123456，root 密码：123456，安装到本地的不受此影响***

![图片.png](https://storage.deepin.org/thread/202212240937174208_图片.png)

## 更新内容

1. 跟进系统版本为 20.8
2. 更新预装的星火应用商店和 Wine 运行器版本，星火应用商店版本号：4.0.1，Wine 运行器版本号：3.0.0.1-uos
3. 开始选择性安装 UEngine 安卓环境和 UEngine 运行器（若不想安装，可以通过断网或在体验模式下打开添加自定义脚本进行修改）

![图片.png](https://storage.deepin.org/thread/202212240941481968_图片.png)

![图片.png](https://storage.deepin.org/thread/202212240941227743_图片.png)

## 预装软件

1. 星火应用商店（联网会自动更新）
2. Wine 运行器（联网会自动更新）
3. QQ（Wine）（会根据网络情况自动选择星火应用商店的版本或官方应用商店的版本）
4. 微信（Wine）（会根据网络情况自动选择星火应用商店的版本或官方应用商店的版本）
5. WPS Office（联网时自动安装）
6. 钉钉（联网时自动安装）
7. Todesk
8. TimeShift
9. udom 工具箱
10. deepin 全家桶
11. UOS远程协助
12. rdp 远程桌面连接工具
13. 搜狗输入法
14. deepin wine6 stable
15. 星火应用商店应用源（只存在于 Live CD 环境下，将不会安装到本地）
16. boot repair（只存在于 Live CD 环境下，将不会安装到本地）
17. Ghost（只存在于 Live CD 环境下，将不会安装到本地）
18. pardus-boot-repair（只存在于 Live CD 环境下，将不会安装到本地）
19. Deepin Installer（系统安装程序，只存在于 Live CD 环境下，将不会安装到本地）
20. Deepin Community Live CD Mini 应用商店（只存在于 Live CD 环境下，将不会安装到本地）
21. gparted（只存在于 Live CD 环境下，将不会安装到本地）
22. Deepin 系统修复工具（只存在于 Live CD 环境下，将不会安装到本地）
23. Live CD 工具（只存在于 Live CD 环境下，将不会安装到本地）

***下面是在虚拟机 Live 模式启动的截图***

![图片.png](https://storage.deepin.org/thread/202212240935037342_图片.png)

```

```

![图片.png](https://storage.deepin.org/thread/202212240934551223_图片.png)

***这里是在虚拟机安装到本地后的截图***

![图片.png](https://storage.deepin.org/thread/202212240949293710_图片.png)

![图片.png](https://storage.deepin.org/thread/202212240949384535_图片.png)

# 可以不看的提示

1. 断网安装将不会安装 WPS、QQ、微信这三个体积很大的软件
2. 体验模式可以当 Live CD 用

# 下载链接

鹤川云盘：https://pan.hechuanyun.xyz/s/Weua
123 云盘：https://www.123pan.com/s/pDSKVv-yRpWv
迅雷云盘：https://pan.xunlei.com/s/VNF2FXAW-Ygci78DPS3b6bVxA1?pwd=5mgs   提取码：5mgs

![image.png](https://storage.deepin.org/thread/202210231313582420_image.png)

百度网盘：链接: [https://pan.baidu.com/s/1n5J8M8iqfI-kMbmHfR-x9w](https://pan.baidu.com/s/1n5J8M8iqfI-kMbmHfR-x9w) 提取码: ejr7
![image.png](https://storage.deepin.org/thread/202203201435562540_image.png)
备用1：[http://gfdgdxi.free.idcfengye.com/DeepinCommunityLiveCD/1.7.0/](http://gfdgdxi.free.idcfengye.com/DeepinCommunityLiveCD/1.7.0/)
程序论坛： [https://gfdgdxi.flarum.cloud/](https://gfdgdxi.flarum.cloud/)

