---
title: Deepin_安装oracle12c过程
description: 
published: true
date: 2022-05-11T15:44:40.022Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:32:09.781Z
---

## 简介
本文的经验来自深度百科用户(873792861)分享。

[原文地址](https://bbs.deepin.org/forum.php?mod=viewthread&tid=43149)

## 正文

最近，想学习下oracle，于是，在网上查找相关资料，在oracle官网下载了oracle12c。
1. 建立软链接

    ```
    mkdir /usr/lib64
    ln -s /usr/bin/awk /bin/awk
    ln -s /usr/bin/basename /bin/basename
    ln -s /usr/bin/rpm /bin/rpm
    ln -s /etc /etc/rc.d
    ln -s /usr/lib/x86_64-linux-gnu/libpthread_nonshared.a /usr/lib64/
    ln -s /usr/lib/x86_64-linux-gnu/libc_nonshared.a /usr/lib64/
    ln -s /lib/x86_64-linux-gnu/libgcc_s.so.1 /lib
    ln -s /usr/lib/x86_64-linux-gnu/libstdc++.so.6 /usr/lib64/
    ```

2. 调整参数，limits.conf和sysctl.conf 我已经上传，只需要把这两个文本里的内容追加进/etc/security/limits.conf 和/etc/sysctl.conf

3. 安装所需依赖

    ```
    apt-get install libaio-dev sysstat unixodbc-dev libelf-dev unzip g++ zlib1g-dev
    ```

4. 建立组和用户，然后用passwd命令设立用户密码

    ```
    groupadd dba
    useradd -d /home/oracle -m -c "Oracle Database" -g dba -G sudo -s `which bash` oracle
    ```

5. 关于安装目录，安装过程中会让选择目录，默认是在/home/oracle/app，我安装的时候是选择默认的这个目录，所以就无需另外建立目录了

6. 把软件解压到家目录下，切换到oracle用户，cd到软件解压所在的目录

    ```
    su - oracle
    cd database/
    xhost +
    ```

	因为oracle安装程序自带的JDK指定要一种系统没有的字体,所以要在运行安装命令前，设置语言为英文,命令行运行 export LANG=en_us 。

	要显示中文，那么：

	database/stage/Components/oracle.jdk/1.6.0.75.0/1/DataFile/ 下面有filegroup1.jar 这个文件

	用”归档管理器”打开 filegroup1.jar, 在jdk/jre/lib/fonts 目录下创建fallback

	然后将中文字体,比如 zysong.ttf(中易宋体) 或者 simsun.ttc(微软的宋体) 拖到jdk/jre/lib/fonts/fallback 这个目录，没有这些目录就自己建，把字体放进文件夹，然后添加进去。我把添加后的文件也上传了，复制替换就行了。

	接下来，运行安装脚本

    ```
    ./runInstaller
    ```

7. 安装图形界面选择

	“install the database software only” 只安装数据库软件

	“single instance database installation” 只安装单个数据库实例, 不搞什么集群,分布式,那些太高端了

	“Enterprise Edition” 企业版

	如果你是选择Create and configure a databaseand，然后又勾上自动管理内存，那么最大内存MEMROY_MAX_TARGET不能大于dev/shm。

	然后按照提示，复制脚本路径以，root用户执行。

8. 安装完后，先设置环境变量。修改家目录的.profile文件，添加

    ```
    export ORACLE_BASE=/home/oracle/app/oracle  
    export ORACLE_HOME=/home/oracle/app/oracle/product/12.1.0/dbhome_1
    export PATH=$PATH:$ORACLE_HOME/bin
    export ORACLE_SID=myora
    ```

	然后命令行启动dbca，建立数据库myora（这里是自己起的名字，环境变量的ORACLE_SID的名字和这个一样）。

9. 如果要安装sqldeveloper，官网只有rpm的安装包，需要先安装alien 和 fakeroot 这两个工具，用法：

    ```
    fakeroot alien package.rpm
    ```

	并下载jdk,解压，并设置环境变量。
