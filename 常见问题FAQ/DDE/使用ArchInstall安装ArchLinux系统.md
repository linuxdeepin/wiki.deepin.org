---
title: 使用ArchInstall安装ArchLinux系统
description: 
published: true
date: 2022-09-14T02:27:28.705Z
tags: arch
editor: markdown
dateCreated: 2022-09-14T02:18:42.542Z
---

# 使用ArchInstall安装ArchLinux系统
`archinstall`是一个用于安装 Arch Linux 的帮助库。它和其它的预配置安装程序一起打包

`archinstall`以纯文本形式存储所有用户和（辅助）磁盘加密的密码
`archinstall`的默认配置与安装指南不同。如使用 archinstall 安装系统出现问题的话，请在反馈中注明，并提供`/var/log/archinstall/install.log`

## 运行安装程序
首先，按照安装指南#安装前的准备中的启动到 Live 环境操作。archinstall 包是 live 镜像的一部分，可以直接运行：

指导程序将执行（或查询）以下步骤：
```
配置区域
选择镜像
分区磁盘
格式化分区
启用磁盘加密（可选）
设置计算机名
设置 root 密码
安装引导加载程序
```


