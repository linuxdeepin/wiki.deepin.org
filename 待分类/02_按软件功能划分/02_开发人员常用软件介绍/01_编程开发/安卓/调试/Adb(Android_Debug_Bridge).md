---
title: Adb(Android_Debug_Bridge)
description: 
published: true
date: 2022-10-21T04:40:02.478Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:28:52.269Z
---

ADB 是功能强大的命令行工具。它允许您通过数据线或者是网络与 android 设备通信。您可以通过使用 adb 命令向 android 设备安装应用程序、复制文件、运行 shell 命令或者是调试应用。

## adb 命令的安装

在 Deepin linux 系统中，您可以以多种方式安装 adb 命令。

### 通过系统内置软件源进行安装（推荐）

执行以下命令，安装 adb 工具。

``` shell
sudo apt install android-tools-adb
```

### 通过 google 提供的下载链接进行安装

访问 [SDK 平台工具版本说明](https://developer.android.com/studio/releases/platform-tools) 页面，找到 [下载适用于 Linux 的 SDK Platform-Tools](https://dl.google.com/android/repository/platform-tools-latest-linux.zip)，点击链接，在同意条款后下载到本地。然后执行以下命令，将下载到的安装包解压到指定目录。

需要注意的是，此安装方式除了会安装 adb 命令外，还会安装 `fastboot` 等多个 android 设备常用命令。

``` shell
sudo mkdir -p /opt/android/sdk
sudo unzip platform-tools_r33.0.3-linux.zip -d /opt/android/sdk/
```

以上命令将会在 `/opt` 目录下创建名为 `android/sdk` 的目录，之后将下载得到的 zip 文件，解压到此目录下。

如果您使用的 shell 是 bash，使用以下命令将 adb 命令的所在路径添加到环境变量。

```
echo "export PATH=\"/opt/android/sdk/platform-tools:\$PATH\"" >> ~/.bashrc
```

如果您使用的是其他的 shell 程序，请参考对应的文档。

### 通过 android studio 的 sdk manager 进行安装

此安装方式略。

## 故障排除

执行 `adb devices` 提示没有权限。

```
$ adb devices
List of devices attached
*****************     no permissions (user $USER is not in the plugdev group); see [http://developer.android.com/tools/device.html]
```

如果是通过软件源安装的 adb 工具，可以通过重新拔插 android 设备和重启 Deepin 系统解决。

如果是通过 google 提供的下载链接进行安装，则需要参考 [android-udev-rules](https://github.com/M0Rf30/android-udev-rules 解决。

## 相关链接
[玩转ADB命令（ADB命令使用大全）](https://blog.csdn.net/zhonglunshun/article/details/78362439)
[arch wiki上的adb](https://wiki.archlinux.org/index.php/Android_Debug_Bridge)