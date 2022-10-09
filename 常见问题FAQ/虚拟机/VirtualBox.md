---
title: Virtual Box
description: 
published: true
date: 2022-10-09T17:46:28.867Z
tags: virtualbox
editor: markdown
dateCreated: 2022-06-16T02:15:33.016Z
---

Virtual Box（VBox）为开源的跨平台[虚拟机](http://old.deepin.wiki/index.php?title=虚拟机&action=edit&redlink=1)软件。由 Oracle 公司维护。

## 在 deepin 中安装 Virtual Box

1. 从应用商店安装，也可以安装 deepin 仓库中的 `org.virtualbox.virtualbox` 软件包；
2. [终端安装](http://old.deepin.wiki/index.php?title=Apt&action=edit&redlink=1)仓库中的 `virtualbox-6.0` 或 `virtualbox-6.1` 软件包；
3. 前往官方网站[下载](https://www.virtualbox.org/wiki/Linux_Downloads)适用于 Debian 10（当前 deepin 上游版本）或 All distributions 的软件包，进行安装。通过[软件包安装器](http://old.deepin.wiki/index.php?title=软件包安装器&action=edit&redlink=1)安装 [*.deb](http://old.deepin.wiki/index.php?title=Deb) 格式软件包时，会出现需要输入的情况，但不需要手动配置，无视即可。[^1]

需注意，以上三种软件包无法同时安装。

## 问题解答

- 应用商店中安装 Virtual Box 后被安装了 MuPDF / 其他 PDF 软件：

> 应该是某个包依赖一个虚包叫pdf-viewer
>
> 因为系统里没有软件包提供pdf-viewer因此就自动安装提供这个虚包的第一个包org.mupdf
>
> deepin的文档查看器没写 Provides，打包技术有待提高

来源：深度论坛用户 enforcee，[\[使用交流\] 应用商店里的应用也学Windows下的流氓软件了？](https://bbs.deepin.org/post/229035)

## 官方网站

- [Oracle VM VirtualBox](https://www.virtualbox.org/)

[^1]: [\[问题求助\] 升级了新版本 virtualbox因内核版本也要升级, 升级失败](https://bbs.deepin.org/zh/post/226536)