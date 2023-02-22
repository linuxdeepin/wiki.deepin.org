---
title: Earlyoom
description: earlyoom 在系统 OOM 之前挽救系统
published: true
date: 2023-02-02T06:03:46.318Z
tags: 
editor: markdown
dateCreated: 2023-02-02T06:03:43.384Z
---

# earlyoom 让系统再也不会因为内存不足而卡死

> 默认情况下剩余可用物理内存小于 10% 和剩余交换内存小于 10% 时，就会触发早期OOM
{.is-info}

## 安装代码
```
# 直接 apt 安装
apt install earlyoom

# 设定为开机启动，并且马上启动 earlyoom 服务
systemctl enable --now earlyoom
```





项目地址: https://github.com/rfjakob/earlyoom
参考博客: [CSDN 链接](https://blog.csdn.net/ONE_SIX_MIX/article/details/124046026)
