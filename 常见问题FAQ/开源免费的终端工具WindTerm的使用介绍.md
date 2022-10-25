---
title: 开源免费的终端工具WindTerm的使用介绍
description: 
published: true
date: 2022-10-25T15:06:17.677Z
tags: windterm
editor: markdown
dateCreated: 2022-10-25T14:59:01.931Z
---

# 开源免费的终端工具WindTerm的使用介绍
推荐一款终端神器——WindTerm，完全开源，在 GitHub上已经收获6.6k的star

https://github.com/kingToolbox/WindTerm

![2022-10-25_82747.png](/2022-10-25_82747.png)


作者还拿 WindTerm 和 Putty、xterm、Windows Terminal + ssh.exe、iterm2、rxvt、Gnome等等做了一个性能对比


![2022-10-25_9233.png](/2022-10-25_9233.png)

## 安装 WindTerm
WindTerm 不仅开源免费，还跨平台，支持 Windows、Linux 和 macOS

直接到 release 页面选择适合自己操作系统的安装包。

https://github.com/kingToolbox/WindTerm/releases

![2022-10-25_44560.png](/2022-10-25_44560.png)

安装完成后，打开的界面和传统的终端不太一样，WindTerm 更像 IDE 的布局，左边是资源管理器+文件管理器，中间会默认打开一个 zsh 的终端窗口，右边是会话窗口+历史命令窗口，底部是发送窗口 + Shell 窗口。

![2022-10-25_4403.png](/2022-10-25_4403.png)


## 使用 WindTerm
SSH
使用终端最重要的一个场景就是 SSH，连接远程服务器，我这里有一个 1G 内存的轻量级云服务器，我们来连接它体验一下。

点击新建会话按钮开始 SSH 连接

![2022-10-25_17925.png](/2022-10-25_17925.png)

添加主机名，点击「连接」开始进行远程链接，输入用户名和密码，我们关掉一些没必要的窗口，让整个界面更加清爽一些

![2022-10-25_30390.png](/2022-10-25_30390.png)

如果感觉字体比较小的话，可以直接按住**「command+」**两个组合键放大字体。

WindTerm 给我一个非常直观的操作是，它提供了一个折叠的功能，点击-号折叠，点击+号展开

![2022-10-25_50358.png](/2022-10-25_50358.png)

还有一个就是智能提示，非常到位，响应速度很快。

![2022-10-25_91294.png](/2022-10-25_91294.png)

## SFTP
除了 SSH，还有一个重要的场景就是上传文件，我们知道，Xshell 是直接将 FTP 分离了出去，我总觉得这个产品分割设计很脑残，放在一起挺好的

WindTerm 是放在一起的，直接打开文件文件管理器，选择文件上传还是直接拖拽，都非常便利

![2022-10-25_44436.png](/2022-10-25_44436.png)





