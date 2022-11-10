---
title: deepin安装N卡驱动教程
description: 
published: true
date: 2022-11-10T22:30:41.237Z
tags: nvidia, 显卡
editor: markdown
dateCreated: 2022-06-20T09:22:28.761Z
---

## 一、前言

deepin 20.6仓库内的N卡驱动已经更新到了510版本

对于大部分用户而言，已经不需要用run文件来安装驱动

（如有需要可以看我之前的教程）

https://bbs.deepin.org/zh/post/232923?offset=0&postId=1317417

通过apt安装会更加简单

（不需要手动禁用nouveau、不需要更改配置文件、不需要显卡驱动管理器）

## 二、安装N卡驱动

跟着下面的步骤来就行

tips：下面的指令都可以用 <kbd>Ctrl</kbd> <kbd>C</kbd> 复制，然后在终端里面用 <kbd>Ctrl</kbd> <kbd>Alt</kbd> <kbd>V</kbd> 进行粘贴（可以在终端右上角菜单里自定义修改快捷键），回车执行

1. **卸载原有驱动**
	`sudo apt autoremove nvidia-*`

2. **添加32位架构并刷新源**
	```
	sudo dpkg --add-architecture i386
	sudo apt update
	```
	![1](https://storage.deepin.org/thread/202206101518227757_image.png)

3. **安装nvidia-detect**
	`sudo apt install nvidia-detect`

4. **执行nvidia-detect**
	`nvidia-detect`
	![2](https://storage.deepin.org/thread/202206101520101877_image.png)

5. **安装nvidia驱动**
	如果上图绿色部分是***nvidia-driver***
	那就终端输入
	`sudo apt install nvidia-driver nvidia-settings nvidia-smi`
	如果是***其它内容（比如nvidia-legacy-390xx-driver）***
	那就终端输入
	`sudo apt install nvidia-legacy-390xx-driver`
	安装驱动过程中有提示直接回车就行

安装完之后记得重启

## 三、检查驱动是否安装成功

1. 重启之后终端输入
	`nvidia-smi`
	![3](https://storage.deepin.org/thread/202206101526128800_image.png)
	如图所示的两处有内容就说明显卡驱动装好了

2. 如果是**比较老的显卡驱动（比如390系列）**，可能没有nvidia-smi
	可以打开自带浏览器输入
	`chrome://gpu`
	往下翻
	![4](https://storage.deepin.org/thread/202206101537493310_image.png)
	如果**Driver vendor那一栏后面是nvidia**就说明在使用N卡，也就说明驱动已经装好

## 四、intel和nvidia双显卡切换

可以去星火应用商店下载显卡插件进行切换

![5](https://storage.deepin.org/thread/202206101553344494_image.png)

安装完之后需要注销电脑重新登录

然后在状态栏右下角可以找到切换图标

（有些电脑在切换时会卡住或者没反应，直接重启就好）

![6](https://storage.deepin.org/thread/202206101554559505_image.png)

## 五、打开同步功能

![7](https://storage.deepin.org/thread/202206101611035221_image.png)

个人亲测，使用N卡如果没有打开同步功能，用浏览器播放在线视频可能会出现一些割裂线条

（用intel核显没遇到过这个问题）

打开方式有两种

### 第一种方法

（只适用于intel和nvidia双显卡）

安装好显卡切换插件，将显卡切换成intel

然后打开终端输入

`sudo rm /etc/X11/xorg.conf`

然后再切换成nvidia就可以了

### 第二种

（来自论坛内的安洛大佬，我没测试过）

编辑/etc/default/grub

```
sudo deepin-editor /etc/default/grub
```

看看有没有"`GRUB_CMDLINE_LINUX=`"这一行。如果没有就在末尾加上一行：

```
GRUB_CMDLINE_LINUX="nvidia-drm.modeset=1"
```

否则直接在 `GRUB_CMDLINE_LINUX=`后面加内容。

```
sudo update-grub
```

重启电脑。问题解决。

如果还没有解决，可以添加两步：

1. 找到nvidia设置里面的显示器界面，复制NAME:后面的那串字符，在上图里面是eDP-1-1

2. `sudo xrandr --output 刚刚复制的字符 --set "PRIME Synchronization" 1`

## 六、其它问题

### 一、nvidia-detect可能会给部分老显卡推荐nvidia-driver，但是实际安装不可用

解决办法

1、卸载以前的驱动

`sudo apt autoremove nvidia-*`

2、安装390驱动再重启

`sudo apt install nvidia-legacy-390xx-driver`

### 二、有个别用户装了显卡切换插件会出现开机黑屏无法进入桌面的情况

可以尝试如下方法

1. 按住 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> ( + <kbd>Fn</kbd> ) + <kbd>F2</kbd> 进入超级终端输入帐号密码登录

2. 卸载显卡驱动插件
	`sudo apt purge dde-dock-graphics-plugin`

3. 删除配置文件
	`sudo rm /etc/X11/xorg.conf`

然后重启看可不可以进入系统

如果仍旧黑屏的话

1. 重新进入超级终端

2. 卸载显卡驱动
	`sudo apt autoremove nvidia-*`

3. 再删除配置文件
	`sudo rm /etc/X11/xorg.conf`

再重启应该就可以进入桌面了

然后根据上面的步骤重新装一下驱动就行