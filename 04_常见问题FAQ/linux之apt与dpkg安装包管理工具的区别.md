---
title: Linux之apt与dpkg安装包管理工具的区别
description: apt 与 dpkg 均为 ubuntu 下面的包管理工具。 dpkg 侧重于本地软件的管理。 apt 基于dpkg，侧重于远程包的下载和依赖管理，相当于 dpkg 的前端。
published: true
date: 2023-03-06T23:01:53.878Z
tags: apt
editor: markdown
dateCreated: 2023-03-06T02:23:36.690Z
---

## 主要区别

**dpkg 仅用于安装本地的软件包，安装时不会安装依赖包，不解决依赖问题。**
```bash
sudo dpkg -i <package_name>.deb
```

**apt 默认会从远程仓库搜索包的名字，下载并安装，安装时会自动安装依赖包，并解决依赖问题**
```bash
sudo apt install <package_name>
```

**如果需要使用apt 从本地安装，需要在包名前指定路径，否则只从远程仓库查找**
```bash
sudo apt install <path>/<package_name>.deb
```

## dpkg 的常用命令

查看指定包的版本，架构和描述信息
```bash
dpkg -l <package_name>
# 或
dpkg --list <package_name>
```

列出所有已安装的包，和其版本，架构和描述信
```bash
dpkg -l
```

相当于
```bash
apt list --installed
```

查看包的安装路径
```bash
dpkg -L <package_name>
```

查看包是否安装
```bash
dpkg -s <package_name>
# 或
dpkg --status <package_name>
```

## apt 常用命令

更新包信息
```bash
sudo apt update
```

根据包信息升级包
```bash
sudo apt upgrade
```

安装包
```bash
sudo apt install <package_name>
```

删除不再需要的依赖包
```bash
sudo apt autoremove
```