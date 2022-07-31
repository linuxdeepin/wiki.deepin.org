---
title: Wine 运行器
description: Wine 运行器
published: true
date: 2022-07-31T02:38:36.159Z
tags: wine, wine exe
editor: markdown
dateCreated: 2022-07-23T01:44:40.008Z
---

这个程序用到的帖子均在程序谢明中标注，如果有遗漏请尽快与我联系添加，我对此表示深深的歉意

![image.png](https://storage.deepin.org/thread/202207301819019681_image.png)



# 介绍

一个图形化了以下命令的程序

```bash
WINEPREFIX=容器路径 wine（wine的路径） 可执行文件路径
```

而打包器可以方便的把您的 wine 容器打包成 deb 包供他人使用，程序创建的 deb 构建临时文件夹目录树如下：

```bash
/XXX
├── DEBIAN
│   └── control
└── opt
└── apps
    └── XXX
        ├── entries
        │   ├── applications
        │   │   └── XXX.desktop
        │   └── icons
        │       └── hicolor
        │           └── scalable
        │               └── apps
        │                   └── XXX.png（XXX.svg）
        ├── files
        │   ├── files.7z
        │   └── run.sh
        └── info

11 directories, 6 files
```

让你可以简易方便的使用 wine
是使用 Python3 的 PyQt5 构建的
（测试平台：deepin 20.6 1030；UOS 家庭版 21.3；Ubuntu 22.04；优麒麟 22.04）

## 更新日志

**※1、更换为 @PossibleVing 提供的程序图标**
**※2、修改了统信 Wine 生态适配活动的脚本，支持在非 UOS 系统打包**
**※3、修复了打包器在打包应用未指定图标的情况下显示对话框后强制退出的问题**
4、修改 .net framework 3.5 的安装包，从在线版改为本地版
5、支持设置主题
6、添加 Geek Uninstaller 手动升级脚本

# 截图

![image.png](https://storage.deepin.org/thread/202207291533597182_image.png)

## 使用说明

### 均在软件的“小提示”里有说明

### 运行器

1、使用终端运行该程序，可以看到 wine 以及程序本身的提示和报错;
2、wine 32 位和 64 位的容器互不兼容;
3、所有的 wine 和 winetricks 均需要自行安装（可以从 菜单栏=>程序 里面进行安装）
4、本程序支持带参数运行 wine 程序（之前版本也可以），只需要按以下格式即可：

```bash
exe路径\' 参数 \'
```

即可（单引号需要输入）
5、wine 容器如果没有指定，则会默认为 ~/.wine

### 打包器

1、deb 打包软件包名要求：
软件包名只能含有小写字母(a-z)、数字(0-9)、加号(+)和减号(-)、以及点号(.)，软件包名最短长度两个字符；它必须以字母开头
2、如果要填写路径，有“浏览……”按钮的是要填本计算机对应文件的路径，否则就是填写安装到其他计算机使用的路径
3、输入 wine 的容器路径时最后面请不要输入“/”
4、输入可执行文件的运行路径时是以“C:/XXX/XXX.exe”的格式进行输入，默认是以 C： 为开头，不用“\”做命令的分隔，而是用“/”
5、.desktop 的图标只支持 PNG 格式和 SVG 格式，其他格式无法显示图标

### 基于统信 Wine 生态适配脚本的打包器

第一个文本框是应用程序中文名
第二个文本框是应用程序英文名
第三个文本框是最终生成的包的描述
第四个选择框是desktop文件中的分类
第五个输入框是程序在 Wine 容器的位置，以 c:\\XXX 的形式，盘符必须小写，用反斜杠，如果路径带用户名的话会自动替换为$USER
而 StartupWMClass 字段将会由程序自动生成，作用如下：
desktop文件中StartupWMClass字段。用于让桌面组件将窗口类名与desktop文件相对应。这个值为实际运行的主程序EXE的文件名，wine/crossover在程序运行后会将文件名设置为窗口类名
第六个输入框是最终生成的包的包名,包名的命名规则以deepin开头，加官网域名（需要前后对调位置），如还不能区分再加上应用名
最后一个是最终生成的包的版本号，版本号命名规则：应用版本号+deepin+数字

# 下载链接

Gitee：https://gitee.com/gfdgd-xi/deep-wine-runner
Github：https://github.com/gfdgd-xi/deep-wine-runner
Gitlink：https://www.gitlink.org.cn/gfdgd_xi/deep-wine-runner
蓝奏云：https://gfdgdxi.lanzouj.com/b01nz7y3e，密码:7oii
星火应用商店：spk://store/tools/spark-deepin-wine-runner


