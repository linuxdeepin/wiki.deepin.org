---
title: 适配wsl的deepin根文件系统的配置和使用
description: 本文简述了适配windows中wsl的deepin根文件系统的环境搭建及基本使用
published: true
date: 2023-07-03T09:04:26.543Z
tags: deepin, wsl, 根文件系统
editor: markdown
dateCreated: 2023-06-30T08:58:46.233Z
---

# 适配wsl的deepin根文件系统的环境配置和使用

## 环境搭建

1. 应用商店（Microsoft store）中下载安装Windows Subsystem for Linux；

2. 将[ 制作一个deepin的根文件系统 ](https://github.com/deepin-community/deepin-rootfs)目录下的deepin.zip下载至本地，在解压后的文件夹下打开终端；

3. 使用命令`./deepin.exe install .\deepin-rootfs.tar`，生成ext4硬盘映像文件：
![0_0.jpg](/for_trans/wsl/0_0.jpg)

4. 使用命令`wsl -l`，验证已成功安装deepin根文件系统：
![1_1.png](/for_trans/wsl/1_1.png)

5. 使用命令`wsl -d deepin`或`./deepin.exe`启用wsl，当前为根文件系统的root用户权限
   
   ```
   使用命令exit,显示logout退出deepin
   使用命令`wsl -s deepin`将deepin根文件系统设置为默认发行版，这样直接使用`wsl`命令即可直接启用deepin根文件系统
   ```
   ![1_2.png](/for_trans/wsl/1_2.png)

6. 添加用户(非必要项)
   
   - 创建一个deepin帐户，设置默认shell为bash：`useradd -m deepin -s /usr/bin/bash`；
   
   - 设置密码：`passwd deepin`，密码不会在终端中显示，需重复输入；
   
   - 添加deepin用户到sudo用户组：`usermod -aG sudo deepin`。
    ![1_3.png](/for_trans/wsl/1_3.png)
   
   - 将deepin设置为wsl的默认用户：`./deepin.exe config --default-user deepin`，验证方式：windows命令行提示符中输入wsl，启用deepin根文件系统进入deepin用户目录下。

7. 开启systemd支持（建议开启）
   
   ```
   cat >> /etc/wsl.conf << EOF
   [boot]
   systemd=true
   EOF
   ```
   
   deepin根文件系统中输入以上文本，保存后退出至根文件系统。
   
   - 输入exit，显示logout退出deepin
   
   - 需<mark>重启</mark>wsl才能生效，使用命令停止运行wsl：`wsl -t deepin`
   
   - 启用wsl:`wsl -d deepin`

8. 配置语言环境（非必要项）
   默认语言环境是英文，使用命令`sudo dpkg-reconfigure locales`进行修改。需回车三次，输入312，对应的选项zh_CN.UTF-8。再次输入3，对应选项zh_CN.UTF-8。参考步骤7走重启wsl后生效。

## wsl中deepin根文件系统的使用

1. 使用自研应用前需安装以下包，重启wsl后尝试使用deepin应用。
   ```sudo apt install fonts-noto-cjk dde-qt5integration dde-qt5wayland-plugin```
   
   - fonts-noto-cjk：字体库，如果不安装可能导致软件字体不正常。
   
   - dde-qt5integration：deepin应用程序和deepin桌面环境的Qt5主题集成插件。它在Qt的基础上实现了许多额外的功能，比如窗口装饰、阴影绘制、高分辨率下的光标支持、当前工作区的窗口列表获取等。
   
   - dde-qt5wayland-plugin：Qt5 模块，它提供了一些插件和库，用于在 Wayland 上运行或创建 Qt 应用程序。

2. 自研应用的安装和使用
   
   - 深度终端
     安装deepin-terminal：`sudo apt install deepin-terminal`
     需重启wsl后，启用deepin-terminal：`deepin-terminal`
     ![1_4.png](/for_trans/wsl/1_4.png)
   
   - 看图
     安装看图应用：`sudo apt install deepin-image-viewer`
     启用看图，输入命令`deepin-image-viewer`
   
   - 浏览器
     安装浏览器：`sudo apt install org.deepin.browser`
     启用浏览器：`browser`
   
   - 文件管理器
     安装文件管理器：`sudo apt install dde-file-manager`
     启用应用：`dde-file-manager`
   
   - 深度音乐 （启用音乐出现闪退的情况）
     安装音乐：`sudo apt install deepin-music`
     启用音乐：`deepin-music`
   
   - 深度影院
     安装影院：`sudo apt install deepin-movie`
     启用影院：`deepin-movie` 
   
   - 深度相册
     安装相册：`sudo apt install deepin-album`
     启用应用：`deepin-album`
   
   - 深度画板 （启用后无法使用）
     安装画板：`sudo apt install deepin-draw`
     启用画板：`deepin-draw`
   
   *自研应用目前存在UI界面显示异常、部分基本功能无法使用的情况，会持续优化。*

3. 配置编码环境-java
   
   - 安装java包，使用命令`sudo apt install openjdk-8-jdk`
   
   - 查看jdk包是否正确安装：`java -version`
   
   - 下载vim编辑器：`sudo apt install vim`
   
   - 使用vim编辑器：`sudo vim ~/.profile`
   
   - 在profile配置文件页尾添加：
   
   ```
   # set JAVA_HOME
   export JAVA_HOME="usr/bin/java"
   ```
   
   - 使配置生效：`source ~/.profile`
   
   - 查看配置JAVA_HOME是否正确，使用命令：`echo $JAVA_HOME`
![1_5.png](/for_trans/wsl/1_5.png)

4. 使用玲珑应用命令（<mark>不建议</mark>）
   
   - 首先安装玲珑包：`sudo apt install linglong-bin`
   
   - 安装玲珑应用相关依赖：`sudo apt install org.deepin.browser`（该部分存在异常，后续优化）
   
   - 查看玲珑仓库地址：cat /persistent/linglong/config.json，正确的玲珑仓库地址为 https://mirror-repo-linglong.deepin.com
   
   - 否则需手动修改，输入以下命令：ll-cli repo modify https://mirror-repo-linglong.deepin.com --name repo
   
   - 接下来就可以使用玲珑命令下载、卸载、启用部分玲珑应用：
![1_6.png](/for_trans/wsl/1_6.png)
     *若是出现 start ll-service failed,可尝试手动启用，输入命令`ll-service`即可。*


