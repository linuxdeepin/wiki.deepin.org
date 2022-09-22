---
title: 华为AMD笔记本，无显卡驱动解决方法
description: 华为AMD笔记本，无显卡驱动解决方法
published: true
date: 2022-06-27T07:33:05.587Z
tags: amd 华为笔记本 显卡驱动
editor: markdown
dateCreated: 2022-06-27T07:33:03.524Z
---

# 无显卡驱动的现象
**遇到一个用户情况如下：**
> 用户反馈显卡驱动看不到，且待机唤醒有问题，显示适配器无内容。


设备管理器中无法展示具体显卡信息：
![设备管理器无显卡信息.png](/图片存储/设备管理器无显卡信息.png)
可以看到显卡是无驱动信息的：
![无驱动.png](/图片存储/无驱动.png)
## 解决过程
- 用户安装的时候选择的是5.15版本内核，然后切换回5.10版本内核仍旧没有作用。
**最后通过如下方式解决了该问题：**

AMD 核显驱动支持
1. 编辑 grub 启动目录
```bash
sudo vi /etc/default/grub
```
2. 在 ```GRUB_CMDLINE_LINUX_DEFAULT=```一行双引号内的末尾，添加 ```amdgpu.exp_hw_support=1```，修改后如下
```GRUB_CMDLINE_LINUX_DEFAULT="quiet splash amdgpu.exp_hw_support=1"```
3. 保存退出，并更新 grub
```bash
sudo update-grub
```
4. 重启

[参考连接可点击这里](https://blog.csdn.net/tankpanv/article/details/119337770)