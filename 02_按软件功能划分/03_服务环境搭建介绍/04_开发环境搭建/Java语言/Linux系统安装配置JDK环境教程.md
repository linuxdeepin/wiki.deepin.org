---
title: Linux系统安装配置JDK环境教程
description: 
published: true
date: 2022-10-25T02:25:07.805Z
tags: java
editor: markdown
dateCreated: 2022-06-28T06:29:12.278Z
---

# Linux系统安装配置JDK环境教程
~~jdk下载：[https://cway.top/post/696.html](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9jd2F5LnRvcC9wb3N0LzY5Ni5odG1s)~~
openjdk 下载地址[openjdk 18](https://jdk.java.net/18/)

1. 下载jdk
```
wget https://download.java.net/java/GA/jdk18.0.1.1/65ae32619e2f40f3a9af3af1851d6e19/2/GPL/openjdk-18.0.1.1_linux-x64_bin.tar.gz
```
将jdk解压到指定位置比如`/home/$USER/jdk-18/`
2. 配置环境变量

```bash
# 解压到系统全局环境变量
sudo vim /etc/profile

#解压到个人下，个人环境变量
vim .bashrc # bash

vim .zshrc # zsh
```

末尾加以下代码保存即可，JAVA_HOME根据实际解压安装路径而定`$(PATH_TO_JAVA_INSTALL_DIR)`位jdk实际解压位置，CLASSPATH在jdk8以上时无需设置。

```bash
export JAVA_HOME=$(PATH_TO_JAVA_INSTALL_DIR)
export PATH=$PATH:$JAVA_HOME/bin
```

`:wq`保存退出

```bash
# 系统级环境变量
source /etc/profile

# 个人级环境变量
source .bashrc # bash
source .zshrc # zsh
```

