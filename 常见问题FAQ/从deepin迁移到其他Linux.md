---
title: 从deepin迁移到其他Linux
description: 本文的前提条件是你觉得deepin的某些方面不满足你的需求，想换用其他的Linux发行版。本文主要帮助你选择一个Linux发行版和安装部分为deepin开发的应用。 以下操作可能需要一定的动手能力和耐心。
published: true
date: 2022-08-14T00:25:46.109Z
tags: deepin应用, linux发行版
editor: markdown
dateCreated: 2022-06-15T03:48:57.823Z
---

本文由论坛用户pzm9012分享，原帖地址：https://www.yuque.com/pzm9012/ct5ume/ohlxhr

# 从deepin迁移到其他Linux

## 一 选择Linux发行版
### 1.1 预装深度桌面环境（DDE）的Linux
1. [UOS](https://www.chinauos.com/)（桌面家庭版）：相当于Deepin的商业版本，更稳定，功能更多，有官方的技术支持。付费激活（现在有免费激活活动），有30天的试用期。
2. [Manjaro Deepin](https://manjaro.org/download/#Community)：[Manjaro Linux](https://manjaro.org/)的一个社区版本，DDE能够获得较及时的更新，目前（2022-06）似乎无人维护。
3. [ExTix Deepin](https://www.extix.se/)：基于deepin，有少量修改优化。
4. [UbuntuDDE](https://ubuntudde.com/)：基于Ubuntu，有应用商店。
除此以外，[Arch Linux](https://archlinux.org/)，[OpenEuler](https://www.openeuler.org/zh/)，[openSUSE](https://www.opensuse.org/) 等发行版上也可以安装DDE。（更多请查看 [深度桌面移植](https://www.deepin.org/zh/dde/desktop-transplantation/)）
### 1.2 其他桌面环境的Linux
国内Linux发行版：比较容易上手的有[Ubuntu Kylin](https://www.ubuntukylin.com/)，CutefishOS（当前关闭，可使用[社区版](https://www.piscesys.com/)），[JingOS](https://cn.jingos.com/)（当前停更）（适合触屏设备）等。

国际Linux发行版：比较容易上手的有Ubuntu及其风味版本，Linux Mint，Manjaro，openSUSE，Fedora，elementary OS，KDE Neon等。

根据网上对这些Linux的介绍和评价，帮助自己选一个Linux发行版。如果不知道选哪个，建议从UOS，Manjaro（KDE，GNOME和Deepin桌面版本），Kubuntu，Linux Mint（Cinnamon桌面版本）等开始。
## 二 安装和配置Linux
因网络上有很多相关的教程，本文只进行简单讲述，可自行搜索更详细的教程。
#### 2.1 安装系统
访问Linux官网或者开源镜像站，下载你打算安装的Linux发行版的安装镜像（格式为iso），使用[Ventoy](https://ventoy.net/cn/index.html)等软件将U盘制作成启动盘。备份或移动电脑上的文件，使用傲梅分区助手等分区软件建一个分区（或一段未分配空间）用来装系统。从U盘启动，进入（试用版系统或）安装程序，按照提示操作，选择之前建的分区（或未分配空间），挂载点设置为/（其他挂载点分区按需设置）。
#### 2.2 初步配置系统
安装完成后，拔出U盘，重启，在启动菜单中选择刚才安装的Linux并启动。你可能需要进行以下操作：安装缺少的驱动、换国内源、更新系统、调整设置、安装输入法及需要用的应用等。

以下内容以Linux Mint（Ubuntu系）和Manjaro（Arch系）的操作为例进行讲述。
## 三 安装deepin-wine应用
#### 3.1 Linux Mint
> 以下内容参考了这篇帖子：[在ubuntu上完美使用deepin-wine5](https://bbs.deepin.org/zh/post/204884)
{.is-info}

1. 打开文件管理器，进入系统盘（文件系统）的/etc/apt/sources.list.d/，右击窗口内空白处，点击“以管理员打开”，输入密码。
2. 右击窗口内空白处，新建一个空文件，名称为deepin.conf。用文本编辑器打开这个文件，输入以下内容并保存：
`deb [by-hash=force] https://community-packages.deepin.com/deepin/ apricot main contrib non-free`
3. 以相同的方式在 /etc/apt/preferences.d/ 下新建一个名称为deepin的文件并输入以下内容：
`Package: *
Pin: origin community-packages.deepin.com
Pin-Priority: 200`
4. 打开终端，执行命令sudo apt update以刷新源。执行命令sudo apt install deepin-wine5可直接安装deepin-wine5，将命令中的deepin-wine5替换成com.qq.weixin.deepin 或com.qq.im.deepin 则可安装微信或QQ。其他应用的包名可用命令apt search [关键词]搜索。
> （提示：若想最大限度保持系统稳定，可以在安装完要用的deepin-wine应用后，删除这2个文件）
{.is-info}

#### 3.2 Manjaro
> **注意**  出于个人喜好，以下操作中我使用的是yay，与pacman可能有所不同。可以在终端执行命令sudo pacman -S yay来安装yay，再按照本文来操作。
{.is-warning}

由于AUR仓库中有deepin-wine应用，Manjaro可以直接安装这些应用。

以下是部分应用的软件包名，在终端中执行命令yay -S xxx（xxx替换为相应的包名）可安装这些应用。更多软件的包名可以在[AUR网页](https://aur.archlinux.org/)搜索。
yay -S base-level
1. 微信：deepin-wine-wechat com.qq.weixin.spark
2. QQ：com.qq.im.deepin (TIM)com.qq.tim.spark
3. WPS Office：wps-office wps-office-mui-zh-cn ttf-wps-fonts
4. 天翼云盘：cn.189.cloud.spark 
5. Microsoft Edge：microsoft-edge-beta-bin
6. Motrix：参考<https://snapcraft.io/install/motrix/manjaro>

#### 3.3 其他安装方式
1. 在其他发行版使用Deepin-Wine的终级方案:<https://zhuanlan.zhihu.com/p/379196410>
## 四 其他操作
#### 4.1 安装星火商店
星火应用商店是一款由deepin社区爱好者维护，致力于丰富Linux生态的应用商店，它支持大多数Debian系发行版。
在deepin/UOS以外的其他Debian系发行版上需要先安装依赖包，再安装商店。
1. 从官网下载客户端和依赖包。
2. 解压依赖包，打开解压后文件所在的文件夹，在文件管理器内右击空白处，点击“在终端中打开”。
3. 执行以下命令安装所有依赖包：
`sudo apt install ./*.deb`
4. 安装完成后，再安装商店客户端。在终端的安装方法与前面相同，但 * 要换成商店deb包的完整名称（spark-store_x.x.x_amd64.deb）。

> **注意**  已知在Cutefish OS（基于Debian11 bullseye，Qt版本5.15.2 ）上安装星火商店的依赖包会导致系统异常，因此不要安装依赖包，直接安装客户端，然后终端执行命令sudo apt install -f修复依赖即可。（2022.06更新）使用星火商店提供的Debian 11的依赖后再装商店也可以。
{.is-warning}

#### 4.2 安装Vek
Vek 可以用来安装和配置Wine/deepin-wine，运行和管理Windows应用等，使用相对简单。

已知[Vek](https://jacklee02.gitee.io/vek/)在Linux Mint上运行会报错：vek: error while loading shared libraries: libicui18n.so.63: cannot open shared object file: No such file or directory

我查阅了一些资料并操作后发现，下载安装这个deb包后，Vek能正常运行：<https://community-packages.deepin.com/deepin/pool/main/i/icu/libicu63_63.1-6%2Bdeb10u1_amd64.deb>

一篇vek使用教程：<https://docs.qq.com/doc/DWk1BdW1rTkxiaXVs>

#### 4.3（Manjaro）装软件时报错：一个或多个文件没有通过有效性检查！
以下为一种解决办法。
1. 从正确的地方下载要编译安装的deb包（新版旧版均可，下载地址可以参考对应软件包的AUR页面上的源地址，但之前一定要尝试过装这个软件）
2. 在文件管理器的设置打开显示隐藏文件功能，打开/home/用户名/.cache/yay/软件包名/（pacman的软件缓存不在这里），修改PKGBUILD文件，将md5sums，sha1sums等项的信息改为SKIP
3. 在当前文件夹下打开终端，执行makepkg -i

由于作者能力有限，文章可能有一些不足之处，望指出以便改进。