---
title: Desktop_entry
description: 
published: true
date: 2022-05-07T02:29:15.223Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:55:02.566Z
---

## Summary

Desktop Entry is the configuration file of startup in Linux. It has the similar function of shortcuts in Windows.

Currently, KDE Plasma and GNOME desktop use desktop entries to describe configurations of startup of program. FreeDesktop.org defines the standard for desktop entries, and the latest version of standard is "Desktop Entry Specification 1.0".

Desktop entry files of system-scope are located in /usr/share/applications, while personal desktop entries are usually found in ~/.local/share/applications. All of these files have the same extension ".desktop".

## Descriptioin

### Configurations

    [Desktop Entry] # Common header of each .desktop file, suggesting that it is a desktop entry
    Version = 1.0 # Version of this desktop entry (optional)
    Name = Firefox # Program name (mandatory)
    GenericName = Web Browser # Common name of the program (optional)
    Comment = A Web Browser # Description of the program (optional)
    Exec = firefox %u # Command to start the program (mandatory); arguments can be appeded if "Type" is set to "Application"
    Icon = firefox # Icon of the program (mandatory)
    Terminal = false # Whether started in terminal (optional); available when "Type" is set to "Application"
    Type = Application # Type of the program (mandatory); possible values: "Application", "Link", etc.
    Categories = GNOME;Application;Network; # Categories to be shown in application menu (optional)

Note: Desktop entries need to have executing permission before they can be run by users.

### Example

    [Desktop Entry]
    Version=1.0
    Name=Firefox Web Browser
    Name[zh_CN]=Firefox 浏览器
    Comment=Browse the World Wide Web
    Comment[zh_CN]=浏览互联网
    GenericName=Web Browser
    GenericName[zh_CN]=网络浏览器
    Exec=/usr/bin/firefox
    Terminal=false
    X-MultipleArgs=false
    Type=Application
    Icon=/usr/share/firefox/browser/icons/mozicon128.png
    Categories=GNOME;GTK;Network;WebBrowser;
    MimeType=text/html;text/xml;application/xhtml+xml;application/xml;application/vnd.mozilla.xul+xml;application/rss+xml;application/rdf+xml;image/gif;image/jpeg;image/png;x-scheme-handler/http;x-scheme-handler/https;x-scheme-handler/ftp;x-scheme-handler/chrome;video/webm;  # Used when deciding which kinds of file can be opened with this program
    StartupWMClass=Firefox
    StartupNotify=true
    X-Ayatana-Desktop-Shortcuts=NewWindow;

    # The following are configuration of shortcut defined by users
    [NewWindow Shortcut Group]
    Name=Open a New Window
    Name[zh_CN]=新建窗口
    Exec=firefox -new-window about:blank
    TargetEnvironment=Unity


## Reference

[ubuntu unity .desktop 文件书写方法](http://blog.sina.com.cn/s/blog_55e606c2010161xz.html)