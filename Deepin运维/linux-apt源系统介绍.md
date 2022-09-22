---
title: linux-apt源系统介绍
description: 
published: true
date: 2022-06-28T07:54:39.074Z
tags: apt
editor: markdown
dateCreated: 2022-06-28T07:54:36.665Z
---

# linux apt 源系统介绍
# linux apt 源系统介绍

## 前言

linux用户要安装程序怎么办？一、类似windows用户那样去下载独立的安装程序，比如.deb的安装包。二、使用一个在线的软件仓库，在debian系中这个工具叫apt。只要有apt，并且连上网络，就可以使用几个简单的命令，安装需要的程序（如果源仓库有）。但是，接触apt越久，就会碰到许多奇奇怪怪的报错。作为一个专业用户，是不可能不去研究apt系统的构成的，否则连软件安装问题都解决不了，如何能愉快的玩耍？

## gpg key

apt使用gpg验证源的安全和稳定，这是一个基于公钥私钥体系的密码系统。第一步就是下载某个源仓库对应的key，安装进apt系统里面，然后才能使用该源仓库。比如deepin的key在：<https://github.com/linuxdeepin/deepin-keyring/tree/master/keyrings>

1. `sudo apt install gnupg` #安装gpg
2. 下载key
3. `sudo apt-key add this.key`  #安装key
4. 编辑源地址：`sudo nano -w /etc/apt/sources.list` 或在/etc/apt/sources.list.d/添加一个新的.list文件，独立配置一个源仓库地址。
5. `sudo apt update` 更新仓库信息到本地

当然，系统默认就会自带当前发行版的key，这是对第三方仓库而言的操作流程。

## 仓库地址的基本构成

比如地址：`deb http://mirrors.tuna.tsinghua.edu.cn/deepin panda main contrib non-free` 

1. `deb`表示二进制包
2. `http://mirrors.tuna.tsinghua.edu.cn/deepin` 是一个路径，可以用浏览器打开，以此为根目录
3. `panda` 是发行版，位于/dists/panda
4. `main contrib non-free` 是三个子仓库，分别位于/dists/panda/main、.../.../contrib、.../.../non-fre
5. 仓库的信息在/.../panda/Release 文件中记录，该信息可以用`apt policy` 获取:
   1. origin 仓库名： `o=Linux Deepin` 来源
   2. label 标签 ：`l=Deepin` 标签
   3. suite 发行版 ：`a=panda` 存档
   4. codename 代号 ：`n=unstable` 
   5. version 版本 ：`v=2015` 版本
   6. architectures 架构 ：`b=adm64`
   7. components 子仓库 ：`c=main` 部件
   8. 子部件的md5sum
6. deepin 是基于debian，而debian也有自己的发行版本，但是codename和debian似乎保持了相对一致，即unstable(现panda) 对应debian的unstable（激进版），stable（现lion)对应debian的stable（稳定版），因此仓库地址也可以改为：`deb http://mirrors.tuna.tsinghua.edu.cn/deepin unstable main contrib non-free`，可惜并没有位于中间的testing版。

## apt 工作流程

1. 读取源地址，处理依赖关系
   1. `apt -s install xxx` 模拟安装过程 -s
2. 下载包到： /var/cache/apt/archives/
   1. `apt autoclean` 删除缓存的包
   2. `apt -d install xxx` 只下载
3. 仓库软件信息：/var/lib/apt/lists/
4. 可用包信息： /var/lib/dpkg/available
5. 包安装状态`信息： /var/lib/dpkg/status
   1. dpkg --list
6. 已安装包的文件分布：`dpkg -L xxx`
   1. `dpkg -S /bin/ls` 查询特定文件属于那个包，返回coreutils

## apt 仓库布局

### 远程仓库

例子：http://deb.debian.org/debian/dists/unstable

- /Release 描述信息
- /Release.gpg 签名
- /Contents-<架构> 文件列表

例子：http://deb.debian.org/debian/dists/unstable/main/binary-amd64/

- .../Release 描述信息
- .../Packages 二进制包
- .../Sources 源代码包

### 本地

例子：

- /var/lib/apt/lists/deb.debian.org_debian_dists_unstable_Release
- /var/lib/apt/lists/deb.debian.org_debian_dists_unstable_mian_binary-amd64_Packages

安装状态：

- /var/lib/apt/extended_states 自动安装包信息
- /var/cache/apt/archives    包缓存


## 参考

1. 深入 APT 系统: <https://blog.csdn.net/comcat/article/details/1549559>