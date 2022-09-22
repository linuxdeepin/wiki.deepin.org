---
title: wine运行器合并打包器和添加从镜像提取dll的功能
description: 
published: true
date: 2022-07-06T07:42:38.197Z
tags: dll wine
editor: markdown
dateCreated: 2022-07-06T07:42:35.546Z
---

# wine运行器合并打包器和添加从镜像提取dll的功能

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
是使用 Python3 的 tkinter 构建的
（自己美术功底太差，图标只能在网络上找了）
（测试平台：deepin 20.6 1030；UOS 家庭版 21；Ubuntu 22.04）

## 更新日志

**※1、添加并翻新了 deepin-wine5 打包器，改为 wine 打包器，支持常见 wine 的打包**
**※2、新增 Visual Studio C++ 的安装程序**
**※3、新增从系统安装镜像提取 DLL 到 wine 容器的功能（当前只支持 Windows XP 和 Windows Server 2003 的官方安装镜像）**
4、修复了安装星火应用商店的 wine 运行器右键可执行文件打开方式没有 wine 运行器选项的问题
5、新增脚本，优化 deepin terminal 调用本程序脚本显示不佳的问题

# 截图

![image.png](https://storage.deepin.org/thread/202207061004446872_image.png)
![image.png](https://storage.deepin.org/thread/202207061005149959_image.png)
![image.png](https://storage.deepin.org/thread/202207061005251446_image.png)

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

# 下载链接

Gitee：https://gitee.com/gfdgd-xi/deep-wine-runner
Github：https://github.com/gfdgd-xi/deep-wine-runner
Gitlink：https://www.gitlink.org.cn/gfdgd_xi/deep-wine-runner
蓝奏云：https://gfdgdxi.lanzouj.com/b01nz7y3e，密码:7oii
星火应用商店：spk://store/tools/spark-deepin-wine-runner