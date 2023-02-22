---
title: Kdenlive
description: KDE 出品的开源视频编辑软件
published: true
date: 2023-02-22T09:11:13.466Z
tags: 
editor: markdown
dateCreated: 2022-07-15T16:28:01.975Z
---

## 简介
Kdenlive（KDE Non-Linear Video Editor）是一种基于MLT框架、KDE和Qt的自由开源的非线性视频编辑器，注重灵活性和易用性。

![kdenlive.jpg](/kdenlive.jpg)
## 安装（下面两种二选一即可）
### 官方仓库 20.04 版本
```
sudo apt install org.kdenlive.kdenlive
```

### Kdenlive 官网最新版 22.04.3 版本（AppImage）
```
wget https://download.kde.org/stable/kdenlive/22.04/linux/kdenlive-22.04.3-x86_x64.AppImage
chmod +x kdenlive-22.04.3-x86_x64.AppImage
./kdenlive-22.04.3-x86_x64.AppImage
```

## 卸载
### 官方仓库 20.04 版本
```
sudo apt remove --purge org.kdenlive.kdenlive
```

### Kdenlive 官网最新版 22.04.3 版本（AppImage）
```
rm -f kdenlive-22.04.3-x86_x64.AppImage
```

## 仓库地址
http://packages.deepin.com/deepin/pool/main/k/kdenlive/
## 常见问题
### 软件无法打开、闪退
请检查 Deepin 是否在虚拟机里运行，显卡驱动是否正常
## 相关链接
Kdenlive 官方网站：https://kdenlive.org/zh/
维基百科：https://zh.wikipedia.org/zh-cn/Kdenlive