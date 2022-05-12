---
title: Deepin154安装Filezilla_FTP_Server
description: 
published: true
date: 2022-05-07T07:47:22.111Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:31:34.567Z
---

## 简介
Deepin15.4安装FTP服务器--FileZilla Server

## 安装方法

1. 下载FileZilla Server 的exe安装程序：<https://filezilla-project.org/>
2. 使用Deepin15.4下的CrossOver安装上面的软件，默认XP-32容器即可。
3. 安装完成后，在启动器中点击图标可以启动管理界面
4. 像在Windows下的那样，配置服务器的组，用户，以及共享文件夹。
5. 在菜单栏的Server下，点击Active,观察是否可以激活，若是激活的，前面有对号。
6. 如果有对号，说明服务器正常，客户端直接连接即可；否则可能会出现如下的错误：

```
Failed to create listen socket on port 21
Failed to create a listen socket on any of the specified ports. Server is not online!
```

出现这个结果，说明监听21端口失败。

7. 更改默认的２１端口为其他没有使用的端口，我的是改为2121

点击，菜单栏Edit－－－Settings或者界面上的齿轮图标，打开配置界面：

```general settings----connection settings---listen on these ports---将值由21改为2121或者其他没有使用的端口。```
8. 在菜单栏的Server下，点击Active,观察是否可以激活，结果正常了：

```
Creating listen socket on port 2121...
Server online
```

9. 接下来使用客户端连接即可。目前木有发现其他的问题。
