---
title: wine使用教程安装MicrosoftVisio2013
description: 
published: true
date: 2022-07-06T01:02:32.863Z
tags: visio
editor: markdown
dateCreated: 2022-07-06T01:02:30.390Z
---

# wine使用教程安装MicrosoftVisio2013

之前分享了关于在Deepin/UOS家庭版上安装Microsoft Office 2013的教程。有人说需要Visio，今天楼主开一帖介绍一下。

楼主开帖重在介绍利用wine安装运行exe软件的方法，并不在于介绍个别软件。楼主在教程7里面把方法都介绍得十分清楚，学会举一反三，稍微变通一下，我相信每个人都会用同样的方法安装运行exe程序。

本帖用的是教程7的容器和wine版本，学习本帖前，请先学习：[wine使用教程7-借用容器和wine版本安装Microsoft Office2013](https://bbs.deepin.org/post/239589)

## 方法步骤：

## 一、下载Microsoft Visio 2013安装镜像并解压

![截图_选择区域_20220705184005.jpg](https://storage.deepin.org/thread/202207051840472686_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220705184005.jpg)

Microsoft Visio 2013安装文件cn_visio_professional_2013_x86_1138439.exe放在下载文件夹（~/Downloads）

## 二、安装星火应用商店战网客户端并首次运行（详见教程7）

## 三、复制容器和wine版本并改名（详见教程7）

## 四、设置容器Spark-Office的windows版本（详见教程7）

## 五、安装Gecko（详见教程7）

## 六、安装mono（详见教程7）

## 七、安装Microsoft Visio 2013

终端命令：

```
WINEPREFIX=~/.deepinwine/Spark-Office ~/.deepinwine/Lwine7.1-my/bin/wine ~/Downloads/cn_visio_professional_2013_x86_1138439.exe
```

上述命令结构解析：

（1）字段1：WINEPREFIX=是指定的容器路径

（2）字段2：~/.deepinwine/Lwine7.1-my/bin/wine是你所调用的wine的路径

（3）字段3：最后接你要运行的exe程序的路径

注意：不同字段之间有一个空格（英文输入法）。

弹出Visio的安装引导界面后，按提示操作安装即可。

![截图_deepin-terminal_20220705181932.jpg](https://storage.deepin.org/thread/202207051843414187_%E6%88%AA%E5%9B%BE_deepin-terminal_20220705181932.jpg)

![截图_deepin-terminal_20220705182357.jpg](https://storage.deepin.org/thread/202207051843501462_%E6%88%AA%E5%9B%BE_deepin-terminal_20220705182357.jpg)

## 八、测试运行

终端命令：

```
WINEPREFIX=~/.deepinwine/Spark-Office ~/.deepinwine/Lwine7.1-my/bin/wine "c:/Program Files (x86)/Microsoft Office/Office15/VISIO.EXE"
```

上述命令结构解析：

（1）字段1：WINEPREFIX=是指定的容器路径

（2）字段2：~/.deepinwine/Lwine7.1-my/bin/wine是你所调用的wine的路径

（3）字段3：最后接英文双引号，双引号内是你要运行的exe程序在容器drive_c（即模拟的c盘）中的路径。

## 九、制作桌面图标

在桌面新建一个txt文件，命名为MSVISIO.txt，复制以下内容到txt文件里：

```
[Desktop Entry]
Categories=Application
Exec=sh -c 'WINEPREFIX=/home/$USER/.deepinwine/Spark-Office /home/$USER/.deepinwine/Lwine7.1-my/bin/wine "c:/Program Files (x86)/Microsoft Office/Office15/VISIO.EXE"'
Icon=MSVISIO
MimeType=
Name=Visio
StartupNotify=true
Type=Application
X-Deepin-Vendor=user-custom
```

保存退出txt，右键重命名，把这个txt文件的后缀改为desktop，最终文件名为：MSVISIO.desktop

![截图_选择区域_20220705183433.jpg](https://storage.deepin.org/thread/20220705184608604_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220705183433.jpg)

注：

Exec= ————sh -c '字段1：用WINEPREFIX指定容器路径 字段2：wine的路径 "字段3：exe程序在虚拟C盘里的路径"' 注意这里有一个单引号和一个双引号（英文输入法）。

Icon= ————指图标路径，如果图标在/usr/share/icons/hicolor/scalable/apps文件夹，就不用写完整路径，只需要写图标文件的文件名（不写文件后缀）。楼主把svg图标已经制作好了，你可以直接下载使用[MSVISIO.zip](https://storage.deepin.org/thread/202207051846463421_MSVISIO.zip)。下载解压，然后里面的svg图标复制到/usr/share/icons/hicolor/scalable/apps下。（注意，需要在apps文件夹里右键——以管理员身份打开）

Name= ————图标文件显示的名称

**特别说明，Exec=后面不能用~来代替/home/$USER**

桌面图标如果显示不出来，可以注销系统再重新登录进入系统，图标就显示正常了。

## 十、双击运行桌面图标测试一下

成功运行

![截图_visio.exe_20220705182903.jpg](https://storage.deepin.org/thread/202207051848128193_%E6%88%AA%E5%9B%BE_visio.exe_20220705182903.jpg)

![截图_visio.exe_20220705183024.jpg](https://storage.deepin.org/thread/202207051848188398_%E6%88%AA%E5%9B%BE_visio.exe_20220705183024.jpg)

## 十一、字体问题（详见教程7）

## 十二、收尾工作——清理Spark-Office里战网客户端的文件夹（详见教程7）

------

## 另外，补充一下。如果安装了这个帖子[Microsoft Office 2013首次打包-限时体验](https://bbs.deepin.org/post/239932)的deb包的话，本帖第二至第六步、第十二步可以省略，但注意要将本帖第七至第九步里面的Lwine7.1-my替换为Lwine-7.1。