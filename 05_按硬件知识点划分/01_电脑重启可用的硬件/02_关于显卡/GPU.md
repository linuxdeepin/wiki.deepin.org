---
title: GPU
description: 
published: true
date: 2022-12-01T14:58:29.793Z
tags: 
editor: markdown
dateCreated: 2022-05-05T04:19:16.888Z
---

# GPU
## GPU简介
GPU是计算机的图形处理器，负责处理计算机的图像数据以及做一些大型计算工作
## GPU架构
目前主流的GPU架构有AMD的RDNA2架构，以及NVIDIA的安培架构
从早期的Maxwell到最新的ADA架构，每一代GPU架构都进行了很多优化
## GPU驱动
众所周知，NVIDIA对Linux的驱动支持一直很不好，2022年上半年，NVIDIA才将RTX30系列的GPU驱动开源，未来Linux的N卡驱动会基于优化。deepin可以在安装时选择自动安装N卡闭源驱动，方便大家使用
AMD显卡驱动对Linux支持很好，驱动程序构建在Linux内核里，部分独立显卡需要在AMD官网下载开源驱动进行安装
https://www.amd.com/zh-hans/support/linux-drivers

## GPU计算
随着程序对计算机算力要求的不断提升，普通CPU无法快速处理复杂的数据计算，显卡在这时就可以发挥很好的功能