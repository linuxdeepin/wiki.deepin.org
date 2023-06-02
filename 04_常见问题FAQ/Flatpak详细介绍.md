---
title: Flatpak详细介绍
description: 
published: true
date: 2023-06-02T09:57:58.843Z
tags: 
editor: markdown
dateCreated: 2023-06-02T09:57:58.843Z
---

# Flatpak
Flatpak是一种构建、发布、安装和运行应用程序的技术。它主要的目标是Linux桌面系统，同时也可以适用于嵌入式等系统中。

### Flatpak的设计目标是：
* 使应用程序可以安装在任何一个发行版上
* 为应用程序提供固定的环境，实施测试和减少缺陷
* 实现应用程序和操作系统的解耦和，使应用程序可以不依赖于特定的发行版本
* 使应用程序可以自带依赖，能够使用linux发行版没有提供的依赖，避免其对特定发行版本甚至特定库的依赖
* 在沙箱中独立运行应用程序，提升安全性

Flatpak使这些特性易于实现。如果你对flatpak还不了解，建议尝试一下hello workd的例子。

更多信息可查看 flatpak.org

## Flatpak工作原理
我们可以通过几个关键概念来了解flatpak，这同时也可以解释flatpak和传统包管理器的差异。

Runtimes
运行时提供应用程序所须的基本依赖。有多种不同的运行时，从小（但是非常稳定）的freedesktop运行时，到大的桌面环境运行时例如KDE、Gnome等。

每一个应用程序都必须基于一个运行时进行构建，运行时需要安装在宿主系统中老保证应用的运行。用户可以同时安装多种不同的运行时，包括同一运行时的不同版本。

Tips:
每个运行时都可以被认为是一个/usr文件系统。事实上，当一个程序运行的时候，他的运行时会挂载到/usr

### Bundled libraries
如果一个应用程序需要依赖一些运行时没有提供的库，这些库就需要随应用程序绑定发布。这使应用程序可以使用系统中没有提供的依赖，或者使用与安装在宿主系统中的版本不同的依赖。

### SDKs（Software Developer Kits）
SDK是一个包含了“devel”模块的运行时，他们不会在应用运行的时候被用到。这些工具包含打包工具、头文件、编译器以及调试器。每一个应用程序都基于其运行时对应的SDK构建。

### Extensions
extension是一个运行时或者应用的可选项。通常是分离自运行时的翻译、调试信息等。例如org.freedesktop.Platform.Locale可以添加到org.freedesktop.Platform运行时实现翻译。

### Sandboxs
通过Flatpak，每一个应用都构建和运行在独立的环境中。默认情况下，应用程序只能看到它自己和它的运行时。访问用户文件、网络、图形套接字、bus子系统和设备都需要显示的授权。而其他的访问，例如访问其他进程等是不可能的。

## 　Flatpak命令
flatpak命令用于安装、删除、更新应用程序和运行时。同时也可用于浏览当前已经安装应用，并提供命令来构建和发布应用绑定。可通过flatpak –help获得帮助。
大部分的flatpak命令都是系统级的。如果要执行一个仅限于当前用户的命令，可以使用–user选项，这会使运行时和应用程序与当前用户绑定（仅运行在当前用户下）。
关于flatpak的更多信息可见Command Reference。

### 标识符
Flatpak使用一个唯一的名称标记每一个应用、运行时和SDK。有时候这回作为name/arch/branch的一部分。

### 命名
Flatpak的命名方式采用的是与DNS地址相关的方式，例如com.company.App。地址的最后一部分是对象的名字，前面的部分则是其所属的域。为了防止命名冲突，域应当与DNS注册地址一致。这表示域可以来自某个网站、某个应用或者某个组织。例如，如果一个应用的网站是app.com，则它的flatpak名称应当是com.app.App。多个应用可以属于同一个域，例如org.organization.App1 和org.organization.App2。
如果你没有为你的应用注册域，可以使用第三方网站表示，例如Github上就允许创建私人包，在这里，可以使用个人的名空间name.github.io，相应的应用则可命名为io.github.name.App。
如果一个应用提供了D-Bus服务，一般要求D-Bus服务的名字和应用的名字一致。

### 标识符三元组
很多Flatpak命令仅仅需要应用的名字、运行时或SDK。但是某些环境中也需要去指定架构和分支（通过分支可以指定特定的版本）。这可以通过name/arch/branch三元组实现。例如org.gnome.Sdk/x86_64/3.14 或者 org.gnome.Builder/i386/master。

### Under the hood
Flatpak使用了许多现有的技术。使用Flatpak并不需要对其非常了解，当然有些时候了解这些技术也是很有用的。这些技术包含：

- 来自Atomic项目的bubblewrap应用，它通过下列内核特性，可以让普通用户设置和运行容器：
Cgroups
Namespaces
Bind mounts
Seccomp规则
systemd为沙箱设置cgroups
- D-Bus，一种为应用程序提供高级API的成熟方法
- Open Container Initiative的OCI格式，作为单文件包的便捷传输格式
- 用于版本控制和分发文件系统的OSTree系统
- Appstream元数据，允许Flatpak应用程序在软件中心应用程序中很好地显示。