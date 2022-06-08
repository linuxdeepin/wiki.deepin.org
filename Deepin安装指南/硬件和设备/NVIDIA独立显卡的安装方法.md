---
title: NVIDIA独立显卡的安装方法
description: 
published: true
date: 2022-06-08T06:04:19.384Z
tags: 
editor: markdown
dateCreated: 2022-05-26T03:30:22.290Z
---

## 简介

感谢论坛用户sixchen提供的内容
时间：2021年7月27日
原帖地址：https://bbs.deepin.org/zh/post/223856


## 详情
### 第一步

停用会让 X服务 泉水挂机的服务

linux传统操作：

sudo dedit /etc/modprobe.d/blacklist.conf

在编辑器填入

blacklist vga16fb

blacklist nouveau

blacklist rivafb

blacklist nvidiafb

blacklist rivatv

保存推出

linux传统操作：

sudo update-initramfs -u

 

### 第二步
#### 官网下载驱动文件

[去官网下载linux系统的驱动包](https://www.nvidia.cn/Download/index.aspx)
> 通过点击上述链接，在官网查找到自己网卡对应的具体型号，就可以按照提示完成驱动下载了。

![官网驱动下载.png](/图片存储/官网驱动下载.png)
#### 增加文件执行权限
将下载对应的run文件保存到本地：
![n3.png](/图片存储/n3.png)

在驱动文件所在文件夹中通过右键打开终端

执行如下命令赋予该驱动文件相应的执行权限：

```sudo chmod +x ./*.run```

#### 卸载遗留驱动

```sudo apt-get remove --purge nvidia*```

 

### 第三步

暂时关闭X服务

这一步会黑屏请看完再操作

linux传统操作：

sudo service lightdm stop

进入tty

键盘按下 Ctrl  + Alt + F2

键入账户名 回车

输入密码 回车

su 回车

键入密码 回车

cd Downloads

sudo sh  ./*.run

然后一路yes确定

reboot 重启

 

### 第四步 配置显卡

```lspci | egrep 'VGA|3D' 获取设备BusID```

![n4.png](/图片存储/n4.png)
BusID 就是前面的数字

sudo dedit /etc/X11/xorg.conf  修改配置文件，注意修改自己的BusID个人喜欢方案B

 

方案A：启用双显卡，集显为默认显卡
```
Section "ServerLayout"

    Identifier "layout"

    Screen 0 "intel"

    Screen 1 "nvidia"

EndSection

 

Section "Device"

    Identifier "intel"

    Driver "intel"

    BusID "0:2:0"

    Option "AccelMethod" "SNA"

EndSection

 

Section "Screen"

    Identifier "intel"

    Device "intel"

EndSection

 

Section "Device"

    Identifier "nvidia"

    Driver "nvidia"

    BusID "4:0:0"

    Option "ConstrainCursor" "off"

EndSection

 

Section "Screen"

    Identifier "nvidia"

    Device "nvidia"

    Option "AllowEmptyInitialConfiguration" "on"

    Option "IgnoreDisplayDevices" "CRT"

EndSection
```
 

 

方案B：启用独显，屏蔽集显

 ```

Section "Module"

    Load "modesetting"

EndSection

 

Section "Device"

    Identifier "Card0"

    Driver "nvidia"

    BusID  "PCI:4:0:0"

EndSection
```
 

 

方案C：启用集显，屏蔽独显

 ```

Section "Module"

    Load "modesetting"

EndSection

 

Section "Device"

    Identifier "Card0"

    Driver "intel"

    BusID "PCI:0:2:0"

EndSection
```
保存

 

 

#修改

```sudo dedit /etc/lightdm/display_setup.sh```

 

#写入以下内容：下面三行
```
#!/bin/sh

xrandr --setprovideroutputsource modesetting NVIDIA-0

xrandr --auto

 

sudo chmod +x /etc/lightdm/display_setup.sh 赋予执行权限

 

sudo dedit /etc/lightdm/lightdm.conf  修改

找到#display-setup-script=这行，修改为

display-setup-script=/etc/lightdm/display_setup.sh
```
重新启动后，配置生效。

 
### 第五步 检测

装完之后发现笔记本屏幕窗口撕裂尝试通过以下方式解决：

```sudo dedit /etc/modprobe.d/nvidia-graphics-drivers.conf```

添加

 options nvidia_drm modeset=1

 保存退出

 

使其生效

``` sudo update-initramfs -u```

```reboot```
![n5.png](/图片存储/n5.png)
安装完成后可以通过如下命令查看显卡当前基本使用信息：
```nvidia-smi```

**最后，安装成功**