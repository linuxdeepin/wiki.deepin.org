---
title: Deepin Community Live CD
description: 一个社区制作的第三方 Live CD
published: true
date: 2022-11-01T12:48:37.778Z
tags: live cd
editor: markdown
dateCreated: 2022-07-31T02:45:00.884Z
---

20.7 发布了，就跟进一下

# 前言

deepin 发布的 Live CD 有点古老，功能有少许不全，在部分新电脑是无法启动，对 ventoy 兼容性不是很好，同时我也想自己定制一个属于自己的 Live CD，于是这个 Live CD 就出现了
旧版 Live CD：
![image.png](https://storage.deepin.org/thread/202203201424371318_image.png)
![image.png](https://storage.deepin.org/thread/202203201425394425_image.png)

# 介绍

此 Live CD 基于 deepin 20.6 和原 Live CD 2.0 制作，安装部分维护工具（如果还有需要添加的就说），感谢 [https://bbs.deepin.org/post/166409](https://bbs.deepin.org/post/166409) 的作者 [@xchngg](https://bbs.deepin.org/user/108842)的参考文档，本 Live CD 1.2.1 及以前版本使用该方案打包，测试 Ventoy 在 Legacy 和 UEFT 模式下均可运行此 Live CD，有常用驱动（网卡、显卡、声卡），理论上能运行 deepin 20.6 均可运行
同时也借鉴了以下文章的内容：
[https://bbs.deepin.org/post/228930](https://bbs.deepin.org/post/228930) [@deepin-superuser](user/278484)

[https://bbs.deepin.org/post/228568](https://bbs.deepin.org/post/228568)  [@木一明](user/160805)

**full 用户密码（包括root密码）为：123456**
**tiny、mini、install root 密码未知**

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
18. QQ For Linux
19. ……

# 更新内容

1、升级为基于 20.7，支持最新的 5.18.4-amd-desktop-hwe 内核
2、修复星火源签名问题（且换成镜像源以提高安装速度）
3、新增 15.11 版本
**截止发稿，full最新 1.4.0，install 最新 1.2.1，mini最新1.2.1-1，tiny最新1.2.0-1，install版暂未更新**

# 5个版本区别

如果不想知道，无脑选 full  或 install 版本

## tiny 版

（当然大小并不 tiny）只是在原版 Live CD 升级了内核，没有多的更新，也没有重新打包（所以壁纸也没有换），适用于应急或者空间、网速以及对功能要求不高且电脑配置较低以及语言非简体中文的用户
目前只更新到 1.2.0 版本

![image.png](https://storage.deepin.org/thread/202209112201077726_image.png)
![VirtualBox_deepin live cd Test_08_05_2022_15_42_04.png](https://storage.deepin.org/thread/202205081542284615_VirtualBox_deepinlivecdTest_08_05_2022_15_42_04.png)

![VirtualBox_deepin live cd Test_08_05_2022_15_36_25.png](https://storage.deepin.org/thread/202205081536449227_VirtualBox_deepinlivecdTest_08_05_2022_15_36_25.png)
![image.png](https://storage.deepin.org/thread/202209112203025802_image.png)

## mini 版

在 tiny 版本的基础上，更新并预装了部分原来没有的应用，目前 1.1.0-mini 新预装了 vim、timeshift，将 lights-firefox 升级为 firefox，适用于嫌弃 full 版本空间大以及 tiny 版本功能不全和电脑配置较低以及语言非简体中文的用户
![image.png](https://storage.deepin.org/thread/202209112158229271_image.png)

![VirtualBox_Live CD Test_01_07_2022_20_19_56.png](https://storage.deepin.org/thread/20220701202418495_VirtualBox_LiveCDTest_01_07_2022_20_19_56.png)

![VirtualBox_Live CD Test_01_07_2022_20_24_01.png](https://storage.deepin.org/thread/202207012024198047_VirtualBox_LiveCDTest_01_07_2022_20_24_01.png)
![image.png](https://storage.deepin.org/thread/202209112200295894_image.png)

![VirtualBox_Live CD Test_01_07_2022_20_22_23.png](https://storage.deepin.org/thread/202207012024191528_VirtualBox_LiveCDTest_01_07_2022_20_22_23.png)
![VirtualBox_Live CD Test_01_07_2022_20_20_24.png](https://storage.deepin.org/thread/202207012024185466_VirtualBox_LiveCDTest_01_07_2022_20_20_24.png)

## full 版

（和上面两个版本无关）基于 deepin 20.7 打包，功能较为完整、预装程序也比较多，但无法选择语言（即不能和 tiny、mini 版本一样启动选择语言），如果没有特殊的问题，建议使用这个版本
![image.png](https://storage.deepin.org/thread/202209112148168591_image.png)

![image.png](https://storage.deepin.org/thread/202209112150178582_image.png)

![image.png](https://storage.deepin.org/thread/202209112151255384_image.png)

## install

基于 deepin 20.6 打包，功能非常完整，并未精简过多应用，支持直接运行系官方的安装向导，但无法选择语言（即不能和 tiny、mini 版本一样启动选择语言），以及安装出来的系统会和这个 Live CD 一样被修改过，如果没有特殊的问题，建议使用使用原版镜像进行系统安装
![VirtualBox_Live CD Test_01_07_2022_20_26_44.png](https://storage.deepin.org/thread/202207012027242790_VirtualBox_LiveCDTest_01_07_2022_20_26_44.png)

![VirtualBox_Live CD Test_01_07_2022_20_15_47.png](https://storage.deepin.org/thread/202207012015595089_VirtualBox_LiveCDTest_01_07_2022_20_15_47.png)
![VirtualBox_Live CD Test_01_07_2022_20_17_13.png](https://storage.deepin.org/thread/202207012017254728_VirtualBox_LiveCDTest_01_07_2022_20_17_13.png)
![VirtualBox_Live CD Test_01_07_2022_20_18_48.png](https://storage.deepin.org/thread/202207012018581211_VirtualBox_LiveCDTest_01_07_2022_20_18_48.png)

# 15.11 版

基于 deepin 15.11 制作，满满的回忆
比 full 版本的功能少一点，因为 Deepin 15.11 的底层有些老

![image.png](https://storage.deepin.org/thread/202209112153411297_image.png)

![image.png](https://storage.deepin.org/thread/202209112155289267_image.png)

![image.png](https://storage.deepin.org/thread/202209112157046137_image.png)

# 下载链接

鹤川云盘：https://pan.hechuanyun.xyz/s/Weua

百度网盘：链接: [https://pan.baidu.com/s/1n5J8M8iqfI-kMbmHfR-x9w](https://pan.baidu.com/s/1n5J8M8iqfI-kMbmHfR-x9w) 提取码: ejr7
![image.png](https://storage.deepin.org/thread/202203201435562540_image.png)

阿里云盘（要改后缀名，出问题无法正常下载）：[https://www.aliyundrive.com/s/bfzZhFWCEdi](https://www.aliyundrive.com/s/bfzZhFWCEdi)
和彩云：链接: [https://caiyun.139.com/m/i?0r5CLA9upgtMT](https://caiyun.139.com/m/i?0r5CLA9upgtMT) 提取码:nEy6
![qrcode-分享.jpeg](https://storage.deepin.org/thread/202203201439423300_qrcode-%E5%88%86%E4%BA%AB.jpeg)
天翼云盘：https://cloud.189.cn/t/3IR3m2RrQ7je，访问码:uzl1
![image.png](https://storage.deepin.org/thread/202206051230073378_image.png)
