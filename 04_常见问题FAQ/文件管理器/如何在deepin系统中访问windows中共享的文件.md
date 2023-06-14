---
title: 如何在deepin系统中访问windows中共享的文件
description: 不同系统间的文件共享
published: true
date: 2023-06-14T07:21:20.002Z
tags: deepin, windows, 文件共享
editor: markdown
dateCreated: 2023-06-14T07:21:20.002Z
---

# 如何在deepin系统中访问windows11中共享的文件
本文借鉴以下文章:
[Win11系统如何更改本地账户或微软账户登录？Win11系统更改本地账户或微软账户登录方法](https://zhuanlan.zhihu.com/p/622473782)
[Windows11 文件夹共享设置 如何设置 如何访问](https://zhuanlan.zhihu.com/p/421631878)
 ### 一、windows中共享文件夹
1. windows11系统中使用本地账号登录
> 若当前为本地账户登录Windows，则忽略此步骤。
{.is-info}

![改用本地用户登录.jpg](/for_trans/共享文件/改用本地用户登录.jpg)
![账户名设置.webp](/for_trans/共享文件/账户名设置.webp)
需记住该账户名和密码。

2. 在资源管理器中设置需要共享的文件夹

右键点击需要共享的文件夹，选择“更多选项”-“属性”
![gongxiang.jpg](/for_trans/共享文件/gongxiang.jpg)

3. 查看windows系统IP地址：
![ip地址.webp](/for_trans/共享文件/ip地址.webp)
### 二、deepin系统中访问windows中共享的文件
打开文件管理器，搜索框中输入`smb://+Windows系统的ip地址`，如下图：
![smb.jpg](/for_trans/共享文件/smb.jpg)
输入用户名和密码，回车即可访问windows共享的文件，如下图：
![用户名和密码.jpg](/for_trans/共享文件/用户名和密码.jpg)