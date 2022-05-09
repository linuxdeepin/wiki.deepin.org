---
title: 待分类/在自定义的_btrfs_subvolume_上安装_Deepin
description: 
published: true
date: 2022-05-07T07:49:28.217Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:47:22.157Z
---

# 简介
本经验由深度论坛用户(Dongriyangguang )分享

[原文地址](https://bbs.deepin.org/forum.php?mod=viewthread&tid=135172)

# 正文

受@MattD 的[《在自定义的 btrfs+subvolume 上安装 Deepin 2014》](https://iammattd.github.io/2015/05/07/install-deepin-2014-onto-customized-btrfs-with-subvolume.html) 这篇博客的启发，我尝试将这样的安装方式应用到 Deepin 15 上，并获得了成功。操作的思路、步骤与原文基本一致，略有不同的是我是在虚拟机中安装的，使用了 UEFI 来引导，一些细节上有所调整。其实在 Deepin 2013 的时候就有人做过这样的尝试了：将LD安装到自定义的btrfs+subvolume。

社区发帖有字数限制，感兴趣的朋友可以到我的博客详细查看：在自定义的 btrfs+subvolume 上安装 Deepin15 (blog.csdn.net/elegant__)，也方便我更新补充。
限于水平和精力，安装后也还存在一些问题，比如：

- 安装后系统的语言和时区可能有问题，可以通过控制中心调整。

- 平级结构的子卷在重新挂在的时候失败，仅试验了树形结构。

- 系统启动和关闭时没有出现 deepin 的动画。

- 普通用户下 ping 命令没有权限，可以通过 sudo chmod u+s /bin/ping 解决。

- 如果有朋友更清楚其中的原因和解决办法，也忘不吝赐教。

- 再次感谢@MattD和 @woodelf 的工作。
