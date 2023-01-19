---
title: 用apt-fast加速软件包下载
description: 本文由论坛用户pzm9012分享，原帖地址：https://www.yuque.com/pzm9012/ct5ume/bb7322
published: true
date: 2022-10-03T15:04:19.184Z
tags: apt
editor: markdown
dateCreated: 2022-06-15T01:54:05.622Z
---

# 用apt-fast加速软件包下载
apt-fast是一款命令行工具，能够通过多线程下载软件包来提升下载应用和更新的速度。

项目链接：https://github.com/ilikenwf/apt-fast    本文主要参考安装说明中的Debian and derivates部分。

# 一 基础（新手请认真阅读）
有些用户可能平时主要通过应用商店和控制中心获取应用和更新，因此这里大致介绍一下安装软件包和更新的命令。（下面的命令中带方括号的部分需要根据实际情况自行替换）

## 1. 安装软件包

打开终端，输入以下命令并回车：

`sudo apt install [软件包名]`

之后，终端会列出将要安装的软件包，并询问是否确定执行。此时输入 Y并回车。

注意：这里的 [软件包名]不等于 软件名称。如果不知道要安装软件的软件包名是什么，请在输入以下命令回车：

`apt search --names-only [关键词]`

这里的 [关键词]不支持模糊搜索，因此需要短而准确。

## 2.更新系统和软件

打开终端，输入以下命令并回车：

`sudo apt update && sudo apt full-upgrade`

终端会列出将要更新、保持不变或新安装的软件包，并询问是否确定执行。此时输入 Y并回车。更新过程中有时会询问一些设定问题，我一般是按默认选择。

## 3. 提示

1. 执行开头带 sudo的命令时，会要求输入密码，输入的密码在屏幕上不会有显示，直接输完按回车即可。
2. 如果要跳过更改软件包前的确认操作，请在命令的末尾加上  -y（注意空格）。
3. 执行命令时，按 Ctrl+C 终止运行。

# 二 安装apt-fast

如果系统中已经安装了 星火应用商店 ，可以直接在终端执行命令 sudo apt install apt-fast 来安装它，但安装的apt-fast不是最新版本。

1. 打开文件管理器，进入系统盘的/etc/apt/source.list.d/ 文件夹。右击窗口内空白处，点击“以管理员身份打开”，输入密码。待弹出新窗口后，在文件夹中新建一个任意格式的文件，将它的名称和后缀全部删除，改成 apt-fast.list 。 

1. 打开这个文件，写入以下内容： 然后保存并退出。  

`deb http://ppa.launchpad.net/apt-fast/stable/ubuntu focal main`
`deb-src http://ppa.launchpad.net/apt-fast/stable/ubuntu focal main`

说明：这里使用了 Ubuntu 的 PPA 仓库，有极小的依赖风险。如果后续不需要更新apt-fast可以删除这个文件。

![apt-fast1.png](/apt-fast1.png)

1.打开终端，依次执行以下命令：（建议将命令一条一条地复制到终端中执行）

`apt-key adv --keyserver keyserver.ubuntu.com --recv-keys
A2166B8DE8BDC3367D1901C11EE2FF37CA8DA16B`
`sudo apt update`
`sudo apt install apt-fast`

![apt-fast2.png](/apt-fast2.png)

1. 安装apt-fast时需要进行3项设定。第1项选择apt，第2项和第3项按个人习惯选择，我全部选择默认的选项。  


![apt-fast3.png](/apt-fast3.png)
![apt-fast4.png](/apt-fast4.png)
![apt-fast5.png](/apt-fast5.png)

如果命令执行过程中没有报错或意外终止，那么apt-fast至此安装完成。

# 三 使用apt-fast

如果要使用apt-fast加速软件包下载，把命令中的 apt替换成 apt-fast再执行。一般只在下载软件包或更新时用到apt-fast。

![apt-fast6.png](/apt-fast6.png)
![apt-fast7.png](/apt-fast7.png)

# 四 演示视频

我录制了一个比较简易的视频，演示apt-fast的安装过程。录视频时文章还未写出来，因此演示可能略有差异。

视频链接：
https://www.bilibili.com/video/BV1JZ4y1f7At?share_source=copy_web



由于作者能力有限，若文章存在需要改进之处，或者apt-fast最新版本在deepin上有bug，请指出。