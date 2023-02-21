---
title: Wine 运行器
description: Wine 运行器
published: true
date: 2023-01-09T02:53:04.799Z
tags: wine, wine exe
editor: markdown
dateCreated: 2022-07-23T01:44:40.008Z
---

这个程序用到的帖子均在程序谢明中标注，如果有遗漏请尽快与我联系添加，我对此表示深深的歉意

![图片.png](https://storage.deepin.org/thread/202212112154463787_图片.png)

![图片.png](https://storage.deepin.org/thread/202212112155056532_图片.png)

:tail:

# 介绍

Wine运行器是一个能让Linux用户更加方便地运行Windows应用的程序，内置了对Wine图形化的支持、各种Wine工具、自制的Wine程序打包器和运行库安装工具等。
它同时还内置了基于VirtualBox制作的、专供小白使用的Windows虚拟机安装工具，可以做到只需下载系统镜像并点击安装即可，无需考虑虚拟机的安装、创建、分区等操作。
此外，它还简化了如下命令，让你可以更简便地使用Wine：

```bash
env WINEPREFIX=容器路径 wine（wine的路径） 可执行文件路径
```

让你可以简易方便的使用 wine
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
（测试平台：deepin 20.8；UOS 家庭版 21.3.1；Ubuntu 22.04；优麒麟 22.04；deepin 23；openkylin）

## 更新日志

**※1、支持使用 Qemu + Chroot 跨运行 Wine 以及指定程序的功能；**
**※2、提供了简易打包器以用于打包简易 deb；**
**※3、支持下载配置过的 Qemu + Chroot 容器；**
**※4、支持在隔离的 Chroot 容器内运行 Wine；**
**※5、支持解压指定 deb 的内打包好的容器；**
**※6、优化 Wine 列表显示；**
**※7、新增程序论坛和教程入口；**
**※8、程序公告功能；**
**※9、新增程序评分功能；**
**※10、新增解包 deb 内 Wine 容器功能；**
**※11、新增 Vkd3d Proton 安装功能，更新 dxvk 版本至 2.0.0；**
**※12、新增程序菜单栏部分栏目图标；**
**※13、打包器支持按下 Shift + F1 查看指定选项提示；**
14、优化非基于生态适配脚本的打包器内容自动填充功能；
15、优化程序文案；
16、新增日志翻译功能；
17、程序进一步完善英语翻译（机翻）；
18、优化程序更新策略；
19、优化日志分析功能；
20、优化程序 UI。

# 截图

![image.png](https://storage.deepin.org/thread/202212102108356218_image.png)

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
6、如果可执行文件比较大的话，会出现点击“获取该程序运行情况”出现假死的情况，因为正在后台读取 SHA1，只需要等一下即可（读取速度依照您电脑处理速度、读写速度、可执行文件大小等有关）
7、对于非 X86 的用户来说，请不要使用本程序自带的 Wine 安装程序和 Windows 虚拟机安装功能（检测到为非 X86 架构会自动禁用）
8、如果非 X86 的用户的 UOS 专业版用户想要使用的话，只需要在应用商店安装一个 Wine 版本微信即可在本程序选择正确的 Wine 运行程序
9、在使用 linglong 包的 Wine 应用时，必须安装至少一个 linglong 的使用 Wine 软件包才会出现该选项，而程序识别到的 Wine 是按 linglong 的使用 Wine 软件包名的字母排序第一个的 Wine，且生成的容器不在用户目录下，而是在容器的用户目录下（用户目录/.deepinwine、/tmp、桌面、下载、文档等被映射的目录除外），同理需要运行的 EXE 也必须在被映射的目录内
10、如果是使用 Deepin 23 的 Wine 安装脚本，请切记——安装过程会临时添加 Deepin 20 的 apt 源，不要中断安装以及千万不要中断后不删除源的情况下 apt upgrade ！！！中断后只需重新打开脚本输入 repair 或者随意安装一个 Wine（会自动执行恢复操作）即可。以及此脚本安装的 Wine 无法保证 100% 能使用，以及副作用是会提示

```bash
N: 鉴于仓库 'https://community-packages.deepin.com/beige beige InRelease' 不支持 'i386' 体系结构，跳过配置文件 'main/binary-i386/Packages' 的获取。
```

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

# 在 Openkylin 安装方法

首先添加作者的源：

Gitlink 源（国内推荐）：

```
wget https://code.gitlink.org.cn/gfdgd_xi/gfdgd-xi-apt-mirrors/raw/branch/master/sources/gitlink.sh && bash gitlink.sh && rm gitlink.sh
```

Github 源（国外推荐）：

```
wget https://gfdgd-xi.github.io/gfdgd-xi-apt-mirrors/sources/github.sh && bash github.sh && rm github.sh
```

上面二选一，添加完后执行

```bash
sudo apt install spark-deepin-wine-runner
```

即可自动补全依赖安装（说实话 openkylin 缺的依赖好多）

# 下载链接

Gitee：https://gitee.com/gfdgd-xi/deep-wine-runner
Github：https://github.com/gfdgd-xi/deep-wine-runner
Gitlink：https://www.gitlink.org.cn/gfdgd_xi/deep-wine-runner
蓝奏云：https://gfdgdxi.lanzouj.com/b01nz7y3e，密码:7oii
奇怪的链接（需要有 IPv6 才能访问）：http://gfdgdxi.msns.cn/gfdgd-xi-apt-mirrors/program/spark-deepin-wine-runner/
星火应用商店：spk://store/tools/spark-deepin-wine-runner
程序官网：https://gfdgd-xi.github.io/
支持程序自带的更新程序进行更新

<iframe src="https://deepin-community-store.gitee.io/spk-resolv/?spk=spk://store/tools/spark-deepin-wine-runner" height="400" width="100%" border="0"></iframe>

[![star](https://gitee.com/gfdgd-xi/deep-wine-runner/badge/star.svg?theme=dark)](https://gitee.com/gfdgd-xi/deep-wine-runner/stargazers)
:tail:
**最后说一下，如果想要在商业环境使用此APP，因为程序内附商业软件，请保证获得相关厂家授权或移除相关组件（移除用程序自带的删除组件功能即可）**
**以及本程序在非 X86 架构上测试较少，可能容易翻车，建议不要在办公环境使用**
**现在新增了通过SHA1值获取应用适配情况的功能，查看链接：https://gfdgd-xi.github.io/wine-runner-info/，如何贡献自己的适配情况？在 Wine 运行器进行评分即可**
**自动配置文件的脚本如何编写/贡献？可见程序 Wiki：https://gfdgd-xi.github.io/wine-runner-wiki/ 如果想要贡献请按照 Wiki 的要求进行Pr**
**感谢由 [@Allen](https://bbs.deepin.org/user/290514) 制作的论坛： [https://gfdgdxi.flarum.cloud/](https://gfdgdxi.flarum.cloud/) ，欢迎使用**

