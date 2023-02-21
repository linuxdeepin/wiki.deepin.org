---
title: Root权限
description: 
published: true
date: 2022-10-17T08:44:09.920Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:41:14.348Z
---

## 前言
root可以指root用户和root权限（例如安卓机中获得root权限）
## root用户
root用户属于root组。root用户是整个deepin中最高权限用户，如果使用者使用root操控电脑，可以修改任何文件。但是并不建议用root用户。这是因为使用root用户，权限过大，会导致一但操作失误，极有可能导致系统文件的损坏，系统无法正常使用。（root用户相当于windows中的adminstrastor用户）
## root权限
root权限即使用root用户时，便拥有了root权限，即电脑的最高权限。
在deepin中，为了保证计算机的安全，是不让用户使用root的。
## 获得root权限
倘若需要root权限，可以执行**下面三条命令中的一条**临时获得root权限：
```
sudo
su - root
sudo -i
```
输入密码后，```sudo ```是以root权限执行命令，而后面两条则是切换到root用户了。