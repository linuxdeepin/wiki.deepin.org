---
title: DTK
description: DTK
published: true
date: 2022-05-05T10:23:47.536Z
tags: dtk
editor: markdown
dateCreated: 2022-05-05T10:23:47.536Z
---

# DTK
## 简介
DTK (deepin tool kit) 是基于Qt5开发的一整套UI图形库，方便统一的编写深度桌面和深度系列应用，主要的功能有：

提供单实例的接口，方便直接使用，不用造轮子
提供XCB窗口移动、缩放等一系列函数，无边框的窗口不用自己折腾几大本X11/XCB 的书了，开发者全部都做好了
提供一大票美观的自绘控件，不用自己造Qt控件了，拉着直接用

DTK的GitHub：https://github.com/linuxdeepin/deepin-tool-kit

作者：ManateeLazyCat
链接：http://www.jianshu.com/p/e871723f9460
來源：简书

## dtkwidget
本段编辑于 2017/10/06，相关内容后续或有变化。

DTK 已于 2017 年 7 月完成重构，相关：https://github.com/linuxdeepin/deepin-tool-kit/issues/8

dtkwidget是控件库，包括所有系统和应用的控件。dtkcore是一些非控件的工具栏。github上的dtk仓库已经只是个虚拟仓库了 (https://bbs.deepin.org/forum.php?mod=viewthread&tid=146363&page=1#pid385967)
如果要用 QT 写出原生风格的程序，使用 dtkwidget 套件即可。

步骤：
(注意：即使你是在 deepin 系统下，想要使用 dtkwidget 仍然需要经过以下步骤)
1. 从应用商店安装 Qt Creator;
2. 从 github 上下载 dtkwidget：https://github.com/linuxdeepin/dtkwidget
3. 解压 dtkwidget，解压的 dtkwidget 目录下有两个文件需要注意。分别是 README.md、debian 文件夹下的 control 文件
4. 打开 README.md 文件，按照 `### Build from source code` 这一部分的操作安装 dtgkwidget
5. 第一步："Make sure you have installed all dependencies."。打开 control 文件，这里（Build-Depends: ）列出了需要安装的依赖包，请把这些都安装上（相信你不会忘了 sudo apt-get install *** 这个命令）
6. 第二步："Build:"。将文件编译出来，这一步需要多花点时间
7. 第三步："Install:"。安装 dtkwidget
8. 打开 Qt Creator，用 Qt Creator 打开 dtkwidget 文件夹下的 examples 文件夹下的工程，编译并运行该工程
9. 学习 examples 程序并开发你自己的 deepin 风格程序

## 整套DTK
具体编译、安装方法已有详细教程，请参考：
[编译打包最新的DTK](https://github.com/rekols/docs/wiki/%E7%BC%96%E8%AF%91%E6%89%93%E5%8C%85%E6%9C%80%E6%96%B0%E7%9A%84DTK)