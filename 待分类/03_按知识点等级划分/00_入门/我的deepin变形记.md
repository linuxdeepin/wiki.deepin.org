---
title: 我的deepin变形记
description: 要阅读最新版本请访问原帖。
published: true
date: 2023-04-15T03:38:45.469Z
tags: 
editor: markdown
dateCreated: 2023-01-10T13:57:40.533Z
---

> 本文原作者：[木一明](https://bbs.deepin.org/user/160805)，原文链接：[https://bbs.deepin.org/post/228568](https://bbs.deepin.org/post/228568)

> 当前使用的正式版本：deepin 20.8
> 
> 文章最新更新时间：2023/04/05

《我的deepin一键变形》脚本，代码托管：

shell版本，不再维护：https://gitee.com/liwl1991/my-deepin-sh

python版本：https://gitee.com/liwl1991/my-deepin-py

效果视频：

https://www.bilibili.com/video/BV1SU4y127ih/

# 1\. 前言

接触和使用deepin也有多年的时间了。虚拟机，物理机，双系统都折腾过。重装系统N次，在N次重装过程中，发现自己折腾和配置deepin的路线大体一致，呈现出极强的个人主义色彩。虽然这个色彩跟我本人的使用习惯息息相关，但是其中一些操作，遇到的问题，也可能是其他人未曾或者也曾遇到过的。那就果断分享出来，给那些还在折腾的道路上不知所措的小伙伴们一点儿光芒，照亮他们入坑的道路。

注意：

本篇不会笼统去介绍系统的安装和使用，只是介绍我在安装完系统以后，如何把我极强的个人主义加压到这个系统里面。把它折腾成我喜欢的模样。

因此此贴，主要是介绍【我与deepin的相爱相杀】的过程。

本篇主要分2部分：

-   小篇幅介绍系统下载安装
-   大篇幅介绍安装完系统以后的事情

# 2\. 系统安装

## 2.1 系统镜像下载

deepin系统ISO下载，使用官方网站给出的各种下载途径即可：[点我直达]([https://www.deepin.org/zh/download/](view-source:https://www.deepin.org/zh/download/))

截止到2021年11月30号，deepin当前可下载的最新版本是20.3版本

其他历史版本下载地址：

-   [V20的历史版本]([https://cdimage.deepin.com/releases/](view-source:https://cdimage.deepin.com/releases/))
-   [V20之前的历史版本]([https://cdimage.deepin.com/releases-archive/](view-source:https://cdimage.deepin.com/releases-archive/))

## 2.2 安装方式

**刻录启动盘【适合专业人士】**

如果有操作系统安装经验的朋友，可以选择刻录光盘，或者制作u盘启动盘的方式。

官方提供了windows操作系统和Linux操作系统，两个平台下的刻录工具：[深度启动盘制作工具]([https://www.deepin.org/zh/original/deepin-boot-maker](view-source:https://www.deepin.org/zh/original/deepin-boot-maker))

然后在自己的PC上，设置BIOS，从U盘启动即可。

**使用第三方工具【适合小白人士】**

比较推荐的是vetory这个工具，[官方地址]([https://www.ventoy.net/cn/index.html](view-source:https://www.ventoy.net/cn/index.html))

只需要下载这个软件，把这个软件安装在U盘，然后把SO镜像拖入U盘，最后设置PC从BIOS启动即可选择deepin的镜像进行安装。

**虚拟机体验【适合体验人士】**

虚拟机安装deepin，对于想要尝试使用deepin的人比较友好。不喜欢就直接销毁虚拟机。或者一些想要在物理机上操作的功能，在虚拟机上可以实验几次，然后考虑在物理机操作。

如果是windows平台的用户，建议使用vmware workstation安装deepin虚拟机。能够使用其提供的沉浸式功能，仿佛在物理机使用deepin

**双系统安装【适合更加专业的人士】**

双系统安装，更加考验一个人的动手能力。除非有过双系统安装经验，否则不建议使用该方式。

双系统的常见安装：双硬盘（包括加装的），不同分区

> 本人只使用过双硬盘，双分区没有尝试过，也不建议这么做，覆巢之下无完卵

**其他方式**

比如live版本，无盘安装，批量安装等，均可尝试，但是笔者并未尝试，这里不做详细介绍。

## 2.3 安装注意事项

-   **内核版本**  
    建议安装，一般是stable内核版本，不是LTS内核版本，适用于喜欢尝鲜或者硬件比较新的用户
-   **分区选择**  
    在人性化，图形化的安装过程，最让非专业人士困惑的就是磁盘分区的操作。  
    本人一般使用手动安装，分区有三个：efi分区，大小默认。boot分区，大小默认。剩下就是/分区，剩下全盘即可。
    
    > 个人不喜欢使用交换分区，对于pc机而言没啥用
    
-   **集成nvidia驱动**。看个人硬件配置和喜好选择

由于我的两台物理机都已经安装和配置好系统，这里以虚拟机来展示一下是我个人的分区步骤：

> 需要注意：如果比较新的物理机，可能是uefi引导方式，需要分配efi分区。进行efi的分区操作很简单，就是在给boot分区配置之前，选择文件系统为efi，大小也是默认即可。不进行efi分区，会提示你分区。这个时候如果已经分好区，点击最下面的“修改引导器”后面的删除操作。

![image.png]([https://storage.deepin.org/thread/202111291118024972_image.png](view-source:https://storage.deepin.org/thread/202111291118024972_image.png))

![image.png]([https://storage.deepin.org/thread/202111291118435170_image.png](view-source:https://storage.deepin.org/thread/202111291118435170_image.png))

![image.png]([https://storage.deepin.org/thread/202111291119379698_image.png](view-source:https://storage.deepin.org/thread/202111291119379698_image.png))

![image.png]([https://storage.deepin.org/thread/202111291120031893_image.png](view-source:https://storage.deepin.org/thread/202111291120031893_image.png))

![image.png]([https://storage.deepin.org/thread/202111291120171609_image.png](view-source:https://storage.deepin.org/thread/202111291120171609_image.png))

这里提示：未挂载交换分区，可能会影响系统性能。

人家也说了，只是友情提示 ，那就直接忽略得了，没啥问题

全部确定以后，等不到一杯茶的时间，系统就安装好了

![image.png]([https://storage.deepin.org/thread/20211129112442142_image.png](view-source:https://storage.deepin.org/thread/20211129112442142_image.png))

重启系统之后，有个性化配置部分，直接下一步下一步下一步。主要设置以下部分：图像，登陆帐号，登陆密码，计算机名字

![image.png]([https://storage.deepin.org/thread/202111291125511805_image.png](view-source:https://storage.deepin.org/thread/202111291125511805_image.png))

# 3\. 系统简单美化

系统简单美化包括deepin桌面环境四大件：

1.  任务栏
2.  启动器
3.  控制中心
4.  文件管理

系统安装完成，登陆以后，初始的样貌是这样：

![image.png]([https://storage.deepin.org/thread/202111291132074775_image.png](view-source:https://storage.deepin.org/thread/202111291132074775_image.png))

下面就开始动手术，一步一步朝着自己想要的方面前进了。

## 3.1 模式选择

系统提供了“时尚模式”和“高效模式”的选择。通过系统介绍视频，或者右键点击任务栏，均可选择调整。

最下面放置各类图标的，叫做任务栏。

鼠标右键任务栏，调整模式为“高效模式”。

![image.png]([https://storage.deepin.org/thread/202111291136309254_image.png](view-source:https://storage.deepin.org/thread/202111291136309254_image.png))

## 3.2 任务栏插件

任务栏右键时，插件提供了一些能够显示在任务栏上的插件模块。选择自己喜欢的，不显示自己不喜欢的插件。

插件也可以进行拖动，隐藏起来或者显示出来。

以下是我个人任务栏右侧的设置：

调整前：

![image.png]([https://storage.deepin.org/thread/20211129113934395_image.png](view-source:https://storage.deepin.org/thread/20211129113934395_image.png))

调整后：

![image.png]([https://storage.deepin.org/thread/202111291140227916_image.png](view-source:https://storage.deepin.org/thread/202111291140227916_image.png))

对比一下右下角，感觉清爽了很多

然后拖动>左边的图标，到右边来。这些常用的pc状态，不应该应藏起来，如下图：

![image.png]([https://storage.deepin.org/thread/202111291141429043_image.png](view-source:https://storage.deepin.org/thread/202111291141429043_image.png))

以下是我个人任务栏左侧的设置：

![image.png]([https://storage.deepin.org/thread/202111291144221142_image.png](view-source:https://storage.deepin.org/thread/202111291144221142_image.png))

调整后：

![image.png]([https://storage.deepin.org/thread/202111291144468610_image.png](view-source:https://storage.deepin.org/thread/202111291144468610_image.png))

看起来很清爽，不着急，后面会放些经常使用的软件上去。

## 3.3 启动器

高效模式的启动器长这样：

![image.png]([https://storage.deepin.org/thread/202111291145552355_image.png](view-source:https://storage.deepin.org/thread/202111291145552355_image.png))

## 3.4 控制中心

![image.png]([https://storage.deepin.org/thread/202111291146448543_image.png](view-source:https://storage.deepin.org/thread/202111291146448543_image.png))

与windows庞大臃肿风格对比，deepin则是简单实用小清新。在控制中心先进性一波初始配置。

我首先配置个性化图像：控制中心-账户-点击图像-点击+，进行个性化图像设置。

![image.png]([https://storage.deepin.org/thread/202111291159264220_image.png](view-source:https://storage.deepin.org/thread/202111291159264220_image.png))

> 因为我使用的是虚拟机 ，需要往虚拟机里面拷贝文件，所以需要通过ssh拷贝。

默认安装的系统，未启动sshd服务，需要手动启动，才能通过ssh登陆。操作如下：

![image.png]([https://storage.deepin.org/thread/202111291157463929_image.png](view-source:https://storage.deepin.org/thread/202111291157463929_image.png))

设置个性化图像后，效果如下图。能看出来，控制中心和启动器的图像都已经变化。

![image.png]([https://storage.deepin.org/thread/202111291201424437_image.png](view-source:https://storage.deepin.org/thread/202111291201424437_image.png))

圆角太大，设置小一点的：

![image.png]([https://storage.deepin.org/thread/202111291202484882_image.png](view-source:https://storage.deepin.org/thread/202111291202484882_image.png))

使用电源时，希望PC一直运行，那就不设置节能策略。如下图，全部选择“从不”

> 如果是笔记本PC，还会有“使用电池”的选项，做同样操作即可。

![image.png]([https://storage.deepin.org/thread/202111291203432353_image.png](view-source:https://storage.deepin.org/thread/202111291203432353_image.png))

“鼠标”设置这部分，如果是笔记本，在“触摸板”设置选项，启用“自然滚动”

主机名修改，点击“系统信息”，如下图：

![image.png]([https://storage.deepin.org/thread/202111291205495140_image.png](view-source:https://storage.deepin.org/thread/202111291205495140_image.png))

修改主机名，通过【控制中心】修改，只是第一步，还需要编辑/etc/hosts文件，不然后续会出现一些无伤大雅的小问题。比如下图，通过控制中心修改“计算机名”为deepin01以后，终端执行 `sudo apt update`时，发现会报deepin01无法解析。这个就是因为/etc/hosts里面的主机名还是之前的。

> 希望官网以后可以修复这个问题

![image.png]([https://storage.deepin.org/thread/202111300910373956_image.png](view-source:https://storage.deepin.org/thread/202111300910373956_image.png))

通过下图能够确认上面的问题。所以还需要 `sudo vim /etc/hosts`修改为 `127.0.0.1 deepin01`，就能解决这个问题

![image.png]([https://storage.deepin.org/thread/202111300912258401_image.png](view-source:https://storage.deepin.org/thread/202111300912258401_image.png))

## 3.5 文件管理器

![image.png]([https://storage.deepin.org/thread/202111291147061203_image.png](view-source:https://storage.deepin.org/thread/202111291147061203_image.png))文件管理器这里，简单配置一下：取消“显示最近使用文件”和“隐藏系统盘“

![image.png]([https://storage.deepin.org/thread/202111291154587017_image.png](view-source:https://storage.deepin.org/thread/202111291154587017_image.png))

![image.png]([https://storage.deepin.org/thread/202111291155523204_image.png](view-source:https://storage.deepin.org/thread/202111291155523204_image.png))

这里介绍一下我本人在文件管理器的常用的使用技巧：

-   在文件管理界面，使用ctrl+shift+?能够查看文件管理全部快捷键
-   选中的文件，通过ctrl+c直接复制文件全部路径
-   ctrl+l，查看当前目录路径，然后ctrl+c可以选中路径
-   右键可以使用终端打开
-   可以安装sshfs，nfs，samba，webdev等来远程挂载文件系统
-   ctrl+1，ctrl+2分别切换列表和图标模式
-   ctrl+h，切换隐藏或者显示隐藏文件的操作

如果是双系统，每次开机之后，在文件管理器一直能够看到win10系统盘。那么该如何取消win10系统盘的挂载？方式有两种

1.通过“磁盘”软件，来设置，主要设置“挂载选项”，让系统在开机后不挂载

2.直接编辑/etc/fstab，添加以下挂载选项（这种方式比较适合专业选手）

`/dev/disk/by-uuid/68B81F88B81F53C2 /mnt/68B81F88B81F53C2 auto nosuid,nodev,nofail,noauto,x-udisks-auth 0 0`

注意：终端执行 lsblk查看硬盘（机械盘，固态盘）的uuid，上面 `68B81F88B81F53C2`便是我安装了win10的系统盘的uuid，主要关注的是后面的挂载选项

## 3.6 桌面壁纸和锁屏壁纸

**桌面壁纸**

![image.png]([https://storage.deepin.org/thread/202111291208199003_image.png](view-source:https://storage.deepin.org/thread/202111291208199003_image.png))

桌面壁纸跟锁屏壁纸不是一张图，这个可以从：桌面-右键-设置壁纸，来证实

![image.png]([https://storage.deepin.org/thread/202111281630098821_image.png](view-source:https://storage.deepin.org/thread/202111281630098821_image.png))

锁屏壁纸的操作，比桌面壁纸复杂些。这里不做过多的解释，一个脚本搞定一切

```shell
#!/bin/bash
#更换默认背景图片
DEFUALT_IMG_JPG=$1
if [ "$#" -ne 1 ]
then
    echo "选择一张图片"
    exit 0
fi
echo "拷贝图片到主题目录"
sudo cp ${DEFUALT_IMG_JPG} /usr/share/wallpapers/deepin/desktop.jpg
echo "设置图片为默认背景图片"
sudo cp ${DEFUALT_IMG_JPG} /usr/share/backgrounds/default_background.jpg
echo "设置系统默认图片"
sudo ln -fs /usr/share/wallpapers/deepin/desktop.jpg /etc/alternatives/deepin-default-background
echo "设置锁屏图片"
sudo cp ${DEFUALT_IMG_JPG} /var/cache/deepin/dde-daemon/image-effect/pixmix/
cd /var/cache/deepin/dde-daemon/image-effect/pixmix
for img in $(ls|egrep -v ${DEFUALT_IMG_JPG})
do
    sudo cp ${DEFUALT_IMG_JPG} ${img} && sudo chmod 600 ${img}
done
echo "重启或者注销系统生效"
```

把上面的内容，复制粘贴，保存为a.txt，终端执行:bash a.txt xxx.jpg

> 注意：脚本里面有sudo操作。建议提前配置好sudo免密码操作，参照下文

特别注意：在本文下面介绍的【系统瘦身】中，我已经使用 `sudo apt purge deepin-wallpapers`，卸载原来的壁纸，会导致注销和登陆界面显示为白色，如下图

> 建议先卸载壁纸 ，然后立马执行这部分操作，避免产生白色背景

![image.png]([https://storage.deepin.org/thread/202111291453392055_image.png](view-source:https://storage.deepin.org/thread/202111291453392055_image.png))

执行上面修改锁屏壁纸脚本，设置锁屏壁纸后，如下图

> 这个壁纸，会在开机登陆，注销登陆，锁屏，点击电源按钮操作，全屏启动器显示

![image.png]([https://storage.deepin.org/thread/2021112915072775_image.png](view-source:https://storage.deepin.org/thread/2021112915072775_image.png))

## **3.7 字体安装和设置**

把喜欢的字体下载下来，放到系统里面。然后全部选中，回车。就可以自动安装字体了，如下图：

![image.png]([https://storage.deepin.org/thread/202111291218501045_image.png](view-source:https://storage.deepin.org/thread/202111291218501045_image.png))

然后在控制中心，配置系统字体，如下图：

> 注意：字体设置成功后，【控制中心】样式会立马生效，而【启动器】，【文件管理器】的字体不会立马生效，需要重启或者注销，重新登陆后生效。

![image.png]([https://storage.deepin.org/thread/202111291220113526_image.png](view-source:https://storage.deepin.org/thread/202111291220113526_image.png))下图是对比图，左边是原来的字体，右边是微软雅黑，差别还是有的。

![image.png]([https://storage.deepin.org/thread/202111291221522603_image.png](view-source:https://storage.deepin.org/thread/202111291221522603_image.png))

注销重新登陆再看看，这个时候字体就统一了，如下图：

![image.png]([https://storage.deepin.org/thread/202111291223131792_image.png](view-source:https://storage.deepin.org/thread/202111291223131792_image.png))

但是此时任务栏右侧的时间和日期的显示，就不那么和谐了，如下图：

> 时间的字号很大，日期的字号很小。而且【控制中心】调整字号变大时，时间字号变小，日期字号变大。很明显是两者共享同一个空间，产生了挤压导致的。

![image.png]([https://storage.deepin.org/thread/202111291224109865_image.png](view-source:https://storage.deepin.org/thread/202111291224109865_image.png))

关于字号的选择，建议1920x1080分辨率选择14号字体就可以了。

【这里有个BUG】

如果微软雅黑15号字体，长时间格式xxxx-xx-xx时，任务栏会产生覆盖，如下图：

![image.png]([https://storage.deepin.org/thread/202111291226387722_image.png](view-source:https://storage.deepin.org/thread/202111291226387722_image.png))

## **3.8 输入法配置**

输入法是用户与系统交互的重要方式之一，也是系统重要体验。

能在deepin上使用的输入法有很多，deepin商店提供很多主流的输入法。

当前提供的系统镜像中，依然默认安装的是fcitx输入法。通过启动器—输入法设置，即可进行输入法的配置。

因为deepin不确定用户是什么样的使用习惯，因此未对切换输入法快捷键做配置，保持默认。

**小技巧: 在未配置输入法切换快捷键之前，可以通过点击任务栏输入法图标的方式进行切换**

在正常的实用习惯中，一般都会保留中文输入和英文输入，两种输入方式。中文的输入可以选择sunpinyin,英文的输入可以选择：键盘—英语。

我个人习惯，直接卸载默认的fcitx输入法，安装fcitx5输入法。

具体教程参照[这里]([https://deepin.wiki/index.php?title=Fcitx5](view-source:https://deepin.wiki/index.php?title=Fcitx5))

1.`sudo apt purge *fcitx*`

2.`sudo apt update`

3.`sudo apt install fcitx5 fcitx5-pinyin`

4.sudo vim /etc/envirotment，添加以下内容，保存退出。

```shell
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
SDL_IM_MODULE=fcitx
```

在任务栏上，已经显示出来了fcitx5的相关快捷方式。设置"Fcitx5"开机启动

![image.png]([https://storage.deepin.org/thread/202111291243583599_image.png](view-source:https://storage.deepin.org/thread/202111291243583599_image.png))

点击"Fcitx5配置“进行，输入法配置

以下是我个人的设置：

1.  配置“键盘-英语”为第一输入法
2.  配置“拼音”为第二输入法
3.  配置输入法切换快捷键为：左shift
4.  设置输入法为单行模式
5.  换皮肤
6.  装词库
7.  修改任务栏显示样式
8.  配置中文括号

![image.png]([https://storage.deepin.org/thread/202111291246167588_image.png](view-source:https://storage.deepin.org/thread/202111291246167588_image.png))

![image.png]([https://storage.deepin.org/thread/202111291246499481_image.png](view-source:https://storage.deepin.org/thread/202111291246499481_image.png))

![image.png]([https://storage.deepin.org/thread/202111291247187487_image.png](view-source:https://storage.deepin.org/thread/202111291247187487_image.png))

![image.png]([https://storage.deepin.org/thread/202111291248343238_image.png](view-source:https://storage.deepin.org/thread/202111291248343238_image.png))

![image.png]([https://storage.deepin.org/thread/202111291249537684_image.png](view-source:https://storage.deepin.org/thread/202111291249537684_image.png))

输入法效果图，还不是特别美观，稍后美化

![image.png]([https://storage.deepin.org/thread/202111291250429006_image.png](view-source:https://storage.deepin.org/thread/202111291250429006_image.png))

【注意：这里有个BUG】

Fcitx5输入法单行模式，在文本编辑器(办公软件WPS也是)无法显示拼音预览，如下图：

![image.png]([https://storage.deepin.org/thread/202111291252026134_image.png](view-source:https://storage.deepin.org/thread/202111291252026134_image.png))

默认的Fcitx5的皮肤很素，跟系统风格不太像，这里使用fcitx5-material皮肤

![image.png]([https://storage.deepin.org/thread/202111291516357316_image.png](view-source:https://storage.deepin.org/thread/202111291516357316_image.png))

修改后的效果如下，稍微美观了一些：

![image.png]([https://storage.deepin.org/thread/202111291518385418_image.png](view-source:https://storage.deepin.org/thread/202111291518385418_image.png))

Fcitx5词库的安装，在上面给出的wiki里面有介绍，这里不做介绍。需要强调的一点就是，fcitx5支持安装搜狗输入法的词库，这个是非常棒的体验。

Fcitx5默认情况下，在任务栏现实的输入法图标是红色的，这个本人不是特别喜欢，就直接换掉了。

需要做的就是：找到这个Fcitx5任务栏图标，准备一个自己喜欢的图标替换掉这个问题，重新登陆即可

这个图标的路径是：

`/usr/share/icons/hicolor/48x48/apps/org.fcitx.Fcitx5.fcitx-pinyin.png`

替换动作：

`sudo cp org.fcitx.Fcitx5.fcitx-pinyin.png /usr/share/icons/hicolor/48x48/apps/org.fcitx.Fcitx5.fcitx-pinyin.png`

修改前：![image.png]([https://storage.deepin.org/thread/202111300942394521_image.png](view-source:https://storage.deepin.org/thread/202111300942394521_image.png))

修改后：![image.png]([https://storage.deepin.org/thread/202111300946101955_image.png](view-source:https://storage.deepin.org/thread/202111300946101955_image.png))

对比一下 ，风格统一了很多。

Fcitx5中文输入法无法输入中文括号【，这是需要修改配置文件的。该配置文件路径是：

`/usr/share/fcitx5/punctuation/punc.mb.zh_CN`

修改这个文件，如下图。这样就可以输入中文括号了【】

> 可以从网上复制粘贴进去

![image.png]([https://storage.deepin.org/thread/202111300949218526_image.png](view-source:https://storage.deepin.org/thread/202111300949218526_image.png))

Fcitx5安装完成在之后，启动器会显示出一些感觉没有什么用的图标，如下图

![image.png]([https://storage.deepin.org/thread/202111300951305966_image.png](view-source:https://storage.deepin.org/thread/202111300951305966_image.png))

这个需要通过我们自己来隐藏

隐藏键盘布局查看器：`sudo vim /usr/share/applications/kbd-layout-viewer5.desktop`，添加 `NoDisplay=true`，即可实现隐藏，如下图

![image.png]([https://storage.deepin.org/thread/202111300953116830_image.png](view-source:https://storage.deepin.org/thread/202111300953116830_image.png))

隐藏Fcitx5，需要编辑的文件 `/usr/share/applications/org.fcitx.Fcitx5.desktop`

隐藏Fcitx5配置，需要编辑的文件 `/usr/share/applications/fcitx5-configtool.desktop`

具体操作都是在文件最后一行下面，添加 `NoDisplay=true`，保存退出

查看启动器，发现已经隐藏

**小提示：**

**所有想要隐藏的图标，均可通过这种方式操作，只需要找对配置文件即可。包括20.3版本中出现的Device Format图标，甚至之前的“计算机”，“回收站“，”文件管理器“等，都可以隐藏**

**特别注意**：

安装fcitx5以后，可能会导致系统应用“截图录屏”的快捷键没法使用，这时候需要安装一个依赖包

终端执行：`sudo apt install qdbus-qt5`，安装这个依赖包之后，就能够正常使用“截图录屏”快捷键了

【2022-01-08更新，记录当前20.3,fcitx5的问题】：

-   无法通过配置导入词库
-   任务栏托盘->右键重启，执行后fcitx5图标消失

# 4\. 系统高级折腾

## 4.1 系统瘦身

### 4.1.1 卸载原创应用

安装完系统之后，【终端】执行命令 `df -h`可以看到系统安装完成，所占磁盘比例还是挺少的

![image.png]([https://storage.deepin.org/thread/202111291325509565_image.png](view-source:https://storage.deepin.org/thread/202111291325509565_image.png))

但是deepin默认安装的一些应用，如果我们不喜欢，完全可以卸载。

这里介绍如何卸载deepin原创应用来实现系统瘦身。

先了解一下深度的一些原创应用和集成的第三方应用名字：

```shell
org.deepin.browser      深度浏览器
org.deepin.downloader   深度下载器
deepin-manual           帮助手册
deepin-draw             深度画板
deepin-album            深度相册
deepin-music            深度音乐
deepin-mail             深度邮箱
deepin-boot-maker       启动盘制作
deepin-diskmanager      磁盘管理器
deepin-devicemanager    设备管理器
deepin-font-manager     字体管理器
deepin-system-monitor   系统监视器
deepin-log-viewer       日志查看器
dde-printer             打印管理器
deepin-reader           文档查看器
deepin-voice-note       语音记事本
deepin-compressor       归档管理器
deepin-movie            深度影院
dde-introduction        欢迎
dde-calendar            深度日历
deepin-calculator       深度计算器
deepin-camera           深度相机
deepin-feedback         用户反馈
deepin-screen-recorder  截屏录屏
deepin-image-viewer     深度看图
deepin-deb-installer    软件包安装器
deepin-editor           文本编辑器
deepin-screensaver      屏幕保护程序
deepin-wallpapers       壁纸
deepin-shortcut-viewer  快捷键预览
deepin-app-store        深度应用商店
dde-grand-search       全局搜索服务
dde-clipboard           剪切板
######################################
simple-scan             扫描易
LibreOffice*            办公软件
onboard                 屏幕键盘
nano                    nano编辑器
onboard                 屏幕键盘
######################################
查看和卸载内核
dpkg -l |egrep "linux-image|linux-headers"
sudo apt purge
商店常用:
com.163.music
com.qq.weixin.deepin
```

了解了这些应用的名称，就可以直接命令行卸载了，也可以通过“启动器右键卸载“，但是只能卸载一部分。

本人通过启动器右键卸载了以下软件：

**音乐，相册，画板，文档查看器，语音记事本，下载器，扫描易，设备管理器，启动盘制作工具，磁盘管理器，字体管理器，日志收集工具，五子棋，连连看，Liboffice，相机，用户反馈，浏览器**

以下是无法通过启动器卸载的软件，通过命令行卸载 `sudo apt purge 软件名`：

**邮箱，屏幕键盘，帮助手册，屏幕保护程序，壁纸，欢迎，系统监视器，全局搜索功能**

上述操作完成之后，剩余的东西不多了，如图（部分图标在后文进行隐藏）：

![image.png]([https://storage.deepin.org/thread/202111291345026825_image.png](view-source:https://storage.deepin.org/thread/202111291345026825_image.png))

因工作需要，我保留了以下原创应用：

**文件管理器，打印机管理，日历，计算器，终端，文本编辑器，看图，影院，截图录屏，软件包安装器，归档管理器**

其他需要的软件，后面再在自行安装。

下图是卸载上述应用之后的系统空间，发现节省了1.1G空间。

![image.png]([https://storage.deepin.org/thread/20211129134712306_image.png](view-source:https://storage.deepin.org/thread/20211129134712306_image.png))

### 4.1.2 卸载多余内核

内核的选择，主要看个人喜好和硬件新旧。

新版本的内核对新的硬件支持很多，比如打印机，蓝牙，屏幕，显卡等。

旧的内核版本也有的比较成熟稳定，适合工作和生常环境。

> 如果没有问题，一般不建议升级内核，容易出现各种问题。但是如果全新安装就已经有各种问题，建议升级内核看看。

**更新内核：**

终端执行命令：`sudo apt install linux-image-deepin-stable-amd64 linux-headers-deepin-stable-amd64`

**查看内核：**

终端执行命令：`sudo dpkg -l | egrep "linux-header|linux-image"`

**卸载内核：**

终端执行命令：`sudo apt purge linux-headers-5.xx.xx-amd64-desktop ; linux-image-5.xx.xx-amd64-desktop`

### 4.1.3 清理系统日志

系统日志目录放置在/var/log/下面，专业认识清理当前目录即可。

也可以通过jourctl命令工具来管理系统日志

参考这篇博客：[《journalctl清理journal日志》]([https://www.cnblogs.com/jiuchongxiao/p/9222953.html](view-source:https://www.cnblogs.com/jiuchongxiao/p/9222953.html))

### 4.1.4 使用其他工具

使用商店或者社区商店里面的系统清理工具，一般都能够清理一些不用的垃圾信息，节省空间

## 4.2 配置文件路径

以下是比较常用的系统路径：

-   默认配置路径：/etc/alternatives
-   系统主题路径：/usr/share/icons
-   系统字体路径：/usr/share/fonts或~/.local/share/fonts
-   应用快捷方式destop路径：/usr/share/applications/或者~/.local/share/applications
-   鼠标右键扩展路径：/usr/share/deepin/dde-file-manager/oem-menuextensions
-   文件管理器右键选项路径：/usr/share/applications/context-menus
-   文件管理器磁盘扩展路径：/usr/share/dde-file-manager/extensions/appEntry
-   任务栏插件路径：/usr/lib/dde-dock/plugins/
-   终端配色文件路径：/usr/share/terminalwidget5/color-schemes
-   权限询问路径：/usr/share/polkit-1/actions/
-   背景图片路径：/var/cache/deepin/dde-dameon/image-effect/pixmix
-   网络连接信息路径：/etc/NetworkManager/system-connections/
-   用户基本信息路径：/var/lib/AccountsService/deepin/users/
-   用户图像默认路径：/var/lib/AccountsService/icons（bigger目录也有）
-   右键新建文档模板路径：/usr/share/templates/或者~/.Templates
-   回收站路径：~/.local/share/Trash
-   应用开机自启动路径：~/.config/autostart
-   控制中心-自定义快捷键配置文件：～/.config/deepin/dde-daemon/keybinding/custom.init，注销或重启生效
-   控制中心-用户头像路径：/var/lib/AccountsService/icons
-   控制中心-系统音效路径：/usr/share/sounds/deepin/stereo/

注意：deepin20.3系统默认没有/usr/share/templates目录，但是这个目录依然可以自动创建和使用。

一般桌面右键-新建文档时，如下图

![image.png]([https://storage.deepin.org/thread/202111301005244087_image.png](view-source:https://storage.deepin.org/thread/202111301005244087_image.png))

我想添加一个新建文档-Markdown文档的选项，那么就可以进行以下操作

1.  终端执行 `sudo mkdir -p /usr/share/templates/.source`
2.  终端执行：`sudo touch /usr/share/templates/.source/markdown-template.md`
3.  创建一个markdown.desktop文件，内容如下：
    
    ```plaintext
    [Desktop Entry]
    Name=Markdown Doc
    Name[zh_CN]=Markdown文档
    Comment=Enter MD filename:
    Comment[zh_CN]=请输入Markdown文档名称：
    Type=Link
    URL=.source/markdown-template.md
    ```
    

然后把markdown.desktop拷贝到 `/usr/share/templates/`目录下即可，注销重新登陆以后，效果如下图

![image.png]([https://storage.deepin.org/thread/202111301010462879_image.png](view-source:https://storage.deepin.org/thread/202111301010462879_image.png))

## 4.3 开发环境

deepin软件源中，一些环境的版本比较低，如果不想使用源内软件，自然可以编译安装，添加环境变量即可

### 4.3.1 配置root密码

终端执行：sudo passwd 设置root密码

### 4.3.2 配置普通sudo无密码

> 首先卸载nano编辑器 ，个人不喜欢用nano。终端执行：sudo apt purge nano

终端执行：`sudo visudo`，编辑内容如下：

```shell
%sudo   ALL=(ALL:ALL) NOPASSWD:ALL
```

终端执行：`sudo usermod -a -G sudo liwl`，将liwl用户添加到sudo组，因为sudo组又配置了NOPASSWD参数，所以liwl执行sudo时，不需要再输入密码

### 4.3.1 安装git

终端执行：`sudo apt install git`

配置git显示中文信息：

`git config --global i18n.commitencoding utf-8`  
`git config --global i18n.logoutputencoding utf-8`

### 4.3.4 安装openldap

参照博客：[《deepin部署openldap服务》]([https://www.cnblogs.com/liwanliangblog/p/14968741.html](view-source:https://www.cnblogs.com/liwanliangblog/p/14968741.html))

### 4.3.5 安装nodejs

我个人系统安装nodejs，主要实现使用右键发送文章到“博客园”或者“为知笔记”

具体实现，参照博文:[《Deepin15和20使用命令行快捷键鼠标右键发送至博客园和为知笔记》]([https://www.cnblogs.com/liwanliangblog/p/12794179.html](view-source:https://www.cnblogs.com/liwanliangblog/p/12794179.html))

### 4.5.6 安装和使用jdk

之前写过一篇配置右键或者双击打开jnlp文件的博客：[《DeepinV20右键打开或双击打开jnlp》]([https://www.cnblogs.com/liwanliangblog/p/14029044.html](view-source:https://www.cnblogs.com/liwanliangblog/p/14029044.html))

## 4.4 终端定制

关于deepin-terminal的定制，本人写过很多博客，具体参见：[《deepin-termial改造之路》]([https://www.cnblogs.com/liwanliangblog/p/14469250.html](view-source:https://www.cnblogs.com/liwanliangblog/p/14469250.html))

因为内容比较多，这里不做详细介绍。

本人已经把改造后的代码保存备份，每次安装完系统以后，便重新编译deepin-terminal

大致步骤：

1.终端执行 `sudo apt purge deepin-terminal`

2.下载源码后，切换分支

3.终端执行：`sudo apt build-dep .`

4.终端执行：`cmake ..; make; sudo make instlal`

安装完成后，从启动器把

## 4.5 其他配置

### 4.5.1 修改启动器图标

这里直接提供一个修改脚本，很简单：

```shell
#!/bin/bash
# 本脚本用于修改deepin启动起logo
PLACE=/usr/share/icons/bloom/places
SVG=$1
if [ $# -ne 1 ];then
        echo "未选择svg"
        exit 0
fi
for i in 128 16 24 256 32 48 512 64 96
do
        echo $i
        sudo cp ${SVG} ${PLACE}/$i/deepin-launcher.svg
done

```

上面文件，复制粘贴保存为a.txt。终端执行：`bash a.txt xxx.svg `

> 得自己事先准备一个喜欢的svg格式图标，比如我喜欢的暗色win10启动器

![image.png]([https://storage.deepin.org/thread/202111291443084086_image.png](view-source:https://storage.deepin.org/thread/202111291443084086_image.png))

具体操作：

1.控制中心设置主题

![image.png]([https://storage.deepin.org/thread/202111291534393371_image.png](view-source:https://storage.deepin.org/thread/202111291534393371_image.png))

2.执行脚本：`sudo bash a.txt deepin-launcher.svg，执行结束后，切换到bloom主题，启动器已经变化，效果如图`

![image.png]([https://storage.deepin.org/thread/202111291536427359_image.png](view-source:https://storage.deepin.org/thread/202111291536427359_image.png))

![image.png]([https://storage.deepin.org/thread/202111291537261621_image.png](view-source:https://storage.deepin.org/thread/202111291537261621_image.png))

**特别注意：**

因为icons依赖的问题，系统存放icron目录的Papirus和bloom目录是不能删除的，其他目录可以删除，节省空间

![image.png]([https://storage.deepin.org/thread/202111291538489613_image.png](view-source:https://storage.deepin.org/thread/202111291538489613_image.png))

控制中心，只剩下了这两个主题

![image.png]([https://storage.deepin.org/thread/202111291539236297_image.png](view-source:https://storage.deepin.org/thread/202111291539236297_image.png))

![image.png]([https://storage.deepin.org/thread/202111291527312083_image.png](view-source:https://storage.deepin.org/thread/202111291527312083_image.png))

如果希望删掉Papirus主题，能够节省112M空间。但是会导致在是用其它主题时，任务栏输入法图标消失。

这里介绍如何修复这个问题：

在删除Papirus主题前，先终端执行：

`sudo cp /usr/share/icons/Papirus/16x16/devices/input-keyboard.svg /usr/share/icons/bloom/status/48/`，这条是修复卸载了Papirus后，任务栏输入法的图标丢失问题

`sudo cp /usr/share/icons/Papirus/48x48/apps/software-store.svg /usr/share/icons/bloom/apps/48/deepin-appstore.svg`，这一条是修复卸载了Papirus后 ，卸载软件，顶栏发送通知的图标丢失问题

这样就可以愉快地删除Papirus主题啦，如下图

![image.png]([https://storage.deepin.org/thread/202112021602537791_image.png](view-source:https://storage.deepin.org/thread/202112021602537791_image.png))

### 4.5.2 修改引导界面图片

这个比较简单，选择一张喜欢的图片，直接拖入控制中心-通用-启动菜单，就可以修改了

> 如果是双系统的话，这个界面还可以调整启动顺序，设置启动延迟和主题。

![image.png]([https://storage.deepin.org/thread/202111291529118090_image.png](view-source:https://storage.deepin.org/thread/202111291529118090_image.png))

![image.png]([https://storage.deepin.org/thread/202111291529348497_image.png](view-source:https://storage.deepin.org/thread/202111291529348497_image.png))

个人笔记本安装的是单系统，因此不需要启动延迟和主题，如下图

![image.png]([https://storage.deepin.org/thread/202111291531192269_image.png](view-source:https://storage.deepin.org/thread/202111291531192269_image.png))

但是这个设置，依然会保留1秒的启动延迟，不太友好。那就需要命令行直接修改/etc/default/grub了，如下图

修改GRUB\_TIMEOUT=0，然后执行 `sudo update-grub2即可。这样开机就不会有延迟了。`

> 双系统建议不要修改到0，并且建议修改到30秒，以便选择操作系统

![image.png]([https://storage.deepin.org/thread/202111291532515784_image.png](view-source:https://storage.deepin.org/thread/202111291532515784_image.png))

### 4.5.3 修改锁屏字体

虽然修改了系统字体，但是在开机登陆，注销登陆界面，字体还是原来的字体，没有变成我设置的“微软雅黑”

![image.png]([https://storage.deepin.org/thread/202111291521179129_image.png](view-source:https://storage.deepin.org/thread/202111291521179129_image.png))

这里需要做以下操作，进行修改：

1.先把之前安装的字体，拷贝到系统路径下

![image.png]([https://storage.deepin.org/thread/202111291522475488_image.png](view-source:https://storage.deepin.org/thread/202111291522475488_image.png))

修改文件/etc/lightdm/deepin/xsettingsd.conf，内容如下

![image.png]([https://storage.deepin.org/thread/202111291524461697_image.png](view-source:https://storage.deepin.org/thread/202111291524461697_image.png))

修改完成，保存退出。注销界面已经变化，比上面没修改之前，美观了一些了。

![image.png]([https://storage.deepin.org/thread/202111291525448782_image.png](view-source:https://storage.deepin.org/thread/202111291525448782_image.png))

### 4.5.4 配置无密码验证

如下图，一般安装软件操作时，需要输入账户密码。

![image.png]([https://storage.deepin.org/thread/202111291541443709_image.png](view-source:https://storage.deepin.org/thread/202111291541443709_image.png))

通过修改配置文件，来实现无密码安装

终端执行：`sudo find / -name org.kubuntu.qaptworker3.policy 2>/dev/null`

找到org.kubuntu.qaptworker3.policy文件位置，修改

![image.png]([https://storage.deepin.org/thread/202111291550118341_image.png](view-source:https://storage.deepin.org/thread/202111291550118341_image.png))

为下图所示，即auth\_admin\_keep改成yes即可，保存退出。

![image.png]([https://storage.deepin.org/thread/202111291548294680_image.png](view-source:https://storage.deepin.org/thread/202111291548294680_image.png))

这时候安装软件，就不会提示输入密码了。如下图，双击直接安装

![image.png]([https://storage.deepin.org/thread/202111291555422865_image.png](view-source:https://storage.deepin.org/thread/202111291555422865_image.png))

### 4.5.5 修复apt没有公钥

安装了edge浏览器之后，使用 `sudo apt update`命令之后，会出现以下没有公钥的问题。解决这个问题的方式有两个：`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 字符串`

1.  注销掉edge的仓库，以后不用了，直接官网下载（当然这个比较那啥）
2.  添加这个公钥。具体操作是终端执行：`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 字符串 ，下图“字符串”是EB3E94ADBE1229CF`

![image.png]([https://storage.deepin.org/thread/202111301353533814_image.png](view-source:https://storage.deepin.org/thread/202111301353533814_image.png))

![image.png]([https://storage.deepin.org/thread/202111301358379409_image.png](view-source:https://storage.deepin.org/thread/202111301358379409_image.png))

### 4.5.6 删除右键新建文档的重复模板

安装了wps之后，在鼠标右键，新建文档时，会多出很多模板。

1.  `rm -rf ~/.Templates`下的内容
2.  `sudo rm -rf /usr/share/templates/wps*`

注销，重新登录即可

### 4.5.7 配置ctrl+d删除文件

终端执行：`sudo apt install xdotool`

创建ctrld.sh，内容如下：

```shell
#!/bin/bash
AW=$(xwininfo -int -id $(xdotool getactivewindow) | grep "Window id")
if [[ $AW =~ "桌面" ]] || [[ $AW =~ " — 文件管理器" ]]
then 
    bash -c 'xdotool keyup d key --delay 0 --clearmodifiers Delete'
else
    bash -c 'xdotool keyup d key --delay 0 --clearmodifiers ctrl+D'
fi

```

把ctrld.sh放在一个目录下：`~/.liwl/deepin/ctrld.sh`

在控制中心配置快捷键，如图：

![image.png]([https://storage.deepin.org/thread/202304041708013537_image.png](view-source:https://storage.deepin.org/thread/202304041708013537_image.png))

# 5\. 第三方应用

以下是本人折腾过的第三方应用软件，主要介绍其使用的一些小技巧。

## 5.1 微信

微信的安装，通过**应用商店**，或者直接命令行执行：`sudo apt install com.qq.weixin.deepin`

配置微信窗口打开快捷键

控制中心——键盘和语言——快捷键，下拉到最下面，自定义快捷键，点击+，进行配置。

如下图：

> 注：在deepin20.3版本上，wechat的deepinwine工具放在全局目录/opt/deepinwine目录下。

![image.png]([https://storage.deepin.org/thread/202111281624428237_image.png](view-source:https://storage.deepin.org/thread/202111281624428237_image.png))

## 5.2 坚果云

本人对坚果云的改造，都在这个博客了-[《deepin优雅地使用坚果云攻略》]([https://www.cnblogs.com/liwanliangblog/p/15179592.html](view-source:https://www.cnblogs.com/liwanliangblog/p/15179592.html))

主要亮点是：1.迁移备份目录后删除主目录的文件夹，2.瘦身，3.解决安装坚果云之后md格式显示为空白

这里聊一下我自己在4K屏幕，初次安装坚果云，打开登陆界面显示不全的问题：

找一个1920x1080的显示器，外接一下，使用这个显示器启动界面呗。或者在启动器坚果云的启动图标右键选择【禁止缩放】设置好目录后，在解禁缩放。

## 5.3 迅雷下载

除了deepin原创的应用“下载器”以外，Linux桌面系统提供的下载工具有很多。迅雷Linux版是选择之一。

> 我选择 free download manager

关于迅雷，只介绍以下内容：

商店下载迅雷。现在没法通过x来退出，点击x只会窗口最小化到任务栏。

退出：右键任务栏迅雷图标，选择“关闭所有”或者“强制退出”

**隐藏任务栏图标**：如果不想退出，但是又不想它显示在任务栏，可以单击任务栏右侧托盘的图标。

## 5.4 搜狗输入法

搜狗输入法下载方式：1.应用商店，2.官方网站

由于本人不是用搜狗输入法，这里只介绍如何隐藏搜狗输入法悬浮面板。

vim ~/.config/sogoupinyin/conf/env.ini，修改StatusAppearance=0，保存退出

搜狗输入法最新版本已经在设置面板内置了【隐藏面板】的选项

安装旧版本的搜狗：`sudo apt install sogoupinyin=2.2.0.0108`

## 5.5 网易云音乐

网易云音乐的安装方式有三个：1.官方下载，2.商店安装，3.命令行安装

命令行安装：`sudo apt install com.163.music`

网易云Linux版本，能够设置全局快捷键，能够播放本地音乐，单纯听音乐足够

这里主要介绍本人出现的问题：

1.  网易云功能模块：私人FM，发现音乐等出现连接网络失败的问题
2.  网易云音乐在4K显示屏无法正常显示的问题

第一个问题：在网易云音乐其他平台，比如手机app,开启【个性化推荐】功能即可

第二个问题：4K或者高清屏幕缩放问题：

修改/usr/share/applications/com.163.music.desktop文件，

Exec字段修改为：

`Exec=env QT_SCALE_FACTOR=2.25 /opt/apps/com.163.music/files/bin/netease-cloud-music %U`

> 2.25是我4K屏幕缩放比例，具体参数自行调整即可

## 5.6 Edge浏览器

[edge的各版本下载地址]([https://packages.microsoft.com/repos/edge/pool/main/m/microsoft-edge-stable/](view-source:https://packages.microsoft.com/repos/edge/pool/main/m/microsoft-edge-stable/))

下载后双击安装即可

从任务栏把"Microsoft Edge"发送到任务栏，然后配置启动快捷键：

![image.png]([https://storage.deepin.org/thread/202111291438408107_image.png](view-source:https://storage.deepin.org/thread/202111291438408107_image.png))

此时便可以通过命令：super+1，启动edge浏览器了，类似win10的快捷键启动任务栏程序一样

## 5.7 向日葵远程

deepin使用远程桌面的方式很多，向日葵是其中一个选择。

这里不去介绍具体的使用方法，只是贴出一个解决【向日葵开启后，deepin特效消失】的解决方案

vim ~/.config/kwinrc，修改\[Compositing\]，添加：

WindowsBlockCompositing=false

UnredirectFullscreen=true

重启或者注销，重新登陆生效

## 5.8 百度网盘

百度网盘用来当离线存储也不错。pan.baidu.com提供了下载地址

这里介绍一个使用python使用百度网盘的工具：bypy

具体教程参照这个博客：[《python操作百度网盘》]([https://blog.csdn.net/zhou_438/article/details/104857791](view-source:https://blog.csdn.net/zhou_438/article/details/104857791))

我本人安装了bypy在以后，创建了一个右键扩展，便于直接右键上传文件到百度网盘。

创建一个deepin-sendto-baidunetdisk.desktop的文件，内容如下

```plaintext
[Desktop Entry]
Name=上传到百度网盘
Type=Application
Icon=baidunetdisk
Encoding=utf-8
Exec=/usr/local/bin/bypy upload %U
Terminal=false
X-DFM-MenuTypes=SingleDir;SingleFile;MultiFileDirs;
```

保存退出后，执行 `sudo cp deepin-sendto-baidudisk.desktop /usr/share/applications/`目录，效果如下图

![image.png]([https://storage.deepin.org/thread/202111301338288201_image.png](view-source:https://storage.deepin.org/thread/202111301338288201_image.png))

> 2022年1月6号，最新版本的百度网盘，支持了文件管理器的右键扩展和磁盘扩展，但是新版本网盘广告有些多。

## 5.9 我的其他软件

| 软件名称 | 软件类型 | 下载方式 | 其他  | 备注  |
| --- | --- | --- | --- | --- |
| vscode | 代码编辑器/IDE | 官网下载 |     |     |
| typora | markdown编辑器 | 官网下载 |     |     |
| kvm | 虚拟化软件 | 命令行安装 | sudo apt install qemu-kvm virt-manager |     |
| wiznote | 笔记软件 | 官网下载 |     |     |
| uTools | 效率工具 | 官网下载 | 比较赞的插件，vim模式的todo,网页快开 |     |
| vncviewer | 远程桌面 | 官网下载 | 高分辨率下vncviewer界面太小 |     |
| wps | 办公软件 | 官网/商店 | 建议官网下载 |     |
| dbeaber | 数据库管理软件 | 官网下载 |     |     |
| easyconnect | vpn软件 | 官网下载 | 使用web登陆vpn时，会提示下载 |     |
| dconf-editor | gsettings图形化设置界面 | apt install |     |     |
| d-feet | bus总线 | apt install |     |     |

# 6\. 高清大合照

最后来几张oled屏幕下的大合照吧

桌面和启动器

![启动器.png]([https://storage.deepin.org/thread/202111291600583275_启动器.png](view-source:https://storage.deepin.org/thread/202111291600583275_%E5%90%AF%E5%8A%A8%E5%99%A8.png))

控制中心

![控制中心.png]([https://storage.deepin.org/thread/202111291601329024_控制中心.png](view-source:https://storage.deepin.org/thread/202111291601329024_%E6%8E%A7%E5%88%B6%E4%B8%AD%E5%BF%83.png))

文件管理器

![文件管理器.png]([https://storage.deepin.org/thread/20211129160148133_文件管理器.png](view-source:https://storage.deepin.org/thread/20211129160148133_%E6%96%87%E4%BB%B6%E7%AE%A1%E7%90%86%E5%99%A8.png))

终端

![终端.png]([https://storage.deepin.org/thread/202111291602036130_终端.png](view-source:https://storage.deepin.org/thread/202111291602036130_%E7%BB%88%E7%AB%AF.png))