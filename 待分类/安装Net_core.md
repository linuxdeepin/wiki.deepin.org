---
title: 安装Net_core
description: 
published: true
date: 2022-05-07T07:49:30.783Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:48:00.119Z
---

## 安装.NET Core SDK

开始安装之前，你需要卸载以前安装的.Net Core版本。

```
sudo apt-get install curl libunwind8 gettext libicu52
curl -sSL -o dotnet.tar.gz https://go.microsoft.com/fwlink/?linkid=848826
sudo mkdir -p /opt/dotnet && sudo tar zxf dotnet.tar.gz -C /opt/dotnet
sudo ln -s /opt/dotnet/dotnet /usr/local/bin
```

**说明:**
**第一步** 是下载相应的工具和.NET Core依赖库。其中deepin系统不存在libicu52库，所以需要安装libicu52，这是原始debian安装说明没有的。
**第二步** 下载相应的SDK文件到dotnet.tar.gz文件，由于该SDK文件较大，所以下载需要很长时间，有需要的同学，就要慢慢等待了。
**第三步** 创建存放文件夹，并解压下载的SDK压缩文件到/opt/dotnet目录。
**第四步** 将dotnet工具链接到/usr/local/bin目录，方便直接在命令中使用该命令。

## 解决dotnet报段错误异常

- 下载 [libcurl3_7.38.0-4+deb8u11_amd64.deb](http://security.debian.org/debian-security/pool/updates/main/c/curl/libcurl3_7.38.0-4+deb8u11_amd64.deb)

- 安装libcurl3_7.38.0-4+deb8u11_amd64.deb

```
    sudo dpkg -i libcurl3_7.38.0-4+deb8u3_amd64.deb
```

- 卸载 curl，锁定lock libcurl3，防止以后在线升级该库。

```
    sudo apt purge curl
    sudo apt-mark hold libcurl3
```

## 初始化代码

让我们初始化一个hello world程序!

    dotnet new console -o hwapp
    cd hwapp

## 运行该事例程序

第一个命令将恢复项目文件中指定的包，第二个命令将运行实际的示例。

    dotnet restore
    dotnet run

准备好了吗!
现在你有一个.Net core程序运行在你的机器上了。

浏览 [.NET 文档](https://docs.microsoft.com/dotnet/core) 获得更多教程, 获得更多示例和完整的 .NET Core 文档.

## 哪些工具推荐?

- **Visual Studio Code**
[下载](https://code.visualstudio.com/Download?wt.mc_id=DotNet_Home) Visual Studio Code

Visual Studio Code has full support for .NET Core. Install the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) to get the best experience.

- **OmniSharp**

[下载](http://www.omnisharp.net/#integrations) OmniSharp

The OmniSharp project enables cross-platform .NET development in editors such as Atom, Brackets, Sublime Text, Emacs, and Vim.

- **JetBrains Rider**

[下载](https://www.jetbrains.com/rider/download/?utm_source=msnetcoremac&utm_medium=web)JetBrains Rider

JetBrains Rider is a cross-platform .NET IDE built using IntelliJ and ReSharper technology. It offers support for .NET and .NET Core applications on all platforms.

## 参考引用链接

官方debian安装教程：<https://www.microsoft.com/net/core#linuxdebian>

ASP.NET core搭建于Deepin 2015.4记录： <https://my.oschina.net/bubifengyun/blog/738634>
