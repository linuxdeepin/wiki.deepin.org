---
title: Desktop Entry 文件
description: 
published: true
date: 2022-06-08T07:01:32.662Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:33:02.433Z
---

[[en:Desktop_entry]]

## 简介

Desktop Entry 文件是 Linux 桌面系统中用于描述程序启动配置信息的文件。Desktop Entry 文件实现了类似于 Windows 操作系统中快捷方式的功能。

目前，Linux KDE 和 Linux GNOME 桌面系统都使用 Desktop Entry 文件标准来描述程序启动配置信息。Desktop Entry 文件标准是由 FreeDesktop.org制定的，目前最新的版本是"Desktop Entry Specification 1.0"

系统范围的Desktop Entry文件地址统一在 `/usr/share/applications`，文件以".desktop"为后缀名。用户个人的Desktop Entry 地址为 `~/.local/share/applications`。

## 简单解说

### 配置

    [Desktop Entry] #每个desktop文件都以这个标签开始，说明这是一个Desktop Entry文件
    Version = 1.0 #标明Desktop Entry的版本（可选）
    Name = Firefox #程序名称（必须），这里以创建一个Firefox的快捷方式为例
    GenericName = Web Browser #程序通用名称（可选）
    Comment = A Web Browser #程序描述（可选）
    Exec = firefox %u #程序的启动命令（必选），可以带参数运行,当下面的Type为Application，此项有效
    Icon = firefox #设置快捷方式的图标（可选）
    Terminal = false #是否在终端中运行（可选），当Type为Application，此项有效
    Type = Application #desktop的类型（必选），常见值有“Application”和“Link”
    Categories = GNOME;Application;Network; #注明在菜单栏中显示的类别（可选）

备注:desktop文件需要可执行权限才可运行，否则将以文本文件打开。

### 实例

例如：firefox

    [Desktop Entry]
    Version=1.0
    Name=Firefox Web Browser
    Name[zh_CN]=Firefox 浏览器
    Comment=Browse the World Wide Web
    Comment[zh_CN]=浏览互联网
    GenericName=Web Browser
    GenericName[zh_CN]=网络浏览器
    Exec=/home/abi/下载/firefox/firefox
    Terminal=false
    X-MultipleArgs=false
    Type=Application
    Icon=/home/abi/下载/firefox/icons/mozicon128.png
    Categories=GNOME;GTK;Network;WebBrowser;
    MimeType=text/html;text/xml;application/xhtml+xml;application/xml;application/vnd.mozilla.xul+xml;application/rss+xml;application/rdf+xml;image/gif;image/jpeg;image/png;x-scheme-handler/http;x-scheme-                  handler/https;x-scheme-handler/ftp;x-scheme-handler/chrome;video/webm;
    StartupWMClass=Firefox
    StartupNotify=true
    X-Ayatana-Desktop-Shortcuts=NewWindow;
    [NewWindow Shortcut Group]
    Name=Open a New Window
    Name[zh_CN]=新建窗口
    Exec=firefox -new-window about:blank
    TargetEnvironment=Unity

## 深入解说

[点此查看--Linux Desktop Entry 文件深入解析](http://www.ibm.com/developerworks/cn/linux/l-cn-dtef/#iratings)

## 来源链接

[ubuntu unity .desktop 文件书写方法](http://blog.sina.com.cn/s/blog_55e606c2010161xz.html)

## 类别（Categories）列表

[Registed Categories](https://specifications.freedesktop.org/menu-spec/menu-spec-1.0.html#category-registry)
