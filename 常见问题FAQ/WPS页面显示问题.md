---
title: WPS页面显示问题
description: 相关论坛帖子链接：https://bbs.deepin.org/zh/post/238699；https://bbs.deepin.org/zh/post/240463
published: true
date: 2022-09-06T07:27:57.127Z
tags: wps, 页面显示
editor: markdown
dateCreated: 2022-09-06T07:26:03.838Z
---

# WPS表格显示问题
感谢论坛用户 hotime 提供的解决方法
### 1、问题描述
如下图wps中，橫行ABCD...和数列1234...显示不全的问题：
![2022-9-6_84457.png](/2022-9-6_84457.png)
### 2、问题产生原因
这是因为WPS表格的默认字体为宋体, 而deepin系统没有宋体, 所以导致的异常。
### 3、问题解决思路
1. 你可以从Windows系统下拷贝宋体字体文件C:\Windows\Fonts\simsun.ttc以及simsunb.ttf到deepin，然后安装。

2. 或者修改wps表格的默认字体: 在WPS表格中点击 文件 - 选项 - 常规与保存 - 标准字体, 将默认的正文字体修改为deepin系统中自带的字体, 例如Noto Sans CJK SC Regular, 保存后重新新建wps表格, 就可以看到正常的显示效果了。
![2022-9-6_33264.png](/2022-9-6_33264.png)
