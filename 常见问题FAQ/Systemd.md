---
title: Systemd
description: 
published: true
date: 2022-06-16T03:03:52.504Z
tags: systemd
editor: markdown
dateCreated: 2022-06-16T03:02:15.607Z
---

# Systemd

[跳到导航](http://old.deepin.wiki/index.php?title=Systemd#mw-head)[跳到搜索](http://old.deepin.wiki/index.php?title=Systemd#searchInput)

Systemd不仅能完成系统的初始化工作，也能对系统和服务进行管理。

## 目录



- [1简介](http://old.deepin.wiki/index.php?title=Systemd#.E7.AE.80.E4.BB.8B)
- 2systemctl命令
  - [2.1启动服务](http://old.deepin.wiki/index.php?title=Systemd#.E5.90.AF.E5.8A.A8.E6.9C.8D.E5.8A.A1)
  - [2.2重启服务](http://old.deepin.wiki/index.php?title=Systemd#.E9.87.8D.E5.90.AF.E6.9C.8D.E5.8A.A1)
  - [2.3停止服务](http://old.deepin.wiki/index.php?title=Systemd#.E5.81.9C.E6.AD.A2.E6.9C.8D.E5.8A.A1)
  - [2.4启用和禁用服务](http://old.deepin.wiki/index.php?title=Systemd#.E5.90.AF.E7.94.A8.E5.92.8C.E7.A6.81.E7.94.A8.E6.9C.8D.E5.8A.A1)
  - [2.5查看服务](http://old.deepin.wiki/index.php?title=Systemd#.E6.9F.A5.E7.9C.8B.E6.9C.8D.E5.8A.A1)
  - [2.6电源管理](http://old.deepin.wiki/index.php?title=Systemd#.E7.94.B5.E6.BA.90.E7.AE.A1.E7.90.86)

## 简介

Systemd在系统中是一个用户级应用程序，它包含一个完整的软件包，配置文件位于`/etc/systemd`目录下，配置工具命令位于`/bin`、`/sbin`两个目录下。备用配置文件位于`/lib/systemd`目录下。
它提供了一个非常强大的命令行工具——[systemctl](http://old.deepin.wiki/index.php?title=Systemd#systemctl.E5.91.BD.E4.BB.A4)。同时，它也提供了service、chkconfig命令，但其本质上仍会被重定向至systemctl命令。

## systemctl命令

### 启动服务

要通过systemctl启动一个服务，可以使用start选项。
例如：

```
systemctl start lightdm.service
```

这里的`.service`可以省略，省略后可以写成：

```
systemctl start lightdm
```

其他命令中此后缀均可省略。

### 重启服务

要重启服务，可以使用restart选项，此选项的作用是，若服务正在运行中，则重启服务，若服务不在运行中，则会启动。
也可使用try-start选择，它只会在服务已经运行的情况下重启服务。
同时，也可以使用reload选项，他会重新加载配置文件。
例如：

```
systemctl restart lightdm
systemctl try-start lightdm
systemctl reload lightdm
```

### 停止服务

要停止正在启动状态的服务，可以使用stop选项。
例如：

systemctl stop lightdm

### 启用和禁用服务

不同于启动服务，启用服务控制的是开机是否要自启动，执行启用服务或者禁用服务命令后，该服务当前的状态并不会发生变化，他将会在系统重新启动时发挥作用。 要启用或禁用服务，可以使用enable（启用）或disable（禁用）选项：

```
systemctl enable lightdm
systemctl disable lightdm
```

### 查看服务

要观察一个服务的运行状态可以使用status选项。

```
systemctl status lightdm
```

### 电源管理

除了管理服务外，它还可以管理系统电源：

- 关闭系统

```
systemctl poweroff
```

- 重启系统

```
systemctl reboot
```

- 进入待机模式

```
systemctl suspend
```

- 进入休眠模式

```
systemctl hibernate
```

- 进入混合休眠模式

```
systemctl hybrid-sleep
```