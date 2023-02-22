---
title: Deepinv20安装显卡驱动教程
description: 
published: true
date: 2022-10-18T00:53:20.070Z
tags: 显卡驱动
editor: markdown
dateCreated: 2022-06-28T02:30:53.851Z
---

# Deepin v20 安装显卡驱动教程
本教程不适用于ARM64和龙芯机器，如果你正在使用鲲鹏、飞腾、麒麟、龙芯等CPU的机器，请不要参考该教程。Wine游戏助手在ARM64和龙芯架构里**一定会遇到**“缺少vulkan库”的提示，请忽略该提示，勾选“强行启动，不再显示此警告”。安装ARM64和龙芯版本Wine游戏助手时应该自动安装了所需的vulkan库，不需要再自行安装。本论坛中的所有显卡驱动安装教程仅限x86 CPU（英特尔、AMD、兆芯、海光）。

------

点击底部的“**查看全部**”查看完整教程。

## 测试显卡驱动是否装好

![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cHM6Ly92a2NleXVndS5jZG4uYnNwYXBwLmNvbS9WS0NFWVVHVS1jYzhjZjA4Zi00OWY1LTRmYzUtODNjMy1lZDJhNjgzNzA0ZDQvN2Y4NzBkNGItZDUwOC00MDcyLTljODEtODY2ZDcwNWU1MzNjLnBuZw..)

如图，打开Wine游戏助手，点右上角的三个点，选“首选项”，选“硬件信息”，拉到最下面，看里面有没有“Vulkan: Supported”这一行字，如果有，说明显卡驱动装好了。

如果没有，请尝试下面的显卡驱动安装方法。如果尝试后还是没有“Vulkan: Supported”，请对硬件信息进行截图，发到[我们的QQ群、微信群](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly93d3cuaHU2MC5jbi9xLnBocC9iYnMudG9waWMuOTU5ODguaHRtbA..)，会有人提供技术支持。

如果你的显卡本身不支持Vulkan，请尝试关闭DXVK和VKD3D，[点此查看关闭方法](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy4xMDA2MDcuaHRtbA..)。

------

分流：如果你用的不是UOS/Deepin，请查看其他教程：

- [Fedora32安装显卡驱动教程](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45NTUyNC5odG1s)
- [Arch Linux安装显卡驱动教程](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45NjY5Ni5odG1s)
- [Ubuntu安装显卡驱动教程](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45ODU0MC5odG1s)
- Debian：应该可以使用下面Deepin/UOS的教程。
- Deepin/UOS显卡驱动安装教程（点“查看全部”查看完整教程）：
  - [方法1：【A卡、AMD核显、英特尔核显】从软件源安装mesa驱动](https://hu60.cn/q.php/bbs.topic.94828.html#fangfa1)
  - [方法2：【N卡】【英特尔核显+N卡】【AMD核显+N卡】使用命令行从 UOS/Deepin 官方软件源安装](https://hu60.cn/q.php/bbs.topic.94828.html#fangfa2)
  - [方法3：【N卡】【中风险】从Debian反向移植软件源安装最新N卡驱动](https://hu60.cn/q.php/bbs.topic.94828.html#fangfa3)
  - [方法4：【N卡】【不推荐】通过英伟达官网的”.run“文件进行安装](https://hu60.cn/q.php/bbs.topic.94828.html#fangfa4)

------

- 如果安装N卡驱动后不能调整亮度，请看[这个教程](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45Njk2Ni5odG1s)。

------

【只有UOS需要，其他操作系统不需要！】如果是UOS，以下所有方法均需要在”控制中心“>”通用“里打开开发者模式，如图：

![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzU0OTM3ODA1ODk0MjYyZmQ1ZTM2ZjVmMzljN2UwMDRhODI2MzcucG5n)

如果点击”在线激活“不成功，请使用”离线激活“，从 [https://www.chinauos.com/developMode](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly93d3cuY2hpbmF1b3MuY29tL2RldmVsb3BNb2Rl) 获取激活文件。具体步骤可参考 [https://jingyan.baidu.com/article/7f766daf6045a60001e1d0b1.html](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9qaW5neWFuLmJhaWR1LmNvbS9hcnRpY2xlLzdmNzY2ZGFmNjA0NWE2MDAwMWUxZDBiMS5odG1s)

如果”离线激活“依然不成功，还可以用这篇帖子“打开开发者模式”一节提到的方法打开： [https://hu60.cn/q.php/bbs.topic.94770.1.html](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45NDc3MC4xLmh0bWw.)

Deepin没有开发者模式，所以没有这种问题。

------

此外，笔记本可能需要打开“Nvidia Prime渲染卸载”选项，然后在“Vulkan ICD 加载器”那里选择“NVIDIA ICD”，否则某些游戏游戏打不开，提示没有可用的显卡驱动。打开方法如下：

![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cHM6Ly92a2NleXVndS5jZG4uYnNwYXBwLmNvbS9WS0NFWVVHVS1jYzhjZjA4Zi00OWY1LTRmYzUtODNjMy1lZDJhNjgzNzA0ZDQvMDIyYjk1ODEtZjA2Ny00YTJkLWFhYTMtNzBlYmE2MjljN2VlLnBuZw..)

------

如果你已经自行安装了显卡驱动（比如通过NVidia官网的`.run`安装包安装的），但是依然看到以下提示：

![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzRlNDE4NTVmMWVmZGQ2Y2IyYTE3NjE2Nzg1MWJhMDhmMzkwMTQucG5n)

你可以尝试通过以下命令安装缺失的Vulkan支持库：

```sql
sudo apt update
sudo apt upgrade
sudo  apt  install  libvulkan1  libvulkan1:i386  'vulkan-utils|vulkan-tools'
```

如果安装后问题依然没有解决，那你可能需要**卸载**通过`.run`安装的驱动，然后用下面的**方法2**安装驱动。

------

## 方法1：从软件源安装mesa驱动【A卡、AMD核显、英特尔核显】

打开终端，输入以下命令安装mesa驱动：

```sql
sudo apt update
sudo apt upgrade
sudo  apt  install  libgl1-mesa-dri  libgl1-mesa-dri:i386  mesa-vulkan-drivers  mesa-vulkan-drivers:i386  libvulkan1  libvulkan1:i386  'vulkan-utils|vulkan-tools'
```

如果你是5700XT这样的NAVI架构显卡，可能需要更新Linux amdgpu固件映像，在终端执行以下命令：

```bash
# 安装git和make，用于下载和安装固件映像
sudo apt install git make

# 下载固件映像
git clone --depth=1 https://mirrors.tuna.tsinghua.edu.cn/git/linux-firmware.git

# 安装固件映像
cd linux-firmware
sudo make install

# 更新linux启动文件
sudo update-initramfs -k all -u
```

更新后重启应该就能驱动了。如果5.4内核进不去，开机时去`Advanced options`菜单里选择5.7内核。

------

## 方法2：使用命令行从 UOS/Deepin 官方软件源安装【N卡】【英特尔核显+N卡】【AMD核显+N卡】

打开终端，首先运行以下两条命令（请逐行复制，不要一次复制多行）：

```csharp
# 启用i386架构，以便安装32位软件包
sudo dpkg --add-architecture i386

# 更新软件包列表
sudo apt update
```

注意`sudo`会询问你电脑的开机密码，输完回车即可，输入过程中不会有任何显示（就像输入没反应一样），这是终端的安全措施。

如果你使用Debian，可能没有预装sudo，此时你可以运行`su`命令，然后输入root密码，进入root会话，然后再执行上面的命令，不过不需要输入命令前面的sudo。

软件包列表更新好之后，就可以运行下面的命令安装驱动了，运行以下命令：

```sql
# 安装驱动程序
sudo apt update
sudo apt upgrade
sudo  apt  install  libgl1-mesa-dri  libgl1-mesa-dri:i386  mesa-vulkan-drivers  mesa-vulkan-drivers:i386  nvidia-driver  nvidia-smi  nvidia-settings  nvidia-vulkan-icd  'vulkan-utils|vulkan-tools'  nvidia-driver-libs:i386  libnvidia-ml1:i386  libxnvctrl0:i386  libvulkan1  libvulkan1:i386

# 更新启动文件
sudo update-initramfs -k all -u
```

**以下是可选步骤**，下载安装mgpu-prime软件包，用于在双显卡笔记本中强制开启独显。
**正常情况下不需要这个软件包**，Wine游戏助手可以自动使用独显。但是如果独显始终没有启用，你可以尝试安装这个软件包。

```bash
# 可选步骤，下载安装mgpu-prime软件包，用于在双显卡笔记本中强制开启独显，正常情况下不需要。
wget  https://file.winegame.net/packages/deepin/mgpu/mgpu-prime_0.2.0_amd64.deb
sudo  apt  install  ./mgpu-prime_0.2.0_amd64.deb
```

注意：你安装时的输出可能和下面的截图不同。不要在意这种细节上的差别。只要安装程序没有明确报错，就表示没有问题，你可以继续后续步骤。记住：每个人安装时，显示出来的内容都可能是不同的。

图片已过时，请执行上面文字中提到的命令。图片中的`deepin-nvidia-prime`已经不存在。

![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nL2I1YmFhZjhiOTZiZTEwOWMzZmI5ZjJmOGUxYzZmZTY5MTI3NzgucG5n)

命令会要求你输入登录密码，输入过程中不会显示你输入的内容，不是卡住了，输完密码回车即可。安装过程中可能会有几个高亮弹窗（是以字符界面显示在终端的），回车确认。

如果遇到这种弹窗（也可能不会遇到），回车确认即可，不用关心它说的内容，它说的内容总结起来只有一句话，装完驱动后要重启。

![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzE3YmFhNDgwYjNiNWVjNzIyYzUxNTNkMjNjYTYwYzczMzY1NzIucG5n)

![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzk1MDgyM2JmODg5ZjA3YzFiMGExYmI1M2U1NjhjODFmNjQzMDkucG5n)

命令全部执行完后重启，N卡就应该能驱动了。

此外，笔记本可能需要打开“Nvidia Prime渲染卸载”选项，然后在“Vulkan ICD 加载器”那里选择“NVIDIA ICD”，否则某些游戏游戏打不开，提示没有可用的显卡驱动。打开方法如下：

![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cHM6Ly92a2NleXVndS5jZG4uYnNwYXBwLmNvbS9WS0NFWVVHVS1jYzhjZjA4Zi00OWY1LTRmYzUtODNjMy1lZDJhNjgzNzA0ZDQvMDIyYjk1ODEtZjA2Ny00YTJkLWFhYTMtNzBlYmE2MjljN2VlLnBuZw..)

注意左侧边栏Wine旁边的齿轮按钮正常不会显示，需要把鼠标放上去才会显示。

#### 对双显卡用户的额外说明：

1. `mgpu-prime`软件包目前只支持`lightdm`显示管理器，不支持`gdm`显示管理器。如果你使用`gdm`（比如预装Gnome桌面的Ubuntu等发行版），则`mgpu-prime`不能帮你启用独显。请尝试安装并使用`lightdm`（目前我没有教程，你可以自己搜一下）。

2. `mgpu-prime`不支持`Wayland`会话，请使用`X11`会话。判断会话类型的方法是执行以下命令：

   ```bash
   echo $XDG_SESSION_TYPE
   ```

   会输出`x11`或者`wayland`。如果输出`wayland`，则独显可能不能正常启用，请在登陆界面改为`X11`会话。

3. `mgpu-prime`会让`Xorg`运行在独显上，所以一旦设置生效，所有程序都会运行在独显上，无法让程序单独运行在集显上。

### 已知问题

- 如果安装N卡驱动后不能调整亮度，请看[这个教程](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45Njk2Ni5odG1s)。
- 界面刷新时，某些窗口内可能会呈现黑色方块，特别是用 Wine 安装的 Windows 程序更容易出现。
- 双显卡笔记本安装`mgpu-prime`软软件包后，即使不玩游戏N卡也会启动，比大黄蜂方式更耗电。

------

## 方法3：【中风险】从Debian反向移植软件源安装最新N卡驱动【N卡】

详见该帖：

[https://hu60.cn/q.php/bbs.topic.98302.html](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45ODMwMi5odG1s)

此外该教程还有高风险版本，但不推荐普通用户使用。除非上面的教程失败或者无法满足你的需求，采用这个：

[https://hu60.cn/q.php/bbs.topic.94160.html](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45NDE2MC5odG1s)

### 已知问题

- 如果安装N卡驱动后不能调整亮度，请看[这个教程](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45Njk2Ni5odG1s)。
- 界面刷新时，某些窗口内可能会呈现黑色方块，特别是用 Wine 安装的 Windows 程序更容易出现。
- 如果是i+N双显卡笔记本，即使不玩游戏N卡也会启动，比大黄蜂方式更耗电。

------

## 方法4：【不推荐】通过英伟达官网的”.run“文件进行安装【N卡】

去 [https://www.nvidia.cn/geforce/drivers/](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly93d3cubnZpZGlhLmNuL2dlZm9yY2UvZHJpdmVycy8.) 下载”Linux 64-bit“驱动，下载到的是".run"格式，比如”NVIDIA-Linux-x86_64-450.57.run“。

至于怎么安装我就不说了，这种方法安装很复杂，而且一但失败，电脑可以完全无法进入桌面。所以，仅建议了解Linux和N卡驱动的开发者去折腾。

### 已知问题

- 如果安装N卡驱动后不能调整亮度，请看[这个教程](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45Njk2Ni5odG1s)。

- 界面刷新时，某些窗口内可能会呈现黑色方块，特别是用 Wine 安装的 Windows 程序更容易出现。

- 如果是i+N双显卡笔记本，默认只有独显+外接显示器可以用，集显+笔记本自带屏幕不能用。笔记本自带屏幕会没有任何显示。需要修改配置才能正常使用笔记本自带屏幕，并且通常很难驱动集显。

- 你可能依然会看到以下提示：

  ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzRlNDE4NTVmMWVmZGQ2Y2IyYTE3NjE2Nzg1MWJhMDhmMzkwMTQucG5n)

  你可以尝试通过以下命令安装缺失的Vulkan支持库：

  ```sql
  sudo apt update
  sudo apt upgrade
  sudo  apt  install  libvulkan1  libvulkan1:i386  'vulkan-utils|vulkan-tools'
  ```

  如果安装后问题依然没有解决，那你可能需要**卸载**通过`.run`安装的驱动，然后用上面的**方法2**安装驱动。