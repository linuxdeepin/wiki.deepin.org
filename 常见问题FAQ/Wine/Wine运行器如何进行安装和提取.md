---
title: Wine运行器如何进行安装和提取
description: 
published: true
date: 2022-07-06T02:16:16.147Z
tags: wine
editor: markdown
dateCreated: 2022-07-06T02:15:00.290Z
---

# Wine运行器如何进行安装和提取

一个图形化了以下命令的程序

```bash
WINEPREFIX=容器路径 wine（wine的路径） 可执行文件路径
```

让你可以简易方便的使用 wine
是使用 Python3 的 tkinter 构建的
（自己美术功底太差，图标只能在网络上找了）
（测试平台：deepin 20.6 1030；UOS 家庭版 21；Ubuntu 22.04）

## 更新日志

**※1、支持打开 spark-wine7-devel 的专门缩放设置（如未安装则此按钮禁用）**
**※2、支持提取选择的 exe 文件的图标**
**※3、支持向指定的 wine 容器安装 mono、gecko、.net framework（此功能在菜单栏“Wine”中，卸载只需要使用程序的卸载按钮打开 Geek Uninstaller 即可）**
**※4、支持指定特定的 wine 容器调用 winetricks**
**※5、在没有指定 wine 容器的情况下，将自动设置为 ~/.wine**
6、新增 ukylin-wine
7、将默认选择的 wine 改为 deepin-wine6 stable
8、支持打开指定容器的 winecfg、winver、regedit、taskmgr
9、双击使用 wine 运行器打开 exe（不知道能不能生效）

# 截图

![image.png](https://storage.deepin.org/thread/202207042234078682_image.png)

## 使用说明

（均在软件的“小提示”里有说明）
1、使用终端运行该程序，可以看到 wine 以及程序本身的提示和报错;
2、wine 32 位和 64 位的容器互不兼容;
3、所有的 wine 和 winetricks 均需要自行安装（可以从 菜单栏=>程序 里面进行安装）
4、本程序支持带参数运行 wine 程序（之前版本也可以），只需要按以下格式即可：

```bash
exe路径\' 参数 \'
```

即可（单引号需要输入）
5、wine 容器如果没有指定，则会默认为 ~/.wine

# 下载链接

Gitee：https://gitee.com/gfdgd-xi/deep-wine-runner
Github：https://github.com/gfdgd-xi/deep-wine-runner
Gitlink：https://www.gitlink.org.cn/gfdgd_xi/deep-wine-runner
蓝奏云：https://gfdgdxi.lanzouj.com/b01nz7y3e，密码:7oii
星火应用商店（好像截图传错版本，无伤大雅）：spk://store/tools/spark-deepin-wine-runner