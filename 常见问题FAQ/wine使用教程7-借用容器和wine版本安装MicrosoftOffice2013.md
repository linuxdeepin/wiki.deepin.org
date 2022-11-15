---
title: wine使用教程7-借用容器和wine版本安装MicrosoftOffice2013
description: 
published: true
date: 2022-11-15T08:11:32.591Z
tags: office20113 wine
editor: markdown
dateCreated: 2022-11-15T08:04:53.630Z
---

# wine使用教程7-wine版本安装Office2013

https://bbs.deepin.org/post/239589

## wine使用教程
### 第7辑：在Deepin/UOS家庭版借用容器和wine版本安装Microsoft Office2013的方法

之前给大家介绍过如何利用deepin-wine6-stable安装Microsoft Office 2013的方法（详见教程第6辑）。经测试，此方法安装的Microsoft Office 2013无法连接服务器，以致于无法输入激活密钥

![2022-11-15_66620.png](/2022-11-15_66620.png)


就此，楼主换一种思路，借用星火商店战网客户端的容器和wine版本来安装Microsoft Office 2013。经测试，可以连接服务器了。方法如下：

说明：利用第6辑和第7辑方法安装的Microsoft Office 2013的PowerPoint和OneNote无法使用，暂未找到解决方法。

### 一、下载Microsoft Office2013安装镜像并解压
以下教学所用Microsoft Office2013安装镜像（cn_office_professional_plus_2013_x86_x64_dvd_1149708.iso）从MSDN网站下载。


![2022-11-15_30808.png](/2022-11-15_30808.png)


Microsoft Office2013安装镜像iso文件放在下载文件夹（~/Downloads）

右键解压

![2022-11-15_12477.png](/2022-11-15_12477.png)


### 二、安装星火应用商店战网客户端并首次运行

安装星火商店里的战网客户端，安装好后一定要双击战网的图标运行一次（直到出现战网客户端账号登录界面，就可以关闭客户端了）。首次运行将建立战网客户端的容器（Deepin-Battlenet文件夹）以及wine版本（Lwine7.1文件夹）。

![2022-11-15_99347.png](/2022-11-15_99347.png)

### 三、复制容器和wine版本并改名
（1）复制Deepin-Battlenet容器并改名Spark-Office

（2）复制Lwine7.1并改名为Lwine7.1-my

![2022-11-15_55798.png](/2022-11-15_55798.png)

### 四、设置容器Spark-Office的windows版本
终端命令：

WINEPREFIX=~/.deepinwine/Spark-Office ~/.deepinwine/Lwine7.1-my/bin/winecfg
在弹出的wine设置窗口，将windows版本设置为windows7


![2022-11-15_94484.png](/2022-11-15_94484.png)

### 五、安装Gecko
终端命令：

32位Gecko：

WINEPREFIX=~/.deepinwine/Spark-Office ~/.deepinwine/Lwine7.1-my/bin/wine msiexec /i ~/.deepinwine/Lwine7.1-my/gecko/wine-gecko-2.47.2-x86.msi

![2022-11-15_7233.png](/2022-11-15_7233.png)

64位gecko：

WINEPREFIX=~/.deepinwine/Spark-Office ~/.deepinwine/Lwine7.1-my/bin/wine msiexec /i ~/.deepinwine/Lwine7.1-my/gecko/wine-gecko-2.47.2-x86_64.msi

![2022-11-15_55924.png](/2022-11-15_55924.png)

### 六、安装mono
终端命令：

WINEPREFIX=~/.deepinwine/Spark-Office ~/.deepinwine/Lwine7.1-my/bin/wine msiexec /i ~/.deepinwine/Lwine7.1-my/mono/wine-mono-7.1.1-x86.msi


























