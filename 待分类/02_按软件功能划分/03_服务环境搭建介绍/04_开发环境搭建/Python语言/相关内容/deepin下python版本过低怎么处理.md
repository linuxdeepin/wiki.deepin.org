---
title: deepin下python版本过低如何处理
description: 
published: true
date: 2022-10-25T07:02:05.044Z
tags: deepin, python
editor: markdown
dateCreated: 2022-07-06T09:51:20.989Z
---

# deepin下安装python 3.9.0

1、依次执行以下命令，安装相关需要的组件
```
sudo apt update
sudo apt install make build-essential libssl-dev zlib1g-dev liblzma-dev
Sudo apt install libbz2-dev libreadline-dev libsqlite3-dev llvm
sudo apt install libncurses5-dev libncursesw5-dev xz-utils tk-dev
```
2、到Python官网下载最新的安装包[https://www.python.org/downloads/](https://www.python.org/downloads/)

3、解压缩安装包
```
tar -xvf Python-3.9.0.tar.xz
```
4、进入安装目录
```
cd Python-3.9.0
```
5、执行configure配置构件文件
```
./configure --enable-optimizations --with-ssl
```
6、编译
```
make -j8
```
7、安装
```
sudo make altinstall
```
8、安装后的清理
```
sudo make clean
sudo apt autoremove
```
9、检查版本是否安装正确
```
python3.9 -V
```
Python 3.9.0，即安装完成。


[原文链接：https://www.fungj.com/information/deepin-v20-installing-python.html](https://www.fungj.com/information/deepin-v20-installing-python.html)