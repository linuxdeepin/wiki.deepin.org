---
title: LAMP服务
description: 
published: true
date: 2022-05-07T07:48:16.060Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:37:13.061Z
---

## 前言
LAMP是Linux web服务器组合套装的缩写，分别是Apache+MySQL+PHP。

## 安装

### 安装LAMP开发环境

深度商店搜索lamp安装

命令安装，终端执行：

    sudo apt-get install lamp

编译安装

下载tra.gz包解压后

    ./configure 
    sudo make && make install

这种方法不是很好，因为会涉及到许多依赖包、子库和模块，费时费力，不熟悉的不一定安装成功。

### Deb安装

下载各Deb包安装

### 手工配置安装

利用apt-get，它会自动检测额外库和模块进行安装。 我们用第三种方法安装，安装全过程不到10分钟。

安装 MySQL 5.6

    sudo apt-get install mysql-server-5.6 mysql-client-5.6

安装过程中需要设置root账户密码，系统会作以下提示：

    New password for the MySQL "root" user:Repeat password for the MySQL "root" user:

安装 Apache2

    sudo apt-get install apache2

在浏览器输入你服务器地址列入

    http://127.0.0.1

查看Apache2是否工作，如果显示(It works!)，说明已经工作。

Apache 默认文档根目录为 /var/www，配置文件 /etc/apache2/apache2.conf，额外配置存储子目录 /etc/apache2 例如 /etc/apache2/mods-enabled (为 Apache 模块), /etc/apache2/sites-enabled (为虚拟主机 virtual hosts), 和 /etc/apache2/conf.d.

安装 PHP5 和 Apache PHP5 模块:

     sudo apt-get install php5 php5-mysql libapache2-mod-php5

然后重启apache:

    sudo /etc/init.d/apache2 restart

测试 PHP5{可忽略此步}

    sudo gedit /var/www/info.php

输入下面的内容：{可忽略此步}

     <?php
    phpinfo();
     ?>

然后打开浏览器访问

    http://127.0.0.1/info.php {可忽略此步}

你可以看到一些已经支持的模块。

然后安装所需模块，例如下面的命令：

     sudo apt-get install php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl

重启 Apache2:

     sudo /etc/init.d/apache2 restart

然后刷新

    http://127.0.0.1/info.php {可忽略此步}

查看模块支持是不是已经增加了。

安装phpmyadmin来管理mysql:

    sudo apt-get install phpmyadmin

在安装过程中会要求选择Web server：apache2或lighttpd，选择apache2，按tab键然后确定。然后会要求输入设置的Mysql数据库密码连接密码 Password of the database's administrative user。

然后将phpmyadmin与apache2建立连接，以我的为例：www目录在/var/www，phpmyadmin在/usr/share /phpmyadmin目录，所以就用下面的命令建立连接。

    sudo ln -s /usr/share/phpmyadmin /var/www 

phpmyadmin测试：在浏览器地址栏中打开<http://127.0.0.1/phpmyadmin> 自此，安装完毕

## 卸载

深度商店搜索lamp卸载

命令安装，终端执行：

     sudo apt-get remove lamp

## 相关链接

[Deepin Linux 12.06 安装 Apache2+PHP5+MySQL5.5](http://www.linuxdeepin.com/forum/25/7792?p=33743)

[Ubuntu 12.04下LAMP安装配置](http://www.linuxidc.com/Linux/2012-05/61079.htm)
