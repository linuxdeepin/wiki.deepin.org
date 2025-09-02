---
title: 我的世界
description: 
published: true
date: 2025-09-02T04:42:40.857Z
tags: 
editor: markdown
dateCreated: 2022-07-02T07:10:37.766Z
---

# 我的世界简介

《我的世界》（Minecraft） 是一款开放世界沙盒游戏，玩家可以自由探索、建造和冒险，支持Java版（跨平台）和基岩版（多设备联机）。  

详细介绍可前往[我的世界中文Wiki](https://zh.minecraft.wiki/)


> 相关链接: 
[Minecraft官网](https://minecraft.net)  [我的世界中文官网](https://mc.163.com)

> 此页面如有错误，欢迎指正
{.is-info}

> 标注 略 的部分可前往我的世界中文Wiki查看
{.is-info}

# Deepin上游玩我的世界Java版

## 硬件需求

> 来自 [Minecraft帮助网站](https://help.minecraft.net/hc/en-us/articles/4409225939853#h_01FFJMSQWJH31CH16H63GX4YKE)
{.is-info}

### 最低配置

- CPU: Intel Core i3-3210 3.2GHz、AMD A8-7600 APU 3.1GHz、Apple M1、龙芯3A4000 1.8GHz 或同等效能的CPU  
- 芯片架构: ARM、x64、x86、LoongArch、RISC-V
- 内存: 2GB

### 推荐配置

- CPU：Intel Core i5-4690 3.5GHz、AMD A10-7800 APU 3.5GHz、Apple M1、龙芯3A6000 2.5GHz 或同等性能的CPU
- 芯片架构: ARM、x64、x86、LoongArch、RISC-V
- 内存: 4GB

> 更多请前往[来源](https://help.minecraft.net/hc/en-us/articles/4409225939853#h_01FFJMSQWJH31CH16H63GX4YKE)查看

## 启动器

**Deepin当前可用Minecraft Java启动器：**  
- [官方启动器](https://www.minecraft.net/zh-hant/download)
- [HMCL](https://hmcl.huangyuhui.net/download/)
- [XMCL](https://xmcl.app/zh/)
- [LauncherX](https://corona.studio/lx/download)
- [Modrinth启动器](https://modrinth.com/app)  
等等

**即将到来的Deepin可用Minecraft Java启动器：**  
- [BakaXL4](https://www.bakaxl.com/v4)
- [PCL.Neo](https://github.com/PCL-Community/PCL.Neo)
等等

> 以下步骤以HMCL为例
{.is-info}

### 启动器下载

以下为HMCL下载方式(此句在后文将略去)

- [HMCl官网](https://hmcl.huangyuhui.net/download/)  选择下载Linux的.jar或
- [GithubReleases](https://hmcl.huangyuhui.net/download/)  下载最新Release里的.exe、.jar或.sh文件

### 启动启动器

> 启动前请确保Deepin已安装jdk11或以上版本(HMCL最新版需要)
{.is-info}

- .jar文件启动:  
```shell
java -jar HMCL-xxx.jar
```
- .sh文件启动：  
```shell
./HMCL-xxx.sh
``` 
或直接双击打开
- .exe文件启动：  
```shell
java -jar HMCL-xxx.exe
``` 
~~linux上居然可以直接用exe来启动HMCL~~

#### 附录：创建桌面快捷图标
以.sh文件为例，桌面快捷图标应为
```desktop
[Desktop Entry]
Categories=Game;
Encoding=UTF-8
Exec=你的路径/HMCL-xxx.sh
Icon=你的路径/hmcl.png
MimeType=
Name=HMCL
Terminal=false
Type=Application
```
- 附附录： 桌面快捷图标
如果是.jar文件，可修改Exec为
```desktop
java -jar 你的路径/HMCL-xxx.jar
```
若想直接用N卡启动HMCL,可修改Exec为
```
env __GLX_VENDOR_LIBRARY_NAME=nvidia __NV_PRIME_RENDER_OFFLOAD=1 你的启动HMCL命令
```

> HMCL启动器图标可在[此处](https://github.com/HMCL-dev/HMCL/blob/main/HMCL/image/hmcl.png)下载

## 游戏前的准备

### 下载Java

- OpenJDK

通过以下命令下载openjdk8
```shell
sudo apt-get install openjdk-8-jre
```
前往[此处](https://openjdk.org/install/)下载openjdk9或以上版本

- Oracle JDK

前往[此处](https://www.oracle.com/java/technologies/downloads/)下载

#### 附录: 我的世界版本对应最低Java版本

| Minecraft Java版版本 | 最低Java版本       |
|----------------------|-------------------|
| 1.20.5（24w14a） | **Java 21**       |
| 1.18（1.18-pre2） | **Java 17**       |
| 1.17（21w19a） | **Java 16**        |
| 1.12（17w13a） | **Java 8**      |

> 信息来源于网络
{.is-info}

### 登陆账号，安装游戏等

> 此部分略去

#### 附录：HMCL启动器中文输入临时解决

下载安装jdk24，使用java24运行HMCL可临时解决HMCL启动器内无法输入中文的问题。
此问题来自于javafx17

## 启动游戏

点击启动器内**启动游戏**或类似意义的按钮即可

### 附录：独显启动我的世界方法
**Nvidia卡:**
> 请先确保Deepin已正确安装Nvidia驱动
{.is-warning}

> Nvidia卡用户请不要安装商店里内置Mesa3D的HMCL(包名：io.huangyuhui.hmcl)，如有需要请安装普通版（包名：io.huangyuhui.hmcl.normal）
{.is-info}

- 方法一： 

安装商店内的**任务栏显卡切换插件**(包名：dde-dock-graphics-plugin)后重启电脑，右键HMCL桌面图标选择**使用prime-run 运行**
- 方法二： 

创建prime-run脚本
```shell
nano ~/.local/bin/prime-run
```
输入
```shell
#!/bin/sh
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia "$@"
```
赋予权限
```shell
chmod +x ~/.local/bin/prime-run
```

- 方法三：

使用环境变量
在HMCL(或其他启动器)里的游戏设置/全局游戏设置等位置找到**环境变量**
输入
```
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia
```

- 方法四：

自行编写nvidia独显启动mc的脚本

**AMD卡**

- 方法一：

安装商店内打包好的HMCL启动器(包名io.huangyuhui.hmcl)

- 方法二：

使用环境变量
在HMCL(或其他启动器)里的游戏设置/全局游戏设置等位置找到**环境变量**
输入
```
DRI_PRIME=1
```

### 附录：提高Deepin游玩mc体验的MOD

- Fcitx5-Enhancer
作用：防止使用fcitx5输入法时fcitx5和我的世界同时响应键位动作
[Modrinth](https://modrinth.com/mod/fcitx5-enhancer/version/0.1.5) [Github](https://github.com/NLR-DevTeam/Fcitx5-Enhancer)

- IMBlocker
作用：一定程度帮助玩家切换输入法
[Modrinth](https://modrinth.com/mod/imblocker-original?version=1.21.8&loader=fabric)[Curseforge](https://www.curseforge.com/minecraft/mc-mods/imblocker)[Github](https://github.com/reserveword/IMBlocker)


## 游戏联机

**加入服务器**

略

**使用官方Realm**

略

**自行搭建服务器方案**

略

**内网穿透联机方案**

略

**EasyTier联机方案**

EasyTier是一个简单、安全、去中心化的异地组网方案。  
详情前往[此处](https://easytier.cn/)
- AstralGame 游戏联机工具

Astral 是一个基于EasyTier的现代化跨平台网络应用，旨在简化 P2P 网络连接和虚拟专用网络的创建与管理。  
详情前往[此处](https://astral.fan/quick-start/what-is-astral/)
- Terracotta

Terracotta即陶瓦联机，同样基于EasyTier，由HMCL开发成员开发，后续可能整合进HMCL中，当前可正常联机。
详情前往[Github](https://github.com/burningtnt/Terracotta)

## 游戏中

略

## 游戏崩溃

- 获取日志

在**游戏意外退出**或类似显示有日志的窗口内点击**导出游戏崩溃信息**或类似按钮

- 分析日志

可前往[CrashMC](https://www.crashmc.com/)学习如何分析我的世界崩溃日志

- 分享日志

将导出的日志分享给含有热心玩家的我的世界相关群组

# Deepin上游玩我的世界基岩版

> 编写中
{.is-info}

Deepin上可使用Mcpelauncher来游玩我的世界基岩版
> 相关链接：
mcpelauncher [文档](https://minecraft-linux.github.io/) [Github](https://github.com/minecraft-linux/mcpelauncher-manifest/)

## mcpelauncher介绍

mcpelauncher是一款针对Android版Minecraft基岩版的第三方启动工具。该启动器通过模拟JNI（Java本地接口）实现与Minecraft的通信，支持在Linux和MacOS系统上运行x86、x86_64、armhf及arm64架构的Minecraft版本。

> 此启动器当前未有中文翻译
{.is-info}

> 使用前请确保拥有已购买安卓Minecraft的Google账号，此方法不支持使用验证破解的第三方apk！
{.is-info}

## mcpelauncher安装

### 使用apt仓库进行安装

- 信任 apt repo 安装软件包

> 请前往 [Github](https://github.com/minecraft-linux/pkg) 查看相应步骤
{.is-info}

- 输入命令安装
```shell
sudo apt update
sudo apt install mcpelauncher-manifest mcpelauncher-ui-manifest msa-manifest
```

### 使用Flatpak安装

> 请确保已安装Flatpak,详情安装步骤见 [此处](https://flatpak.org/setup/Deepin)
{.is-info}

命令行输入此以全局安装

```shell
sudo flatpak install flathub io.mrarm.mcpelauncher
```

更新与卸载请在命令行输入

```shell
sudo flatpak update
sudo flatpak uninstall io.mrarm.mcpelauncher
```
### 使用Appimage运行
> Appimage方式已被弃用，无法游玩较新版本的我的世界基岩版
{.is-warning}

前往[此处](https://github.com/minecraft-linux/appimage-builder/releases/tag/v1.1.1-802)下载编译的Appimage

## 游戏前准备

### 登陆账号

此启动器需要登陆已购买安卓Minecraft的Google账号来下载我的世界基岩版
游戏内需要登陆你所游玩的微软账号

### 下载游戏与配置游戏

点击启动器下方编辑符号可修改游戏配置，如

- 配置名称 (Name)
- 游戏版本(默认最新正式版) (Version)
- 数据目录 (Data directory)
- 启动窗口大小 (Window size)
- 命令行 (Commandline)
- 环境变量 (Environment Variables)

#### 附录：使用独显启动我的世界基岩版

**Nvidia卡**
> 请先确保Deepin已正确安装Nvidia驱动
{.is-warning}

- 方法一(推荐)：

使用环境变量
将**环境变量**编辑为

| Environment Variables |         |
|----------------------|----------|
| __NV_PRIME_RENDER_OFFLOAD | 1     |
| __GLX_VENDOR_LIBRARY_NAME | nvidia |

- 方法二： 

安装商店内的**任务栏显卡切换插件**(包名：dde-dock-graphics-plugin)后重启电脑，选择**使用prime-run 运行**mcpelauncher

**AMD卡**

使用环境变量
将**环境变量**编辑为
| Environment Variables |         |
|----------------------|----------|
| DRI_PRIME | 1      |


## 启动游戏

点击启动器右下角的**PLAY**

## 游戏中

略

## 附录：如何使用光影

### 改包

- 前往mcpedl或klpbbs下载改包光影

- 打开mcpelauncher的目录，并进入 **versions/1.xx.xx.x/assets/assets/renderer/materials**

- 打开所下载的改包光影，将 **/renderer/materials/** 里的文件复制并替换到上一步所打开的目录中

- 将此光影导入至游戏

- 进入游戏，启用此光影

- 进入世界，享受此光影

### 使用MaterialBinLoader for mcpelauncher Mod

- 前往[Github](https://github.com/CrackedMatter/mcpelauncher-materialbinloader)里的releases下载最新的libmaterialbinloader.so
- 在mcpelauncher主目录中创建**mods**文件夹并将下载的libmaterialbinloader.so复制到该文件夹
- 在游戏里导入并启用支持的光影
## 一些问题

### 无法使用输入法

此问题当前无法解决，仅可复制粘贴以暂时输入中文等

### 无法使用光线追踪

此问题当前无法解决，安卓的Minecraft安装包并未包含使用光线追踪的文件

### 延迟渲染异常

维护者正在优化延迟渲染


