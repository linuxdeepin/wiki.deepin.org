---
title: 6-用deepin-wine6-stable安装Office2013
description: 
published: true
date: 2022-10-03T15:02:02.284Z
tags: wine
editor: markdown
dateCreated: 2022-06-27T02:03:19.442Z
---

注：建立deepin-wine6-stable环境：新装的系统需要安装一款应用商店里使用deepin-wine6-stable运行的wine应用（如wine版微信、wine版QQ），并运行一下。（系统会自动建立deepin-wine6-stable环境）

## 一、下载Microsoft Office 2013安装镜像并解压

以下教学所用Microsoft Office 2013安装镜像（cn_office_professional_plus_2013_x86_x64_dvd_1149708.iso）从[MSDN网站下载](https://msdn.itellyou.cn/)。

![1](https://storage.deepin.org/thread/202206262305275758_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220626230515.png)

Microsoft Office2013安装镜像iso文件放在下载文件夹（~/Downloads）

右键解压

![2](https://storage.deepin.org/thread/202206262313334838_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220626201945.png)

## 二、新建容器

新建一个32位的windows7容器（容器名为Deepin-Office）

终端命令：

```bash
WINEARCH=win32 WINEPREFIX=~/.deepinwine/Deepin-Office deepin-wine6-stable winecfg
```

上述命令结构解析：

1. `WINEARCH=`后面写win32，即表示新建一个32位的容器，如果写win64，即表示新建一个64位的容器。（目前wine对32位程序支持较好，若无特殊情况，建议新建32位容器）

2. `WINEPREFIX=`是指定的容器路径

3. `deepin-wine6-stable`是你使用的wine

4. `winecfg`是调出wine设置

**注意：不同字段之间有一个空格（英文输入法），下同。**

![3](https://storage.deepin.org/thread/202206262307333460_%E6%88%AA%E5%9B%BE_deepin-terminal_20220626223500.png)

容器新建好后，就会在~/.deepinwine下多一个Deepin-Office的文件夹，这个文件夹就是模拟的windows系统环境，被称为wine容器

![4](https://storage.deepin.org/thread/202206262335277797_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220626231116.png)

## 三、安装Microsoft Office 2013

终端命令：

```bash
WINEPREFIX=~/.deepinwine/Deepin-Office deepin-wine6-stable ~/Downloads/cn_office_professional_plus_2013_x86_x64_dvd_1149708/setup.exe
```

上述命令结构解析：

1. `WINEPREFIX=`是指定的容器路径

2. `deepin-wine6-stable`是你使用的wine

3. 最后接你要运行的exe程序的路径

![5](https://storage.deepin.org/thread/202206262317377490_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220626223623.png)

弹出Office的安装引导界面后，按提示操作安装即可。

## 四、测试运行

终端命令：

```bash
WINEPREFIX=~/.deepinwine/Deepin-Office deepin-wine6-stable "c:/Program Files/Microsoft Office/Office15/WINWORD.EXE"
```

上述命令结构解析：

1. `WINEPREFIX=`是指定的容器路径

2. `deepin-wine6-stable`是你使用的wine

3. 最后接英文双引号，双引号内是你要运行的exe程序在容器drive_c（即模拟的c盘）中的路径，这里测试的Word

![6](https://storage.deepin.org/thread/202206262320599265_%E6%88%AA%E5%9B%BE_winword.exe_20220626224510.png)

## 五、制作桌面图标

以Access的图标为例，Word、Excel、PowerPoint的图标制作方法一样，就不一一介绍了。

在桌面新建一个txt文件，命名为MSACCESS.txt，复制以下内容到txt文件里：

```ini
[Desktop Entry]
Categories=Application
Exec=sh -c 'WINEPREFIX=/home/$USER/.deepinwine/Deepin-Office deepin-wine6-stable "c:/Program Files/Microsoft Office/Office15/MSACCESS.EXE"'
Icon=1F9D_MSACCESS.0
MimeType=
Name=Access
StartupNotify=true
Type=Application
X-Deepin-Vendor=user-custom
```

保存退出txt，右键重命名，把这个txt文件的后缀改为desktop

![7](https://storage.deepin.org/thread/202206262324484109_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220626232435.png)

注：

Exec= ————sh -c 'WINEPREFIX=容器路径 deepin-wine6-stable "exe主程序路径在虚拟C盘里的路径"'

Icon= ————指图标路径，如果图标在~/.local/share/icons/hicolor文件夹（及其子文件夹），就不用写完整路径，只需要写图标文件的文件名（不写文加后缀）。使用deepin-wine6-stable安装的exe程序，deepin-wine6-stable会将exe程序的图标文件放到~/.local/share/icons/hicolor的子文件夹里，是系统随机命名的。你只需要在hicolor文件夹搜索就可以找到了，然后把搜索出的文件名填到Icon=这一栏即可。（注意，上面的Icon=后面的文件名需要你自己改的）

![8](https://storage.deepin.org/thread/202206262330056916_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220626233000.png)

Name= ————图标文件显示的名称，这里填Access

**特别说明，Exec=后面不能用`~`来代替`/home/$USER`**

## 六、双击运行桌面图标测试一下

成功运行

![9](https://storage.deepin.org/thread/20220626232915849_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220626224706.png)

## 七、字体问题

为了一劳永逸解决wine应用字体显示乱码、方块、显示不出等问题，建议你安装星火应用商店里的“Win字体”

## 效果图：

![10](https://storage.deepin.org/thread/202206262349099875_%E5%BD%95%E5%B1%8F_dde-desktop_20220626234757.gif)

![11](https://storage.deepin.org/thread/202206262350398006_%E5%BD%95%E5%B1%8F_dde-desktop_20220626234953.gif)

![12](https://storage.deepin.org/thread/202206270000054669_%E5%BD%95%E5%B1%8F_dde-desktop_20220626235925.gif)


