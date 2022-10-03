---
title: 2-用deepin-wine6安装/运行exe程序的方法
description: 
published: true
date: 2022-10-03T14:58:27.556Z
tags: wine
editor: markdown
dateCreated: 2022-06-20T08:34:34.911Z
---

注：

1. 建立deepin-wine6-stable环境：新装的系统需要安装一款应用商店里使用deepin-wine6-stable运行的wine应用（如wine版微信、wine版QQ），并运行一下。（系统会自动建立deepin-wine6-stable环境）

2. 以下教学以32位7-Zip的安装程序7z2107.exe（版本21.7.0.0）为案例软件，该exe程序我放在系统下载（Downloads）文件夹，即/home/$USER/Downloads/7z2107.exe，或者写作~/Downloads/7z2107.exe

多数情况下，`/home/$USER`与`~`可以相互替代。

## 一、安装exe程序

### 安装程序到一个新建的容器

终端命令：

```bash
WINEARCH=win32或者wine64 WINEPREFIX=容器路径 deepin-wine6-stable 需安装/运行的exe软件的路径
````

案例：

```bash
WINEARCH=win32 WINEPREFIX=~/.deepinwine/Wine-7zip deepin-wine6-stable ~/Downloads/7z2107.exe
```

（注：如果提示安装mono，可点取消。mono模拟的.NET Framework，不是所有exe软件都需要这个。需要的时候你再手动安装即可）

上述命令结构解析：

1. `WINEARCH=`后面写win32，即表示新建一个32位的容器，如果写win64，即表示新建一个64位的容器。

2. `WINEPREFIX=`是指定的容器路径（此处Wine-7zip就是容器名称，如果容器Wine-7zip不存在，默认会新建这个容器）。

3. 如果要用deepin-wine5，中间可以写成`deepin-wine5`；如果要用deepin-wine5-stable，中间可以写成`~/.deepinwine/deepin-wine5-stable/bin/wine`

4. 最后接的是你要安装的exe安装程序的所在路径。

## 二、运行已安装到容器的exe主程序

终端命令：

```bash
WINEPREFIX=容器路径 deepin-wine6-stable "c:/exe主程序在虚拟C盘（即drive_c）里的路径"
```

案例：

```bash
WINEPREFIX=~/.deepinwine/Wine-7zip deepin-wine6-stable "c:/Program Files/7-Zip/7zFM.exe"
```

## 三、winecfg设置

可设置Windows版本、替换dll函数、窗口修饰、显示分辨率等。

1. 修改Windows版本
  默认的Windows版本是Windows 7，有的exe安装时提示系统版本太低的话，就需要利用winecfg修改为Windows 10。有的exe软件在Windows XP表现更好，就需要用winecfg修改为Windows XP。
  打开winecfg的终端命令：
    ```bash
    WINEPREFIX=容器路径 /opt/deepin-wine6-stable/bin/winecfg
    ```
    案例：
    ```bash
    WINEPREFIX=~/.deepinwine/Wine-7zip /opt/deepin-wine6-stable/bin/winecfg
    ```
    上述命令结构解析：
    `WINEPREFIX=`是指定的容器路径，后面打一个空格，然后输入winecfg所在路径 /opt/deepin-wine6-stable/bin/winecfg
    案例软件7-Zip无需修改Windows版本即可正常运行。

2. 函数顶替
  有的exe软件，无需新增函数顶替。
  有的exe软件，新增以下几个函数顶替基本上就能正常运行了：atl100、mlang、msls31、riched20、usp10
  有的exe软件还需要添加msvcp60、riched32等函数
  案例软件7-Zip无需新增顶替函数即可正常运行。

## 四、字体设置

由于linux系统默认是没有windows常用字体（如Arial、微软雅黑、宋体），所以用wine安装的exe软件大概率会出现字体乱码、字体呈现方块、字体显示不出来等问题。此时，需要设置字体，方法有三种：

### 方法1：直接安装“Win字体”应用（可以与方法2并用）

到星火应用商店里下载安装“Win字体”。安装好后，再调出winecfg（方法如前述），字体选项下勾选“允许加载系统字体”，建议顺便把“允许加载Windows Fonts目录下的字体”也勾上。

### 方法2：复制字体到虚拟C盘的字体文件夹（可以与方法1并用）

将exe软件需要用的字体文件（如宋体的文件为simsun.ttf）复制粘贴到容器的字体文件夹，路径通常为：

~/.deepinwine/容器名称/drive_c/windows/Fonts

案例软件Fonts文件夹路径：~/.deepinwine/Wine-7zip/drive_c/windows/Fonts

调出winecfg（方法如前述），字体选项下勾选“允许加载Windows Fonts目录下的字体”，建议顺便把“允许加载系统字体”也勾上。

### 方法3：修改字体注册表（尽量不要与方法1和方法2并用）
不安装字体，而是在注册表里把需要的字体替换为你系统已有字体。

打开注册表的命令：

```bash
WINEPREFIX=容器路径 /opt/deepin-wine6-stable/bin/regedit
```

案例：

```bash
WINEPREFIX=~/.deepinwine/Wine-7zip /opt/deepin-wine6-stable/bin/regedit
```

找到HKEY_CURRENT_USER\Software\Wine\Fonts\Replacements，如果没有Replacements，新建一个项，项名为Replacements。

然后在Replacements下添加字符串值。如要把宋体（SimSun）替换为思源宋体（Noto Serif CJK SC），则字符串名称为SimSun，值为Noto Serif CJK SC

![1](https://storage.deepin.org/thread/202206101524221794_%E6%88%AA%E5%9B%BE_regedit.exe_20220610152347.png)

一个一个添加字符串太麻烦，楼主做了一个字体替换注册表字体替换自建注册表10.zip，解压后把里面的.reg文件导入注册表即可（regedit界面左上角——注册表——导入注册表文件）。

![2](https://storage.deepin.org/thread/202206101543466844_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220610154327.png)

## 五、winetricks设置

可以添加一些运行时、组件、dll。不是每一个exe软件都需要用到winetricks。案例软件7-zip无需设置winetricks。

安装winetricks的终端命令：

```bash
sudo apt install winetricks
```

运行winetricks的终端命令：

```bash
WINEPREFIX=~/.deepinwine/Wine-7zip winetricks
```

选择默认的wine容器——安装windows DLL和组件，然后根据需要安装运行时、组件、dll。

## 六、给exe软件制作一个桌面图标  

在桌面新建一个txt文件（如7zip.txt），复制以下内容到txt文件里：

```ini
[Desktop Entry]
Categories=Application
Exec=sh -c 'WINEPREFIX=/home/$USER/.deepinwine/Wine-7zip deepin-wine6-stable "c:/Program Files/7-Zip/7zFM.exe"'
Icon=/usr/share/icons/Default/devices/64/media-optical.svg
MimeType=
Name=7-zip
StartupNotify=true
Type=Application
X-Deepin-Vendor=user-custom
```

保存退出txt，右键重命名，把这个txt文件的后缀改为desktop（如7zip.desktop）

![3](https://storage.deepin.org/thread/202206101638494169_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220610163830.png)

注：

Exec= ————sh -c 'WINEPREFIX=容器路径 deepin-wine6-stable "c:/exe主程序路径在虚拟C盘里的路径"'

Icon= ————指图标路径，文件格式一般为png、svg、icon，图标大小64×64为宜，图标格式最好是svg。

Name= ————图标文件显示的名称

以上三项后面的内容可以根据你自己所安装的软件的实际情况更改。

**特别说明，Exec=后面不能用~来代替/home/$USER**

## 七、卸载exe软件  

终端命令：

```bash
WINEPREFIX=容器路径 deepin-wine6-stable "c:/exe软件的卸载程序uninstall.exe的路径"
```

案例:

```bash
WINEPREFIX=~/.deepinwine/Wine-7zip deepin-wine6-stable "c:/Program Files/7-Zip/Uninstall.exe"
```
