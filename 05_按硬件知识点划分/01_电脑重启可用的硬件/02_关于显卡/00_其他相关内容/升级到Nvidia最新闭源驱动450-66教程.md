---
title: 升级到Nvidia最新闭源驱动450教程
description: 
published: true
date: 2022-10-18T00:56:34.448Z
tags: 升级 闭源启动 nvidia
editor: markdown
dateCreated: 2022-06-28T03:33:13.262Z
---

# 升级到Nvidia最新闭源驱动450.66教程
### 备注：按照这个教程安装的版本目前是450.66，以后可能会是更新的版本。文中出现的版本号450.66仅供参考。

------

**重大故障警告：**
**操作不慎可能会导致系统完全无法启动！**
**即使完全按照教程进行操作，也不一定会成功！**
**如果失败，你将无法进入图形界面，只能进入Linux控制台（纯文本命令行界面）*****\*进行驱动修复工作！\****
***\*在最坏的情况下，系统会完全没有图像显示，你只能启动到另一个系统进行修复工作，或者必须进行重装！
\****
***\*如果你没有Linux命令行经验，但还是想进行尝试，请立即备份数据，并做好系统无法恢复、必须重装的准备！\****

------

UOS 20 自带的Nvidia闭源驱动是450.51，Nvidia官网最新的Linux驱动是450.66。虽然UOS官方软件源（软件仓库）没有450.66驱动，但我们可以通过添加其他Linux发行版（这里是 Debian sid 不稳定版）的软件源来实现升级到450.66驱动。

升级有什么好处？~~在运行Wine应用的时候，黑窗口出现的频率减少了。此外，游戏性能方面可能也会有些许改善。但总体上差别不大，如果你担心系统损坏，强烈建议你不要升级！~~没有好处，自从官方源把驱动从440.59升级到450.51，两者都差别就不大了。强烈不建议这样安装，除非你想装CUDA。官方源带的是CUDA 9.2，sid源里面有CUDA 10。注意：这里只有显卡驱动安装教程，没有CUDA安装教程。以后可能会写个CUDA安装教程。

此外，你也可以使用`buster-backports`源（sid软件包反向移植到debian10），比`sid`源更可靠，因为UOS和deepin v20就是基于debian 10，所以陷入依赖地狱的风险更小，甚至可以长期启用。不过我之前试了一下，如果用[这个教程安装显卡驱动](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45NDgyOC4xLmh0bWw.)，那么从`buster-backports`源里安装的CUDA 10就不能正常工作，运行CUDA samples时报错说找不到GPU。

```swift
deb https://mirrors.aliyun.com/debian buster-backports main contrib non-free
```

使用`buster-backports`源安装的方法见该贴：[https://hu60.cn/q.php/bbs.topic.98302.html](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45ODMwMi5odG1s)

------

## 升级方法：

1. 打开开发者模式（控制中心 > 通用 > 开发者模式）。
2. 打开终端（在应用列表里找“终端”），粘贴如下命令后回车（此操作是为了确保你已经安装旧版Nvidia闭源驱动的所有组件）：

```bash
sudo  apt  install  -y  nvidia-driver  nvidia-smi  nvidia-settings  deepin-nvidia-prime  nvidia-vulkan-icd  'vulkan-utils|vulkan-tools'  nvidia-driver-libs:i386  libnvidia-ml1:i386  libxnvctrl0:i386  libvulkan1  libvulkan1:i386
```

此时会提示你输入密码，输入你的UOS开机密码即可。输入时不会显示任何内容，这是正常现象，输完回车即可。

如果提示找不到`deepin-nvidia-prime`，换成以下两条命令：

```bash
wget https://file.winegame.net/packages/deepin/mgpu/mgpu-prime_0.2.0_amd64.deb

sudo  apt  install  -y  nvidia-driver  nvidia-smi  nvidia-settings  nvidia-vulkan-icd  'vulkan-utils|vulkan-tools'  nvidia-driver-libs:i386  libnvidia-ml1:i386  libxnvctrl0:i386  libvulkan1  libvulkan1:i386  ./mgpu-prime_0.2.0_amd64.deb
```

**重大故障警告：**
**在完成教程的第4步之前，不要运行 apt upgrade 命令或其他任何命令对系统进行升级，否则系统肯定会出问题。如果在此期间，系统的自动更新功能检测到可升级内容，不要点击安装！**
**此外，给高手的提醒：不建议升级 nvidia-settings 这个软件包，因为它的依赖太多，升级后恐怕会陷入依赖包地狱，以后安装UOS官方源的软件会异常困难。不升级** ***\*nvidia-settings 不会影响Nvidia控制面板的正常使用。\****

1. 继续在终端输入如下命令，一行一行粘贴并回车（不包括 # 井号开头的行）：

```bash
# 添加包含Nvidia 450.66驱动的软件源
echo 'deb http://mirrors.aliyun.com/debian sid main contrib non-free' | sudo tee /etc/apt/sources.list.d/debian-sid.list
# 更新软件包列表
sudo  apt  update

# 运行完会提示你“有 1492 个软件包可以升级”，不要激动。这不是我们系统的升级包，不要升级，否则系统肯定会出问题！
# 安装新版驱动
sudo  apt  install  nvidia-driver  libnvcuvid1  libnvidia-encode1  nvidia-vdpau-driver  vdpau-va-driver

# 需要输入y并回车来确认安装。
# 正常情况下应该提示“升级了 42 个软件包”，多十几个少十几个也是正常的。
# 如果看到升级几百个软件包的情况，说明教程已过时，你应该输入n拒绝安装，然后直接执行第4步的清理工作。
# 安装过程中会出现“失败：目录非空”等字样，这是正常现象，无需在意。
# 此外还会弹出“加载了不匹配的 NVIDIA 内核模块”提示框，按回车进行“确定”即可。
# 升级新版驱动的附加组件
sudo apt install nvidia-egl-common nvidia-installer-cleanup nvidia-kernel-common nvidia-legacy-check nvidia-modprobe nvidia-persistenced nvidia-support nvidia-vulkan-common libnvidia-ml1:i386

# 正常情况下应该提示“升级了 8 个软件包”，并且无需输入y进行确认。
# 要求你输入y，并且看到升级几百个软件包的情况，说明教程已过时，你应该输入n拒绝安装，然后直接执行第4步的清理工作。
# 更新启动文件（如果不执行该命令，驱动可能会不生效）
sudo update-initramfs -k all -u
```

1. 清理工作：上述升级过程借用了 Debian sid 不稳定版的软件源，该仓库除了新版Nvidia驱动，还有一千多个其他软件的所谓“新版本”，但这些“新版本”和UOS不兼容，会对日后UOS系统的软件安装升级造成负面影响。所以在完成Nvidia驱动升级后，我们必须清理掉这个新加的软件源，以下是方法：

继续在终端中粘贴命令：

```bash
# 删除刚加的软件源
sudo  rm  /etc/apt/sources.list.d/debian-sid.list
# 清理软件源缓存
sudo  apt  clean
# 清理多余的软件包
sudo  apt  autoclean
```

1. 完成第4步后，就可以重启电脑了。重启后如果顺利出现图形界面，你就可以打开“NVIDIA X 服务器设置”应用来确认是不是升级成功了。升级成功后界面上应该有如下文字：

```bash
NVIDIA Driver Version:  450.66
```

![点击查看大图](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzM3YjM3ZTYwZDc1NGIyMGRiODJjZDk4MGNlNjlkM2FmMTIyMzk5LnBuZw..)

如果无法进入图形界面，请进行下面提到的恢复操作。

1. 笔记本记得打开“Nvidia Prime渲染卸载”选项，否则某些游戏游戏打不开，提示没有可用的显卡驱动。打开方法如下：

![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzkxNDI0YTkxMzRlYmE4ZTM3NmRhYmUxMTJiMTgxNjUzMTE3MjgzLnBuZw..)

注意左侧边栏Wine旁边的齿轮按钮正常不会显示，需要把鼠标放上去才会显示。

------

## 安装失败，恢复旧版驱动：

1. 以下方法适用于虽然进不去图形界面，但是显示器仍然有显示的情况。如果显示器完全没有显示，请自行搜索解决方案。如果你没有任何思路，请考虑重装。
2. 在系统启动一段时间（不用关心它是不是卡住）后，按 Ctrl + Alt + F2 组合键。系统应该会进入文本命令行界面。
   注意：如果你有多个显示器，文本命令行界面可能会显示在非常用显示器上（比如笔记本的自带屏幕上），请自行寻找一下。
   如果要返回之前的界面，按 按 Ctrl + Alt + F1 组合键。
3. 输入你的系统用户名，就是你设置开机密码的时候设置的用户名，然后回车。
4. 输入密码，回车。
5. 如果用户名和密码正确，你将进入类似图形界面中“终端”的输入模式，出现以“$”结尾的提示字符，然后你可以自由输入命令。
   如果用户名或密码错误
6. 文本命令行界面不支持中文。为了防止显示方块，需要运行如下命令切换为英文：

```bash
export  LANGUAGE=en
```

1. 运行如下命令安装旧版驱动：

```bash
# 确认你已经删除了新版驱动的软件源。
# 提示“rm: cannot remove '/etc/apt/sou*d/deb*sid.list': No such file or directory”是正常的，说明之前已经删了。
sudo  rm  /etc/apt/sou*d/deb*sid.list
# 更新软件包列表
sudo  apt  update

# 如果你连的Wifi，出现网络问题，你可能需要使用网线。
# 卸载新版驱动
sudo  apt  purge  -y  nvidia*
# 安装旧版驱动
sudo  aptitude  install  -y  nvidia-driver  nvidia-smi  nvidia-settings  deepin-nvidia-prime
```

注意：最后一步使用了`aptitude`命令，因为要对一些软件包进行降级，使用`apt`命令无法完成。不要输错。

命令完成后重启。