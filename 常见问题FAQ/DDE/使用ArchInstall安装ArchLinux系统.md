---
title: 用archinstall自动化脚本安装Arch Linux
description: 
published: true
date: 2022-09-14T04:05:37.353Z
tags: arch
editor: markdown
dateCreated: 2022-09-14T02:18:42.542Z
---

# 用archinstall自动化脚本安装Arch Linux

上次学习了如何在 ArchLinux中安装深度桌面（DDE）后，有很多论坛的老铁交流说可以使用archinstall来玩一把更方便快捷，后续尝试了一波，把尝试过程记录并分享如下

**历史帖子：如何在 ArchLinux中安装深度桌面（DDE）**

**https://bbs.deepin.org/post/242841**

`archinstall`是一个用于安装 Arch Linux 的帮助库。它和其它的预配置安装程序一起打包

`archinstall`以纯文本形式存储所有用户和（辅助）磁盘加密的密码
`archinstall`的默认配置与安装指南不同。如使用 archinstall 安装系统出现问题的话，请在反馈中注明，并提供`/var/log/archinstall/install.log`


## 以UEFI方式启动为例运行安装程序
![2022-9-14_25062.png](/2022-9-14_25062.png)

**首先，按照安装指南#安装前的准备中的启动到 Live 环境操作。archinstall 包是 live 镜像的一部分,可以直接运行**

![2022-9-14_84223.png](/2022-9-14_84223.png)

**加载完成后**

![2022-9-14_96134.png](/2022-9-14_96134.png)

## 指导程序将执行（或查询）以下步骤
```
配置区域
选择镜像
分区磁盘
格式化分区
设置 root 密码
安装引导加载程序
```
## 磁盘分区及格式化配置
**选择Drive(s)选项后进入,选择/dev/sda磁盘选项**

![2022-9-14_50101.png](/2022-9-14_50101.png)

**选择Disk layout选项后进入**

![2022-9-14_79657.png](/2022-9-14_79657.png)

**可以直接选择系统建议分区方式**

![2022-9-14_27829.png](/2022-9-14_27829.png)

**选择磁盘格式**

![2022-9-14_76380.png](/2022-9-14_76380.png)

![2022-9-14_43436.png](/2022-9-14_43436.png)

**配置完成后保存退出**

![2022-9-14_72584.png](/2022-9-14_72584.png)


## 其他相关参数选项配置

**此处演示安装的是DDE桌面环境，添加的安装包也是deepin相关**

1. Mirror region配置为`China`
2. Root password设置`root账户的密码`
3. Audio配置为`pulseaudio`
4. Additional packages配置增加额外的安装包有：`grub efibootmgr sudo vim ttf-dejavu deepin deepin-extra lightdm xorg-server deepin-kwin networkmanager`
5. Network configuration配置为`Use NetworkManager`
6. Timezone配置为`Asia/Shanghai`


![2022-9-14_31335.png](/2022-9-14_31335.png)

## Additional packages配置额外的安装包

![2022-9-14_1790.png](/2022-9-14_1790.png)

## 配置完成后开始安装即可
![2022-9-14_20326.png](/2022-9-14_20326.png)

![2022-9-14_48341.png](/2022-9-14_48341.png)

![2022-9-14_76299.png](/2022-9-14_76299.png)
