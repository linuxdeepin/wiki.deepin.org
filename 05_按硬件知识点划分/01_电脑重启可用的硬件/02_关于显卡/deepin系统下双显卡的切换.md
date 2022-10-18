---
title: deepin系统下双显卡的切换
description: 本文只针对双显卡机器（I+N卡或A+N卡），deepin内核优选5.18.17-amd64-desktop-hwe版本
published: true
date: 2022-10-18T10:24:53.847Z
tags: 切换, 双显卡, 硬件加速
editor: markdown
dateCreated: 2022-10-18T10:12:04.991Z
---

# deepin系统下双显卡的切换

本文只针对双显卡机器（I+N卡或A+N卡），deepin内核优选5.18.17-amd64-desktop-hwe版本。

## 一、安装N卡驱动

1. 安装N卡驱动的方法
   a. 安装系统时，硬盘分区时勾选“集成NVIDIA闭源驱动”项
   ![安装界面勾选项.jpg](/for_trans/切换显卡/安装界面勾选项.jpg)

    b. 若以上方法无法安装，则终端中输入命令`sudo apt install nvidia-driver`，下载NVIDIA闭源驱动。

   详细的安装步骤可参考wiki：[Deepin安装最新NVIDIA驱动](https://wiki.deepin.org/zh/05_%E6%8C%89%E7%A1%AC%E4%BB%B6%E7%9F%A5%E8%AF%86%E7%82%B9%E5%88%92%E5%88%86/01_%E7%94%B5%E8%84%91%E9%87%8D%E5%90%AF%E5%8F%AF%E7%94%A8%E7%9A%84%E7%A1%AC%E4%BB%B6/02_%E5%85%B3%E4%BA%8E%E6%98%BE%E5%8D%A1/Deepin%E5%AE%89%E8%A3%85%E6%9C%80%E6%96%B0NVIDIA%E9%A9%B1%E5%8A%A8 )

2. 判定NVIDIA闭源驱动是否安装成功的方法：终端中输入命令：`dpkg -l | grep nvidia`，检查终端中显示。

   - 若闭源驱动安装成功，则显示如下：
    ![显卡驱动相关信息显示.png](/for_trans/切换显卡/显卡驱动相关信息显示.png)

   - 若NVIDIA闭源驱动未安装成功，则无以上截图中信息显示。有可能安装的N卡驱动为开源的nouveau驱动，此时可参考wiki[显卡](https://wiki.deepin.org/zh/05_%E6%8C%89%E7%A1%AC%E4%BB%B6%E7%9F%A5%E8%AF%86%E7%82%B9%E5%88%92%E5%88%86/01_%E7%94%B5%E8%84%91%E9%87%8D%E5%90%AF%E5%8F%AF%E7%94%A8%E7%9A%84%E7%A1%AC%E4%BB%B6/02_%E5%85%B3%E4%BA%8E%E6%98%BE%E5%8D%A1/%E6%98%BE%E5%8D%A1)中方法将其禁用掉。

## 二、关于官方推出的切换至NVIDIA闭源的方法

   1. 终端中使用命令`sudo apt install deepin-prime`，下载deepin-prime包。

   2. 设置显卡混合使用模式，使用命令`sudo prime-select offload`，offload机制是开启渲染卸载功能，根据环境变量选择是否启用独显。

      注：显卡的切换涉及到xorg的配置，所以使用显卡切换相关命令时需要注销才能生效。

   3. 查看当前配置的显卡模式，终端中使用命令：`sudo prime-select get-current`，终端中显示如下：
      ![查看当前配置.png](/for_trans/切换显卡/查看当前配置.png)

   4. 开启offload后通过修改环境变量，达到N卡渲染的效果（共两种方法）：

  - 终端中使用命令`sudo apt install mesa-utils`安装mesa-utils这个包，用来显示系统的glx相关信息。
    终端中输入命令配置环境变量：` __NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia glxinfo | grep vendor`
    显示如下：
        ![环境变量.png](/for_trans/切换显卡/环境变量.png)
        例：若使用影院播放视频时需要使用N卡渲染，则需使用命令:
     ```
     __NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia deepin-movie
     ```
  - 或对相关应用的.desktop文件中的环境变量进行修改
    例：使用影院播放视频时使用N卡渲染，修改文件`sudo vim /usr/share/applications/deepin-movie.desktop`
    第4行中修改为
    ```  
    Exec=sh -c "__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia deepin-movie" %U
    ```
    如下图：
    ![环境变量修改.png](/for_trans/切换显卡/环境变量修改.png)
    保存后则每次打开影院播放视频时，均为N卡渲染。

## 三、播放视频时，N卡渲染验证方法

1. 使用工具nvidia-smi：

   a. 终端中下载`sudo apt install nvidia-smi`

   b. 播放高清视频时，查看显存在用情况，终端中调用`nvidia-smi`工具，截图显示如下：

   ![nvidia-smi.png](/for_trans/切换显卡/nvidia-smi.png)

   若N卡无法渲染，则占用显存数量值为0或4MiB。

2. 使用工具nvidia-settings

   a. 终端中下载：`sudo apt install nvidia-settings`

   b. 播放高清视频时，查看显存在用情况，终端中调用：`nvidia-settings`，截图显示如下：

   ![nvidia-settings.jpg](/for_trans/切换显卡/nvidia-settings.jpg)

## 四、硬件解码功能

如何判断N卡是否能支持硬件解码的判断标准暂时不了解，有了解的朋友请帮忙优化一下。目前使用offload模式通过改变环境变量的方式，来达到使用N卡渲染的方案，暂不能调用硬件解码功能。

1. 通过以上方案使用N卡渲染，打开影院应用，设置-基础设置-解码方式中选择“硬件解码”。

2. 终端中下载`sudo apt install htop`，播放高清视频时，终端中输入命令`htop`查看CPU占用率，如下图：

   ![htop.jpg](/for_trans/切换显卡/htop.jpg)

   若视频播放不流畅或出现卡顿，CPU占用率很高，说明硬件解码功能未被调用。

## 五、还原设置（切回I卡或N卡）

终端中使用命令`sudo prime-select unset`

1. 切回至I卡：`sudo prime-select intel`
2. 切回至A卡：`sudo prime-select amd`