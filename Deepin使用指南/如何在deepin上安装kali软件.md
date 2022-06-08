---
title: Deepin上安装kali软件
description: 
published: true
date: 2022-06-08T08:01:58.523Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:47:44.610Z
---

## 简介

本经验由深度用户(geange)分享，[原文地址](https://bbs.deepin.org/forum.php?mod=viewthread&tid=136057&extra=)
由deepin用户ExplosiveBattery补充

## 正文

相必有些人会知道一个项目，专门为debian或者ubuntu安装来自于kali的软件katoolin（ExplosiveBattery推荐使用katoolin，<https://github.com/LionSec/katoolin>，因为这个项目会有人维持着更新）

我也有尝试过使用这个软件，但是用python写的一个小脚本让我觉得它的界面实在是不太好看，所以参考了katoolin项目的代码后，我用bash重写这个项目。加上了bash中whiptail，让界面更加好看（话说好不好看也是看个人吧）

先贴katoolin一下项目的地址吧 <https://github.com/geange/kaliTools>
这个项目是deepin linux上进行的（不得不说deepin这个国产linux在图形界面做的相当不错，以至于我都抛弃了习惯的ubuntu）。

如何使用

        git clone git@github.com</a>:geange/kaliTools.git
        cd kaliTools/
        chmod +x index.sh
        sudo ./index.sh 

你将会看到初始界面

选择第一个，按下Enter键，kaliTools将会添加kali的源到你本机的系统中。

- 第二个选项是删除kali的源，建议更新系统时删除kali的源，以免出现依赖冲突的问题
- 第三个选项是批量安装kali分类的软件，按下enter键进入后会出现选项，可以选择单选或者多选（按下空格键可以勾选或者取消勾选）
- 第四个选项是按需要安装kali的软件，需要哪一个就安装哪一个
- 第五个选项退出软件（我对bash还不太熟悉，不知道怎么按下cancel键退出。。。） （进入第三或者第四个选项时，如果需要退出，按下ESC键即可）

如果觉得不错，请帮我在github点个星，谢谢了^-^。有BUG也可以联系我，邮箱geange@outlook.com

我的项目地址

<https://github.com/geange/kaliTools>
