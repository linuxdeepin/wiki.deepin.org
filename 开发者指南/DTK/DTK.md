---
title: DTK
description: DTK
published: true
date: 2022-09-23T16:01:28.160Z
tags: dtk
editor: markdown
dateCreated: 2022-05-05T10:23:47.536Z
---

# DTK
## 简介
DTK (deepin tool kit) 是基于Qt5开发的一整套UI图形库，方便编写风格统一的深度桌面和深度系列应用，主要的功能有：

- 提供单实例的接口，方便直接使用，不用重复造轮子。
- 提供XCB窗口移动、缩放等一系列函数，无边框的窗口。
- 提供美观的自绘控件，直接拖拽使用。

DTK链接：https://github.com/orgs/linuxdeepin/repositories?q=dtk

**DTK核心仓库：**
- **dtkcore:** https://github.com/linuxdeepin/dtkcore
- **dtkgui:** https://github.com/linuxdeepin/dtkgui
- **dtkwidget:** https://github.com/linuxdeepin/dtkwidget
- **qt5integration:** https://github.com/linuxdeepin/qt5integration

> 若要从源码编译、安装dtk组件，请按照dtkcore、dtkgui、dtkwidget的顺序编译，且保证dktcore、dtkgui、dtkwidget的版本一致。
{.is-warning}

> 请勿使用master版本编译，源码编译风险请自行承担!!
{.is-danger}




## dtkcore
dtkcore是DTK的核心组件，等同于Qt5中的core组件。Deepin系统默认安装该组件，如需要重新安装请打开终端输入以下命令：
> sudo apt install libdtkcore5 --reinstall
{.is-info}

若从源码编译则需要遵循以下步骤：
> `1.git clone -b [tags] https://github.com/linuxdeepin/dtkcore.git`
> `2.cd dtkcore`
> `3.mkdir build && cd build`
> `4.sudo apt build-dep ../`
> `6.cmake ..`
> `7.make`
{.is-info}

若编译完成后需要安装有两种可选方案：
> `1.sudo make install #源码安装`
> `2.debuild -us -uc -b #打包成deb包可分享给他人`
{.is-info}


## dtkgui
dtkgui是DTK的图形核心组件，等同于Qt5中的gui组件。Deepin系统默认安装该组件，如需要重新安装请打开终端输入以下命令：
> sudo apt install libdtkgui5 --reinstall
{.is-info}

若从源码编译则需要遵循以下步骤：
> `1.git clone -b [tags] https://github.com/linuxdeepin/dtkgui.git`
> `2.cd dtkgui`
> `3.mkdir build && cd build`
> `4.sudo apt build-dep ../`
> `6.cmake ..`
> `7.make`
{.is-info}

若编译完成后需要安装有两种可选方案：
> `1.sudo make install #源码安装`
> `2.debuild -us -uc -b #打包成deb包可分享给他人`
{.is-info}

## dtkwidget
dtkwidget是DTK的核心组件，等同于Qt5中的widget组件。Deepin系统默认安装该组件，如需要重新安装请打开终端输入以下命令：
> sudo apt install libdtkwidget5 --reinstall
{.is-info}

若从源码编译则需要遵循以下步骤：
> `1.git clone -b [tags] https://github.com/linuxdeepin/dtkwidget.git`
> `2.cd dtkwidget`
> `3.mkdir build && cd build`
> `4.sudo apt build-dep ../`
> `6.cmake ..`
> `7.make`
{.is-info}

若编译完成后需要安装有两种可选方案：
> `1.sudo make install #源码安装`
> `2.debuild -us -uc -b #打包成deb包可分享给他人`
{.is-info}

## qt5integration
dtkwidget是DTK的插件组件，等同于Qt5中的plugin组件。Deepin系统默认安装该组件，如需要重新安装请打开终端输入以下命令：
> sudo apt install qt5integration --reinstall
{.is-info}

若从源码编译则需要遵循以下步骤：
> `1.git clone -b [tags] https://github.com/linuxdeepin/qt5integration.git`
> `2.cd qt5integration`
> `3.mkdir build && cd build`
> `4.sudo apt build-dep ../`
> `6.qmake ..`
> `7.make`
{.is-info}

若编译完成后需要安装有两种可选方案：
> `1.sudo make install #源码安装`
> `2.debuild -us -uc -b #打包成deb包可分享给他人`
{.is-info}


