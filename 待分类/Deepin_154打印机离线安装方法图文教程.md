---
title: Deepin_154打印机离线安装方法图文教程
description: 
published: true
date: 2022-05-07T07:47:21.819Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:31:44.521Z
---

## 简介

本经验有深度论坛用户(zpu198)分享，[原文地址](https://bbs.deepin.org/forum.php?mod=viewthread&tid=138794&extra=)

## 正文

基本情况：常用办公离不开打印机，而本人所在单位计算机涉密，打印机的离线安装一直不会（本人也是小白，只是deepin粉丝），没事来论坛看看，综合各方面的贴子汇总如下，希望可以帮到像我一样的小白。

参考贴子：<https://bbs.deepin.org/post/138493>

下面以hp p1008为例

准备工作：手动下载plugin

下载地址：
<https://www.openprinting.org/download/printdriver/auxfiles/HP/plugins/>

下载驱动版本号见第2步

![图片](https://storage.deepin.org/forum/201705/02/134957dq5wfpa3w44a4pq4.png)

下这两个文件放到任意自己找得到的地方，在别的机子上装，就复制到别的机子上就行

1. 设置root密码。打开“深度终端”，输入：sudo passwd，然后输入自己用户密码（即登陆密码），重新设置（root、unix)密码。（如果已经设置，请跳过。）

2. 打开终端（DeepinTerminal），输入hp-plugin。在终端的输出中会显示版本号。

P Linux Imaging and Printing System (ver. 3.16.10)这就是在前面要下载的驱动版本号

Plugin Download and Install Utility ver. 2.1

```
Copyright (c) 2001-15 HP Development Company, LP
This software comes with ABSOLUTELY NO WARRANTY.
This is free software, and you are welcome to distribute it
under certain conditions. See COPYING file for more details.


HP Linux Imaging and Printing System (ver. 3.16.10)
Plugin Download and Install Utility ver. 2.1

Copyright (c) 2001-15 HP Development Company, LP
This software comes with ABSOLUTELY NO WARRANTY.
This is free software, and you are welcome to distribute it
under certain conditions. See COPYING file for more details.
```

3. 点yes,
  
4. 选择手动下载plugin 2个文件所在位置
  
5. 选定hplip-3.16.10-plugin.run
  
6. 点next,

7. 点yes,
  
8. i agree  前打勾，
  
9. 点next,
  
10. 输入第一步设置的root 密玛
  
11. 点ok，
  
12. 点OK 就安装完成，可以添加打印机了

13. 点添加

14. 点forward
  
15. 点apply后
  
16. 点打印测试页，应该 成功了吧
