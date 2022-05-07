---
title: Deepin下java开发环境部署
description: 
published: true
date: 2022-05-07T02:36:09.243Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:32:22.510Z
---

## 简介

本经验由深度论坛用户(zhang12345shun)分享，[原文地址](https://bbs.deepin.org/forum.php?mod=viewthread&tid=36225)

## 正文

### SUN JDK（现已改名Oracle JDK）
1.下载Sun版JDK压缩包（.tar.gz），选择其中的32/64位Linux版本。

2.将其解压缩：

`sudo tar -zxvf ~/Downloads/jdk-8u45-linux-i586.tar.gz -C /usr/lib` 

其中参数-C后面的路径是解压缩的目标路径。

3.根据官网的说法：

> Starting with version 8u40, the JDK installation is integrated with the alternatives framework and after installation, the alternatives framework is updated to reflect the binaries from the recently installed JDK. Java commands such as java, javac, javadoc, and javap can be invoked from the command line.  

所以：

```
sudo update-alternatives --install /usr/bin/java java  /usr/lib/jdk1.8.0_66/bin/java 1000 
sudo update-alternatives --install /usr/bin/javac javac  /usr/lib/jdk1.8.0_66/bin/javac 1000
```

现在可以验证一下JDK安装是否已成功 

`java -version`

### tomact 安装和使用

1.下载并解压缩到部署位置(8.0.30)

2.配置环境变量

  `startup.sh----->catalina.sh----->setclassspath.shJAVA_HOME=/usr/lib/jdk1.8.0_66JRE_HOME=$JAVA_HOME/jre`

* 备注：这里的配置可以不写（如果jdk是8u40及以后版本） 

3.启动tomcat： 

  `sudo ./bin/startup.sh`

4.关闭tomcat： 

  `sudo ./bin/shutdown.sh`

5.最后，验证tomcat关闭是否成功：

  在浏览器中输入：http://localhost:8080/


### MYSQL安装和使用
1.下载并解压缩 

  `sudo tar -xzvf mysql-6.0.11-alpha-linux-x86_64-glibc23.tar.gz -C destdir`

2.新增用户mysql和组mysql 

```
sudo groupadd mysql 
sudo useradd -g mysql mysql
```

3.创建链接 

```
cd /usr/local 
sudo ln -s /opt/mysql-6.0.11-alpha-linux-x86_64-glibc23/ mysql
```

4.改变mysql文件夹own group 

```
sudo chown -R mysql . 
sudo chgrp -R mysql .
```

5.执行初始化脚本 

`scripts/mysql_install_db –user=mysql`

6.改变文件夹权限 

```
chown -R root . 
chown -R mysql data
```

7.配置mysql环境 

 使用自带的配置文件复制到/etc 目录下比如：

`cp support-files/my-medium.cnf /etc/my.cnf `

根据内存不同使用不同的配置文件。一般建议使用 

** my-larger.cnf ** 

* 说明：会占用系统内存512M，运行主要的进行。

** my-medium.cnf ** 

* 说明：mysql平时只占用系统内存在【32M～64M】之间，或者和其他程序一起工作时比如 web server .占用内存不会超过128M 

** my-small.cnf ** 

* 说明：只占用系统的很小内存（<=64M）,只运行重要的守护进程。不会占用太多的资源 

8.启动服务 

```
bin/mysqld_safe –user=mysql & //启动服务 
bin/mysqladmin -u root password ‘new_password’ //初始化root密码
```

9.开机自启动 

复制服务脚本 : `cp support-files/mysql.server /etc/init.d/mysql `

取消自启动:`sudo update-rc.d -f mysql.server remove`

把 /usr/local/mysql/bin/mysql 命令加到用户命令中 

`sudo ln -s /usr/local/mysql/bin/mysql /usr/local/bin/mysql `

现在就直接可以使用 mysql 命令了 

`mysql -u root -p`

### Eclipse 安装使用
1.安装JDK8，具体过程参考上面
2.下载 Eclipse 最新版http://www.eclipse.org/downloads/ 

解压 Eclipse:

 `sudo tar -zxvf ~/Downloads/eclipse-*.tar.gz`

3.创建 Eclipse 快捷方式 

在终端中执行如下命令 

` sudo gedit /usr/share/applications/eclipse.desktop `

粘贴并保存如下内容 

```
[Desktop Entry] 
Type=Application 
Name=Eclipse 
Comment=Eclipse Integrated Development Environment 
Icon=eclipse 
Exec=/opt/eclipse/eclipse 
Terminal=false 
Categories=Development;IDE;Java; 
```

至此，我们就将最新版本的 Eclipse 安装完成


### MAVEN安装

1.下载并加压包到安装位置 exp:/usr/local/

2.配置命令连接符

`sudo update-alternatives --install /usr/bin/mvn mvn /opt/apache-maven-3.3.9/bin/mvn 1000`

3.配置默认jdk版本和默认编译级别在目录conf/setting.xml中配置

```
<profile>
    <id>jdk-1.8</id>
    <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
    </activation>
    <properties>      
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>            
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
    </properties>
</profile>
```