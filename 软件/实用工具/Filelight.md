---
title: Filelight
description: KDE 出品的可视化磁盘用量分析软件
published: true
date: 2022-07-15T15:01:56.489Z
tags: 
editor: markdown
dateCreated: 2022-07-15T14:58:03.230Z
---

## 简介
Filelight 是 KDE 图形磁盘使用分析器，也是KDE Gear应用程序集的一部分。
它没有显示分区或目录中文件的树状视图，甚至没有显示 xdiskusage 之类的列表示目录视图，而是显示了一系列同心饼状图，表示请求的分区或目录中的各种目录及其使用的空间量。这种方法被称为多级饼图、日暴图或环形图。用户也可以在代表特定目录的饼图段上重复对该目录的分析，右击该段以打开该位置的文件管理器或终端模拟器，或复制到剪贴板或删除目录，然后右键单击代表文件的段以打开、复制到剪贴板或删除它。

![filelight.png](/filelight.png)

## 安装
```
sudo apt install filelight
```

## 卸载
```
sudo apt remove --purge filelight
```

## 仓库地址
http://packages.deepin.com/deepin/pool/main/f/filelight/

## 常见问题
## 相关链接
维基百科（英文）：https://en.wikipedia.org/wiki/Filelight
Filelight 官方网站：https://apps.kde.org/zh-cn/filelight/
Filelight Github 主页：https://github.com/KDE/filelight