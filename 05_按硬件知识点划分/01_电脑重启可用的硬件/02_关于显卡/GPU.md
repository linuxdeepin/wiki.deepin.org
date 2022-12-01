---
title: GPU
description: 
published: true
date: 2022-12-01T15:09:27.189Z
tags: 
editor: markdown
dateCreated: 2022-05-05T04:19:16.888Z
---

# GPU
## GPU简介
GPU是计算机的图形处理器，负责处理计算机的图像数据以及做一些大型计算工作

## GPU架构
目前主流的GPU架构有AMD的RDNA2架构，以及NVIDIA的安培架构

英特尔也在2022年下半年推出了自家的A系列独立显卡

从早期的Maxwell到最新的ADA架构，每一代GPU架构都进行了很多优化

## GPU驱动
众所周知，NVIDIA对Linux的驱动支持一直很不好，2022年上半年，NVIDIA才将RTX30系列的GPU驱动开源，未来Linux的N卡驱动会基于优化。deepin可以在安装时选择自动安装N卡闭源驱动，方便大家使用
AMD显卡驱动对Linux支持很好，驱动程序构建在Linux内核里，部分独立显卡需要在AMD官网下载开源驱动进行安装
https://www.amd.com/zh-hans/support/linux-drivers
英特尔独占也为Linux提供了良好的驱动支持
https://www.intel.cn/content/www/cn/zh/support/articles/000005520/graphics.html
英伟达也为大多数显卡提供了闭源驱动
https://www.nvidia.cn/geforce/drivers/

## GPU计算
随着程序对计算机算力要求的不断提升，普通CPU无法快速处理复杂的数据计算，显卡在这时就可以发挥很好的功能

GPU计算是使用GPU（图形处理单元）作为协处理器来加速CPU，以加快程序的运行速度，GPU计算加速器于2007年由英伟达率先推出。

CPU由4到8个CPU内核组成，而GPU由数百个较小的内核组成。它们共同运作以应对应用程序中的数据，这种大规模并行架构为GPU提供了高计算性能。