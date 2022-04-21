---
title: LAMP_service
description: 
published: true
date: 2022-04-21T03:56:11.293Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:56:08.504Z
---

[[zh:LAMP服务]]


## Summary

LAMP is short for Linux + Apache + MySQL + PHP， a suite of web server software.

## Install LAMP

### Install LAMP develop environment

You can either search "lamp" in Deepin Store to install it, or execute in terminal:

    sudo apt-get install lamp

It is also possible to build from source and then install it. Download the source package (.tar.gz) and extract it, then switch to the directory that contains the source code and execute:

    ./configure 
    sudo make && make install

This, however, is only recommanded to advance user, as it needs some experience of solving dependencies of packages, libraries and modules, which can be time consuming.

### Install each deb package one by one

Benefit from the powerful function of dependency resolving of apt, users can also install  and Apache, MySQL and PHP seperately, then configure them manually.

First install MySQL:

    sudo apt-get install mysql-server-5.6 mysql-client-5.6

You may need to set a password for the root account

    New password for the MySQL "root" user:
    Repeat password for the MySQL "root" user:

Then install Apache2:

    sudo apt-get install apache2

After that, type in the address bar of your browser:

    http://127.0.0.1

to see if Apache2 works. If a string "It works!" appears, the Apache server has already been started successfully.

The default document root of Apache is /var/www, and the main configuration file is "/etc/apache2/apache2.conf". All extra configurations are stored in "/etc/apache2", for example "/etc/apache2/mods-enabled" is the directory where all Apache modules are stored, "/etc/apache2/sites-enabled" is used for all virtual hosts configurations, and "/etc/apache2/conf.d" is for other server-related configurations.

Install PHP5 and PHP5 module for Apache:

    sudo apt-get install php5 php5-mysql libapache2-mod-php5

and restart Apache server:

    sudo /etc/init.d/apache2 restart

Test for PHP5 (optional):

    sudo gedit /var/www/info.php

and add following lines:    

     <?php
    phpinfo();
     ?>

then visit following address in a browser:

    http://127.0.0.1/info.php

to see all supported modules of PHP5.

Install some extra modules if needed:

     sudo apt-get install php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl

Restart Apache2 for PHP5 modules to be integrated into Apache server:

    sudo /etc/init.d/apache2 restart

### To manage MySQL more easily, you can install phpmyadmin, a PHP front end for MySQL management:

    sudo apt-get install phpmyadmin

During the installation, you may be asked to choose "Web server" between "apache2" or "lighttpd". Please select "apache2", then press Tab and enter. You may also need to input "Password of the database's administrative user".

Then create a link to phpmyadmin in the document root of Apache. For example, the document root is at /var/www, and the phpmyadmin is installed in /usr/share/phpmyadmin, then we need to execute:

    sudo ln -s /usr/share/phpmyadmin /var/www 

To test if phpmyadmin has been installed correctly: type

    http://127.0.0.1/phpmyadmin

in the browser, and type user name and password to see if the login is successful.

## Uninstall LAMP suite

Execute in terminal:

    sudo apt-get remove lamp
     
if you have installed "lamp" suite previously, or

    sudo apt-get remove mysql-server-5.6 mysql-client-5.6 apache2 php5 php5-mysql libapache2-mod-php5 phpmyadmin

if you have each components of LAMP separately.


## References

[Deepin Linux 12.06 安装 Apache2+PHP5+MySQL5.5](http://www.linuxdeepin.com/forum/25/7792?p=33743)

[Ubuntu 12.04下LAMP安装配置](http://www.linuxidc.com/Linux/2012-05/61079.htm)