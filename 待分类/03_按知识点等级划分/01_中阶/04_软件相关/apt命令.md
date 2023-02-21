---
title: apt命令
description: 日常使用的一些操作命令
published: true
date: 2022-10-18T02:35:02.306Z
tags: 终端 命令
editor: markdown
dateCreated: 2022-06-13T05:43:53.041Z
---

## 主要记录了日常使用较高频率的终端操作命令,供大家查询使用
1. 相关命令如果不了解，强烈建议复制百度后再使用
1. 命令要在输入法英文模式下输入
1. 一些命令执行权限不够的情况下，命令前面加sudo
1. sudo输入密码的时候不显示，输完密码直接按Enter(回车)执行

# 更新

&& 两条命令一同输入之间的连接符号，其前后有空格
`sudo apt-get update`更新软件源中的所有软件列表
`sudo apt-get upgrade` 更新软件。 
`sudo apt-get dist-upgrade`更新系统版本
通常apt的更新是使用两条命令完成的
`sudo apt-get -f install`修复安装”-f = –fix-missing”

`sudo apt-get update && sudo apt-get dist-upgrade -y`
`sudo apt-get update && sudo apt dist-upgrade -y`
`sudo apt-get -f install`
`sudo dpkg --configure -a && sudo apt -f install`
`sudo apt --fix-broken install`
`sudo apt update && sudo apt full-upgrade`
`apt list --upgradable`
`sudo apt full-upgrade`


# 卸载
`apt-get remove`会删除软件包而保留软件的配置文件
`apt-get purge`会同时清除软件包和软件的配置文件
`sudo apt remove linux-image`  
`sudo apt autoremove` 包名 命令即可卸载上面的那些包
还有没有删除干净的内容可以用`sudo apt-get autoremove`来清理

remove – 卸载软件包
autoremove – 卸载所有自动安装且不再使用的软件包
purge – 卸载并清除软件包的配置

这里重点介绍一下autoremove：
apt-get autoremove的行为重点是卸载所有自动安装，
举个栗子：C 依赖于 B, D 依赖于B, 且D没有被其他手动安装的包依赖
apt-get remove C将删除C,同时提示你用apt-get autoremove去清除B,D
apt-get autoremove C 将删除B, C, D。
所以，这条命令最恐怖的是在不了解的情况下，你不知道他会把系统中的什么配置文件给删除，导致需要重装系统的风险

