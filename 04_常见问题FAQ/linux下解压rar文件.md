---
title: linux下解压rar文件
description: 
published: true
date: 2023-06-06T08:05:45.187Z
tags: 
editor: markdown
dateCreated: 2023-06-06T08:00:51.166Z
---

# linux下解压rar文件
1、linux中常常会遇到一些rar结尾的文件包，靠linux本身的命令是无法实现解压rar结尾的文件夹的，需要安装rar的压缩软件才可以。
2、要将服务器的账号切换为root账户，否则安装会出错。

## 1、下载linux版本的rar软件
访问官网 下载最新的、适用于自己的linux版本的rar软件。https://www.rarlab.com/download.htm
可以通过命令getconf LONG_BIT查看自己的linux服务器的字长。我的是64位的，就下载图示箭头所指的版本
![2023-6-6_93983.png](/2023-6-6_93983.png)
## 2、解压下载好的rar软件
首先通过cd命令，进入这个放rar软件的文件夹，之后通过下面的命令，解压这个软件
```
tar -xzpvf rarlinux-x64-612.tar.gz 
```
## 3、编译安装
解压结束之后，在当前文件夹下会多出一个文件夹rar，通过命令cd rar，进入这个文件夹，之后使用命令make，安装软件
此处注意，如果不是在root账户下，make命令会出错，访问被拒绝。

## 4、解压rar结尾的文件
使用cd命令进入rar文件所在的文件夹，之后使用命令
```
rar x XXX.rar 
```
然后就可以解压XXX.rar文件啦