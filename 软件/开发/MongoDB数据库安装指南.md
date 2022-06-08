---
title: MongoDB数据库安装指南
description: 
published: true
date: 2022-05-07T07:48:24.012Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:38:24.645Z
---

# MongoDB安装指南

## Platform Support

MongoDB only provides packages for 64-bit builds of Debian 8 and 9. See Supported Platforms for more information.

## 程序包相关

MongoDB provides officially supported packages in their own repository:

Package Name         Description
mongodb-org         A metapackage that will automatically install the four component packages listed below.
mongodb-org-server Contains the mongod daemon, associated init script, and a configuration file (/etc/mongod.conf). You can use the initialization script to start mongod with the configuration file. For details, see Run MongoDB Community Edition.
mongodb-org-mongos Contains the mongos daemon.
mongodb-org-shell Contains the mongo shell.
mongodb-org-tools Contains the following MongoDB tools: mongoimport bsondump, mongodump, mongoexport, mongofiles, mongorestore, mongostat, and mongotop.

## 安装 MongoDB社区版

NOTE:To install a different version of MongoDB, please refer to that version’s documentation. To install the previous version, see the tutorial for version 3.6.

## Using .deb Packages方式

The Debian package management tools (i.e. dpkg and apt) ensure package consistency and authenticity by requiring that distributors sign packages with GPG keys.

### 1. 导入MongoDB公钥

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
```

### 2. 创建MongoDB的软件源 /etc/apt/sources.list.d/mongodb-org-4.0.list

你可以选择安装 Debian 9 “Stretch”仓库

```
echo "deb http://repo.mongodb.org/apt/debian stretch/mongodb-org/4.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
```

### 3. 安装MongoDB

```
sudo apt-get update
sudo apt-get install -y mongodb-org
```

### 4.解决没有/home/mongodb目录的问题

```
sudo mkdir /home/mongodb
sudo chown -R mongodb:mongodb /home/mongodb
```

## 运行 MongoDB

Most Unix-like operating systems limit the system resources that a session may use. These limits may negatively impact MongoDB operation. See UNIX ulimit Settings for more information.

The MongoDB instance stores its data files in /var/lib/mongodb and its log files in /var/log/mongodb by default, and runs using the mongodb user account. You can specify alternate log and data file directories in /etc/mongod.conf. See systemLog.path and storage.dbPath for additional information.

If you change the user that runs the MongoDB process, you must modify the access control rights to the /var/lib/mongodb and /var/log/mongodb directories to give this user access to these directories.

### 1. 启动 MongoDB 服务

命令启动MongoDB的 mongod 服务项。:

```
sudo systemctl start mongod 
```

### 2. 验证 MongoDB是否安装成功

查看MongoDB的日志文件 **/var/log/mongodb/mongod.log**，你可以看到类似这样的描述信息

```
[initandlisten] waiting for connections on port <port>
where <port> is the port configured in /etc/mongod.conf, 27017 by default.
```

你也可以通过命令过滤查看信息

```
cat /var/log/mongodb/mongod.log | grep port
```

默认端口是2701

### 3. 停止 MongoDB 服务

```
sudo systemctl stop mongod
```

### 4. 重启 MongoDB 服务

```
sudo systemctl restart mongod
```

### 5.设置开机启动或禁用 MOngoDB服务

设置开机启动：

```
sudo systemctl enable mongod
```

如果你不想开机启动，也可以禁用开机启动

```
sudo systemctl disable mongod
```

### 6. 开始使用 MongoDB

如果想获取帮助, [MongoDB](https://docs.mongodb.com/manual/#getting-started) 提供了开始指导. 看看 [开始指南](https://docs.mongodb.com/manual/#getting-started) 获取有用帮助信息。
在命令窗口输入 **monogo**进入MongoDB交互模式，  **Control+C** 退出命令窗口，当然后台mongod必须在运行状态。

## 删除 MongoDB

To completely remove MongoDB from a system, you must remove the MongoDB applications themselves, the configuration files, and any directories containing data and logs. The following section guides you through the necessary steps.

**警告：** 这个删除MongoDB过程是不可逆的，包括程序配置文件、数据库文件都将被删除。所以，删除前，请备份好配置文件和数据库文件。

### 1. 停止 MongoDB 服务

通过命令停止mongodb服务:

```
sudo systemctl stop mongod 
```

### 2. 删除软件

移除已经安装的MongoDB程序。

```
sudo apt-get purge mongodb-org*
```

### 3. 删除数据目录

删除 MongoDB 数据库文件和日志文件。

```
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb
```

官方参考链接 <https://docs.mongodb.com/manual/tutorial/install-mongodb-on-debian/>
