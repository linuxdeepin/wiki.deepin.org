---
title: Deepin Community Live CD
description: 一个社区制作的第三方 Live CD
published: true
date: 2022-12-27T01:44:50.832Z
tags: live cd
editor: markdown
dateCreated: 2022-07-31T02:45:00.884Z
---

> Install 版没传完，先把其它版本发布先
> 祝各位冬至快乐
> （程序里面没写冬至专版，一是完成时间不统一，**二是我今天才知道今天是冬至**:joy: ）
> 镜像 Grub 引导界面的那个更新时间不太相同，属于正常现象（甚至压根没改，反正能用就行）
> :joy:

# 前言

deepin 发布的 Live CD 有点古老，功能有少许不全，在部分新电脑是无法启动，对 ventoy 兼容性不是很好，同时我也想自己定制一个属于自己的 Live CD，于是这个 Live CD 就出现了
旧版 Live CD：
![image.png](https://storage.deepin.org/thread/202203201424371318_image.png)
![image.png](https://storage.deepin.org/thread/202203201425394425_image.png)

# 介绍

此 Live CD 基于 deepin 20.8 和原 Live CD 2.0 制作，安装部分维护工具（如果还有需要添加的就说），感谢 [https://bbs.deepin.org/post/166409](https://bbs.deepin.org/post/166409) 的作者 [@xchngg](https://bbs.deepin.org/user/108842)的参考文档，本 Live CD 1.2.1 及以前版本使用该方案打包，测试 Ventoy 在 Legacy 和 UEFT 模式下均可运行此 Live CD，有常用驱动（网卡、显卡、声卡），理论上能运行 deepin 20.6 均可运行
同时也借鉴了以下文章的内容：
[https://bbs.deepin.org/post/228930](https://bbs.deepin.org/post/228930) [@deepin-superuser](user/278484)

[https://bbs.deepin.org/post/228568](https://bbs.deepin.org/post/228568)  [@木一明](user/160805)

**install、full 用户密码（包括root密码）为：123456**
**tiny、mini root 密码未知**

![image.png](https://storage.deepin.org/thread/202209112148168591_image.png)

![image.png](https://storage.deepin.org/thread/202209112150178582_image.png)

![image.png](https://storage.deepin.org/thread/202209112151255384_image.png)

## 打包所带的程序

1. 远程协助
2. TestDisk
3. ukoapp
4. deepin 修复工具
5. CPU-G/CPU-X
6. GParted
7. 字符映射表
8. Timeshift
9. lub
10. deepin 全家桶
11. todesk
12. Live CD 工具（会过期）
13. Pardus Boot Repair
14. PowerISO
15. 一个小型的应用商店
16. 深度备份还原工具
17. Grub Customizer
18. QQ For Linux（2.0.1）
19. boot repair
20. Ghost
21. ……

# 更新内容

## Tiny 版

无更新

## Mini 版

1. 更新 QQ For Linux 至镜像封装时最高版本

## Full-15.11 版

1. 更新 QQ For Linux 至镜像封装时最高版本
2. 更新了部分 apt 源内更新内容

## Full 版

1. 更新 QQ For Linux 至镜像封装时最高版本
2. 跟进 Deepin 20.8 更新内容

# 5个版本区别

如果不想知道，无脑选 full  或 install 版本

## tiny 版

（当然大小并不 tiny）只是在原版 Live CD 升级了内核，没有多的更新，也没有重新打包（所以壁纸也没有换），适用于应急或者空间、网速以及对功能要求不高且电脑配置较低以及语言非简体中文的用户
目前只更新到 1.2.0 版本

![VirtualBox_deepin live cd Test_08_05_2022_15_36_25.png](https://storage.deepin.org/thread/202205081536449227_VirtualBox_deepinlivecdTest_08_05_2022_15_36_25.png)

![image.png](https://storage.deepin.org/thread/202211082115502052_image.png)

## mini 版

在 tiny 版本的基础上，更新并预装了部分原来没有的应用，目前 1.1.0-mini 新预装了 vim、timeshift，将 lights-firefox 升级为 firefox，适用于嫌弃 full 版本空间大以及 tiny 版本功能不全和电脑配置较低以及语言非简体中文的用户
![图片.png](https://storage.deepin.org/thread/202212222214448582_图片.png)

![图片.png](https://storage.deepin.org/thread/202212222215035384_图片.png)

## full 版

（和上面两个版本无关）基于 deepin 20.8 打包，功能较为完整、预装程序也比较多，但无法选择语言（即不能和 tiny、mini 版本一样启动选择语言），如果没有特殊的问题，建议使用这个版本

![图片.png](https://storage.deepin.org/thread/202212222219111297_图片.png)

![图片.png](https://storage.deepin.org/thread/202212222219339267_图片.png)

![图片.png](https://storage.deepin.org/thread/202212222221006137_图片.png)

## install

基于 deepin 20.8 打包，功能非常完整，并未精简过多应用，支持直接运行系官方的安装向导，定位是社区定制的系统安装镜像，同时有 Live CD 的维护功能和系统安装功能
1.7.0 的 install 版还需要再等等
详细介绍可见：https://bbs.deepin.org/post/244651

![image.png](https://storage.deepin.org/thread/202211082108573458_image.png)

![image.png](https://storage.deepin.org/thread/202211082110097514_image.png)

# 15.11 版

基于 deepin 15.11 制作，满满的回忆
比 full 版本的功能少一点，因为 Deepin 15.11 的底层有些老

![图片.png](https://storage.deepin.org/thread/202212222224545894_图片.png)

![图片.png](https://storage.deepin.org/thread/202212222223589271_图片.png)

# 下载链接

鹤川云盘：https://pan.hechuanyun.xyz/s/Weua
123云盘：https://www.123pan.com/s/pDSKVv-yRpWv

百度网盘：链接: [https://pan.baidu.com/s/1n5J8M8iqfI-kMbmHfR-x9w](https://pan.baidu.com/s/1n5J8M8iqfI-kMbmHfR-x9w) 提取码: ejr7
![image.png](https://storage.deepin.org/thread/202203201435562540_image.png)

和彩云：链接: [https://caiyun.139.com/m/i?0r5CLA9upgtMT](https://caiyun.139.com/m/i?0r5CLA9upgtMT) 提取码:nEy6
![qrcode-分享.jpeg](https://storage.deepin.org/thread/202203201439423300_qrcode-%E5%88%86%E4%BA%AB.jpeg)
备用1：http://gfdgdxi.free.idcfengye.com/DeepinCommunityLiveCD/1.7.0/

