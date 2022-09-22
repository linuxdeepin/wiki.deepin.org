---
title: 简单几步为Deepin添加蓝牙音频编码支持
description: 
published: true
date: 2022-06-28T03:06:52.122Z
tags: 
editor: markdown
dateCreated: 2022-06-28T03:06:50.168Z
---

# 简单几步为Deepin添加蓝牙音频编码支持
UOS的蓝牙音频（A2DP）默认只支持SBC音频编码，音质很一般。现在很多蓝牙耳机都支持LDAC、aptX HD、aptX、AAC等高音质音频编码，虽然UOS默认不支持，但我们可以自行添加。

方案来自开源项目 [https://github.com/EHfive/pulseaudio-modules-bt/wiki/Packages#ubuntu-1804-1810-1904](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9naXRodWIuY29tL0VIZml2ZS9wdWxzZWF1ZGlvLW1vZHVsZXMtYnQvd2lraS9QYWNrYWdlcyN1YnVudHUtMTgwNC0xODEwLTE5MDQ.)
原文是适用于Ubuntu的安装方法，我在此改成适用于UOS 20的安装方法。此方法应该也适用于Deepin v20，不过我没有测试过。

安装步骤：

1. 仅UOS需要，Deepin不需要：打开开发者模式（“控制中心 > 通用 > 开发者模式”）。
2. 打开终端，输入如下命令并回车（添加蓝牙音频编码器软件源）：

```bash
echo 'deb http://ppa.launchpad.net/eh5/pulseaudio-a2dp/ubuntu bionic main' | sudo tee /etc/apt/sources.list.d/pulseaudio-a2dp.list
```

此时会提示你输入密码，输入你的UOS开机密码即可。输入时不会显示任何内容，这是正常现象，输完回车即可。

1. 继续在终端输入如下命令，一行一行粘贴并回车（不包括 # 井号开头的行）：

```bash
# 信任刚添加的软件源（获取证书）
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv A45582EC25E9D8E6
# 更新软件列表
sudo apt update
# 升级现有的蓝牙音频组件
sudo apt upgrade -y
# 安装支持LDAC、aptXX HD、aptX、AAC蓝牙音频解码器的音频组件
sudo apt install -y pulseaudio libavcodec58 libldac pulseaudio-modules-bt pavucontrol
```

**ppa.launchpad.net** 的服务器位于国外，如果命令下载文件的速度很慢，或者命令报错（比如“部分索引文件下载失败”），你可能需要自行采取措施。