---
title: Fedora36安装深度桌面
description: Fedora36安装深度桌面
published: true
date: 2022-09-08T06:02:20.193Z
tags: 
editor: markdown
dateCreated: 2022-09-07T09:34:53.083Z
---

# 如何在 Fedora 36 Linux 上安装深度桌面（DDE）

深度桌面环境 （DDE）被认为  是深度 Linux 开发者创造的最优秀的美观桌面环境之一。它也经常被认为是 Linux 上最漂亮的桌面。对于 Fedora 的用户来说，这可以很容易地安装并且对于那些喜欢在桌面之间跳跃的人来说是一个可选的选择。

## 更新 Fedora

首先，确保您的系统是最新的，因为在使用以下命令安装辅助桌面环境时这是必不可少的。
在Fedora桌面环境中，打开终端输入：```sudo dnf upgrade --refresh -y```

## 安装深度桌面环境

深度桌面环境包含在 Fedora 36 的存储库中，使得安装相对简单。要开始安装，打开终端输入：```sudo dnf group install "Deepin Desktop" -y```
安装可能需要几分钟时间，具体取决于硬件的使用年限，尤其是您的互联网连接。总体而言，下载量大约在 450MB 左右，之后所需的磁盘空间大小约为 1.5GB。
安装完成后，您需要重新启动 PC。这可以在您的终端中使用 ```reboot```命令快速完成。

## 登录深度桌面环境

重新启动桌面后，您将进入登录屏幕。
首先，您需要通过单击屏幕右上角的设置图标来更改和验证桌面环境，然后选择 Deepin 。

## 更新深度桌面环境

为深度桌面环境和任何默认 DNF 包运行标准 DNF 更新命令以进行更新。
在终端中输入：```sudo dnf update```

当更新可用时，像使用任何其他 DNF 包一样运行标准升级命令。
在终端中输入：```sudo dnf upgrade --refresh -y```

## 删除（卸载）深度桌面环境

删除Deepin桌面环境的用户，请使用以下终端命令:
``` sudo dnf group remove "Deepin Desktop" ```
完成后，重新启动系统。

## 查看所有包环境信息
在终端中输入：```yum list installed```查看所有包环境信息

## 查看日志信息
可在路径 ~/.cache/deepin下查看日志信息


