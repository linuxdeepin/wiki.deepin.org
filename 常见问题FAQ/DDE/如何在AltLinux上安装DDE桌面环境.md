---
title: 如何在ALT Linux上安装DDE桌面环境
description: 
published: true
date: 2022-11-24T09:28:55.714Z
tags: 
editor: markdown
dateCreated: 2022-11-24T09:28:55.714Z
---

# 如何在ALT Linux上安装DDE桌面环境
Your content here

## Alt Linux 镜像下载和安装

正式版下载地址：https://getalt.org
Sisyphus 版下载地址：https://en.altlinux.org/Regular

Sisyphus 是由 ALT Linux 团队开发和维护免费和开源软件存储库，适合作为发行版 LiveCD 构建的基础。 Sisyphus 中的软件往往比较新但不保证稳定性。

Alt Worstation 10 暂时还不能使用 DDE，推荐使用 Sisyphus 版本尝鲜。

## DDE 安装

ALT Linux 是一个使用 apt-get/rpm 进行包管理，首次安装推荐更新系统和仓库。

```bash
apt-get update
apt-get dist-upgrade
```

安装完整 DDE 组件：

```bash
apt-get install deepin-full
```

重启后可以选择进入 DDE

![2022-11-24_65690.png](/2022-11-24_65690.png)





## 参考资料
- [alt wiki](https://www.altlinux.org/Deepin)