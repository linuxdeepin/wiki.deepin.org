---
title: 如何在deepin下部署MySQL服务
description: 
published: true
date: 2023-03-12T15:18:02.419Z
tags: 
editor: markdown
dateCreated: 2023-03-12T15:15:21.588Z
---

## 简介

MySQL是一个关系型数据库管理系统，由瑞典MySQL AB 公司开发，属于 Oracle 旗下产品。MySQL 是最流行的关系型数据库管理系统之一，在 WEB 应用方面，MySQL是最好的 RDBMS (Relational Database Management System，关系数据库管理系统) 应用软件之一。

## 前言

这里不使用命令行从软件源安装的方式，为了配置更加灵活以及多版本数据库共存的考虑，使用通用包解压安装。
MySQL5.x与MySQL8.x配置有一定区别，这里分别给出两个版本的安装。

## MySQL5.x

获取通用二进制安装包，这是MySQL官方下载地址
[安装包链接](https://downloads.mysql.com/archives/community/)

### 解压安装

这里选择5.7.40 x64版本的安装包
```bash
wget https://cdn.mysql.com/archives/mysql-5.7/mysql-5.7.40-linux-glibc2.12-x86_64.tar.gz
```

解压
```bash
sudo tar -zxvf mysql-5.7.40-linux-glibc2.12-x86_64.tar.gz -C /usr/local
```

重命名
```bash
sudo mv /usr/local/mysql-5.7.40-linux-glibc2.12-x86_64 /usr/local/mysql5
```

创建相应目录并赋予合适权限
```bash
sudo mkdir -p /usr/local/mysql5/data /usr/local/mysql5/log
sudo touch /usr/local/mysql5/log/mysql.err /usr/local/mysql5/mysql.pid
sudo chown mysql:mysql -R /usr/local/mysql5
```

配置文件编写 在mysql5的目录下创建
```bash
sudo vim /usr/local/mysql5/my.cnf
```

写入内容
```bash
[mysqld]
# 设置能访问数据库的ip地址 0.0.0.0代表无限制
bind-address=0.0.0.0
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=/usr/local/mysql5
# 设置mysql数据库的数据的存放目录
datadir=/usr/local/mysql5/data
# 本地连接的 socket 套接字
socket=/usr/local/mysql5/mysql.sock
# 错误日志存放位置
log_error=/usr/local/mysql5/log/mysql.err
# pid-file文件位置 当MySQL实例启动时，会将自己的进程ID写入一个文件中——该文件即为pid文件
pid_file=/usr/local/mysql5/mysql.pid
# 允许最大连接数
max_connections=1000
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=100
# 服务端使用的字符集默认为UTF8
character-set-server=utf8mb4
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
default_authentication_plugin=mysql_native_password
#是否对sql语句大小写敏感，1表示不敏感
lower_case_table_names = 1
#MySQL连接闲置超过一定时间后(单位：秒)将会被强行关闭
#MySQL默认的wait_timeout  值为8个小时, interactive_timeout参数需要同时配置才能生效
interactive_timeout = 1800
wait_timeout = 1800
#Metadata Lock最大时长（秒）， 一般用于控制 alter操作的最大时长sine mysql5.6
#执行 DML操作时除了增加innodb事务锁外还增加Metadata Lock，其他alter（DDL）session将阻塞
lock_wait_timeout = 3600
#内部内存临时表的最大值。
#比如大数据量的group by ,order by时可能用到临时表，
#超过了这个值将写入磁盘，系统IO压力增大
tmp_table_size = 64M
max_heap_table_size = 64M
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
# 本地连接的 socket 套接字
socket=/usr/local/mysql5/mysql.sock
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
# 本地连接的 socket 套接字
socket=/usr/local/mysql5/mysql.sock
```

无注释版
```bash
[mysqld]
bind-address=0.0.0.0
port=3306
basedir=/usr/local/mysql5
datadir=/usr/local/mysql5/data
socket=/usr/local/mysql5/mysql.sock
log_error=/usr/local/mysql5/log/mysql.err
pid_file=/usr/local/mysql5/mysql.pid
max_connections=1000
max_connect_errors=100
character-set-server=utf8mb4
default-storage-engine=INNODB
default_authentication_plugin=mysql_native_password
lower_case_table_names = 1
interactive_timeout = 1800
wait_timeout = 1800
lock_wait_timeout = 3600
tmp_table_size = 64M
max_heap_table_size = 64M
[mysql]
default-character-set=utf8mb4
socket=/usr/local/mysql5/mysql.sock
[client]
port=3306
default-character-set=utf8mb4
socket=/usr/local/mysql5/mysql.sock
```
### 创建用户组

mysql用户不涉及登录，仅作为运行用户使用。
```bash
groupadd mysql
useradd -r -g mysql -s /bin/false mysql
```

### 初始化

```bash
sudo /usr/local/mysql5/bin/mysqld --defaults-file=/usr/local/mysql5/my.cnf --initialize --datadir=/usr/local/mysql5/data --basedir=/usr/local/mysql5 --user=mysql
```
参数说明

> --defaults-file=/usr/local/mysql5/my.cnf 指定配置文件(一定要放在最前面，至少 --initialize 前面)
--user=mysql 指定用户(很关键)
--basedir=/usr/local/mysql5/ 指定安装目录
--datadir=/usr/local/mysql5/data 指定数据目录

查看随机密码
```bash
sudo cat /usr/local/mysql5/log/mysql.err
```
可以看到最后一行，生成11位的密码
```bash
[Note] A temporary password is generated for root@localhost: P)lmw5.?BsJp
```

### 添加服务到系统
服务service文件到/etc/init.d
```bash
sudo cp /usr/local/mysql5/support-files/mysql.server /etc/init.d/mysql5
```
替换变量
```bash
sudo sed -i '46s@basedir=@basedir=/usr/local/mysql5@g' /etc/init.d/mysql5
sudo sed -i '47s@datadir=@datadir=/usr/local/mysql5/data@g' /etc/init.d/mysql5
sudo sed -i "57s@lockdir='/var/lock/subsys'@lockdir='/var/lock/subsys5'@g" /etc/init.d/mysql5
sudo sed -i '58s@lock_file_path="$lockdir/mysql"@lock_file_path="$lockdir/mysql5"@g' /etc/init.d/mysql5
sudo sed -i '63s@mysqld_pid_file_path=@mysqld_pid_file_path=/usr/local/mysql5/mysqld.pid@g' /etc/init.d/mysql5
sudo sed -i '207s@conf=/etc/my.cnf@conf=/usr/local/mysql5/my.cnf@g' /etc/init.d/mysql5
```

授权以及刷新服务
```bash
sudo chmod +x /etc/init.d/mysql5
systemctl daemon-reload
```

运行服务
```bash
# 启动
systemctl start mysql5
# 停止
systemctl stop mysql5
```

查看服务状态
```bash
systemctl status mysql5
```

添加软链接
```bash
sudo ln -s /usr/local/mysql5/bin/mysql /usr/bin/mysql5
```
为了区分其他的版本的mysql这里加上软链接。

### 登录
```bash
mysql5 -u root -h 127.0.0.1 -p
```
> 这里需要加上-h 指定ip使用tcp方式连接。
> 或者使用-S 指定sock路径，否则默认情况下使用/tmp/mysql.sock路径的sock，会无法连接。
> 
{.is-info}

```bash
mysql5 -u root -S /usr/local/mysql5/mysql.sock -p
```

### 修改密码

修改临时root密码和修改相应权限
```bash
set password=password('123456'); # 修改密码
use mysql # 连接权限数据库
update user set host='%' where host='localhost' and user='root';
flush privileges; # 刷新权限
```
## MySQL8.x

[安装包链接](https://downloads.mysql.com/archives/community/)

### 解压安装

这里选择8.0.32 x64版本的安装包
```bash
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-8.0.31-linux-glibc2.12-x86_64.tar.xz
```

解压
```bash
sudo tar -xvJf mysql-8.0.31-linux-glibc2.12-x86_64.tar.xz -C /usr/local
```

重命名
```bash
sudo mv /usr/local/mysql-8.0.31-linux-glibc2.12-x86_64 /usr/local/mysql8
```

创建相应目录并赋予合适权限
```bash
sudo mkdir -p /usr/local/mysql8/data /usr/local/mysql8/log
sudo touch /usr/local/mysql8/log/mysql.err /usr/local/mysql8/mysql.pid
sudo chown mysql:mysql -R /usr/local/mysql8
```

配置文件编写 在mysql8的目录下创建
```bash
sudo vim /usr/local/mysql8/my.cnf
```

写入内容
```bash
[mysqld]
bind-address=0.0.0.0
port=3306
basedir=/usr/local/mysql8
datadir=/usr/local/mysql8/data
socket=/usr/local/mysql8/mysql.sock
log_error=/usr/local/mysql8/log/mysql.err
pid_file=/usr/local/mysql8/mysql.pid
max_connections=1000
max_connect_errors=100
character-set-server=utf8mb4
default-storage-engine=INNODB
default_authentication_plugin=mysql_native_password
lower_case_table_names = 1
interactive_timeout = 1800
wait_timeout = 1800
lock_wait_timeout = 3600
tmp_table_size = 64M
max_heap_table_size = 64M
[mysql]
default-character-set=utf8mb4
socket=/usr/local/mysql8/mysql.sock
[client]
port=3306
default-character-set=utf8mb4
socket=/usr/local/mysql8/mysql.sock
```

### 创建用户组

mysql用户不涉及登录，仅作为运行用户使用。
```bash
groupadd mysql
useradd -r -g mysql -s /bin/false mysql
```

### 初始化

```bash
sudo /usr/local/mysql8/bin/mysqld --defaults-file=/usr/local/mysql8/my.cnf --initialize --datadir=/usr/local/mysql8/data --basedir=/usr/local/mysql8 --user=mysql
```
参数说明

> --defaults-file=/usr/local/mysql8/my.cnf 指定配置文件(一定要放在最前面，至少 --initialize 前面)
--user=mysql 指定用户(很关键)
--basedir=/usr/local/mysql8/ 指定安装目录
--datadir=/usr/local/mysql8/data 指定数据目录

查看随机密码
```bash
sudo cat /usr/local/mysql8/log/mysql.err
```
可以看到最后一行，生成11位的密码
```bash
[Note] A temporary password is generated for root@localhost: 7GpDqGyqrw;V
```

### 添加服务到系统
服务service文件到/etc/init.d
```bash
sudo cp /usr/local/mysql8/support-files/mysql.server /etc/init.d/mysql8
```

替换变量
```bash
sudo sed -i '46s@basedir=@basedir=/usr/local/mysql8@g' /etc/init.d/mysql8
sudo sed -i '47s@datadir=@datadir=/usr/local/mysql8/data@g' /etc/init.d/mysql8
sudo sed -i "57s@lockdir='/var/lock/subsys'@lockdir='/var/lock/subsys8'@g" /etc/init.d/mysql8
sudo sed -i '58s@lock_file_path="$lockdir/mysql"@lock_file_path="$lockdir/mysql8"@g' /etc/init.d/mysql8
sudo sed -i '63s@mysqld_pid_file_path=@mysqld_pid_file_path=/usr/local/mysql8/mysqld.pid@g' /etc/init.d/mysql8
sudo sed -i '207s@conf=/etc/my.cnf@conf=/usr/local/mysql8/my.cnf@g' /etc/init.d/mysql8
```

授权以及刷新服务
```bash
sudo chmod +x /etc/init.d/mysql8
systemctl daemon-reload
```

运行服务
```bash
# 启动
systemctl start mysql8
# 停止
systemctl stop mysql8
```

查看服务状态
```bash
systemctl status mysql8
```

添加软链接
```bash
sudo ln -s /usr/local/mysql8/bin/mysql /usr/bin/mysql8
```

### 登录

```bash
mysql8 -u root -h 127.0.0.1 -p
```
> 这里需要加上-h 指定ip使用tcp方式连接。
> 或者使用-S 指定sock路径，否则默认情况下使用/tmp/mysql.sock路径的sock，会无法连接。
> 
{.is-info}

```bash
mysql8 -u root -S /usr/local/mysql8/mysql.sock -p
```

### 修改密码

修改临时root密码和修改相应权限
```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456'; # 修改密码
USE mysql # 使用数据库
UPDATE user set host = '%' where user = 'root'; # 允许root从任意ip连接
FLUSH PRIVILEGES; # 刷新权限
```

到此两个版本的MySQL服务的部署结束，但是这里两个版本的MySQL都是使用3306端口运行的如果希望同时运行的话，需要设置将my.cnf中设置不同端口，并且在mysql 连接时指定-P设置的端口进行连接。

## 参考

[Debian10安装MySQL8详细教程](https://www.cnblogs.com/tothk/p/16498848.html)
[图文流程 Linux部署多个Mysql](https://blog.csdn.net/qq_37241221/article/details/126304321)
[Linux中安装mysql8(详细)](https://blog.csdn.net/studio_1/article/details/128345934?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-128345934-blog-106499000.pc_relevant_3mothn_strategy_and_data_recovery&spm=1001.2101.3001.4242.1&utm_relevant_index=3)
[mysql如何指定默认sock_MySQL多实例的环境下，服务器端本地连接到指定实例的问题（sock方式连接）](https://blog.csdn.net/weixin_42511507/article/details/113193659)