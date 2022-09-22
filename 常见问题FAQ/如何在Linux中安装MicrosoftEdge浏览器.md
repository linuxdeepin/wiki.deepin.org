---
title: 如何在Linux中安装MicrosoftEdge浏览器
description: 
published: true
date: 2022-06-27T08:24:05.342Z
tags: edge
editor: markdown
dateCreated: 2022-06-27T08:18:52.917Z
---

# 如何在Linux中安装MicrosoftEdge浏览器

Edge浏览器最初是在Windows 10上发布的，随后是Mac OS，X Box和Andoird。开发版据说是预览版，旨在让想要在Linux上构建和测试其站点和应用程序的开发人员使用。

![0](https://inews.gtimg.com/newsapp_bt/0/13152103530/1000)

目前尚无法使用网络账户登录，例如Microsoft帐户，到目前为止，Edge仅支持本地帐户。

有两种方法可以在Linux上安装Microsoft Edge浏览器。

从Microsoft Edge网站下载安装包。

使用包管理器安装。

下面介绍使用两种方式安装。

使用.deb或.rpm文件安装Microsoft Edge

首先，从Microsoft Edge Inside网站下载或文件，它将Microsoft仓库添加到系统中，这将自动使Microsoft Edge保持最新版本。

在Ubuntu/Deepin系统中安装

$ wget https://packages.microsoft.com/repos/edge/pool/main/m/microsoft-edge-dev/microsoft-edge-dev_89.0.767.0-1_amd64.deb

$ sudo dpkg -i microsoft-edge-*.deb

点击Edge浏览器的图标就可以启动了。


在Fedora/OpenSUSE系统中安装

`wget https://packages.microsoft.com/yumrepos/edge/microsoft-edge-dev-89.0.767.0-1.x86_64.rpm`

`rpm -ivh microsoft-edge-*.rpm`

使用软件包管理器安装Microsoft Edge

在Ubuntu\Deepin中安装Edge

`curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg`

`sudo install -o root -g root -m 644 microsoft.gpg /etc/apt/trusted.gpg.d/`

`sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/edge stable main" > /etc/apt/sources.list.d/microsoft-edge-dev.list'`

`sudo rm microsoft.gpg`

`sudo apt update`

`sudo apt install microsoft-edge-dev`

在Fedora中安装Edge

`rpm --import https://packages.microsoft.com/keys/microsoft.asc`

`dnf config-manager --add-repo https://packages.microsoft.com/yumrepos/edge`

`mv /etc/yum.repos.d/packages.microsoft.com_yumrepos_edge.repo /etc/yum.repos.d/microsoft-edge-dev.repo`

`dnf install microsoft-edge-dev`

在OpenSUSE中安装Edge

```
$ sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

$ sudo zypper ar https://packages.microsoft.com/yumrepos/edge microsoft-edge-dev

$ sudo zypper refresh

$ sudo zypper install microsoft-edge-dev
```
尽管在Linux中有许多可用的浏览器，但我们必须拭目以待，看看Edge在未来版本中将如何发展。

