---
title: UEngine使用总结
description: UEngine使用总结
published: true
date: 2022-09-01T13:50:30.040Z
tags: apk, uengine
editor: markdown
dateCreated: 2022-07-22T11:44:27.358Z
---

最新的 deepin 20.2.2 带来了可以运行安卓应用的uengine环境，如下图是安装在 uengine 环境上的应用：

![](https://storage.deepin.org/thread/202107121748565233_%E6%88%AA%E5%9B%BE%E5%BD%95%E5%B1%8F_20210712174835.png)

那么这些应用不是平白无故出现的吧，肯定是需要安装的，安装有许多方法，例如说下面这些方法来安装：

# 一、应用商店

打开![](https://storage.deepin.org/thread/202107121803032846_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20210712180244.png)，定位到“安卓应用”，就有很多 Android 应用可以安装

![](https://storage.deepin.org/thread/202107121804396197_%E6%88%AA%E5%9B%BE_deepin-home-appstore-client_20210712180430.png)

这里都应该知道怎么装了，不细讲

# 二、在终端安装

（这里需要的水平有提升，首先要知道终端是什么，这里不讲）

首先打开终端，可以用

```bash
apt search uengine
```

来获取所有的包名，但太多了，就可以通过

但可以通过这样

```bash
apt search uengine XXX
```

缩小寻找范围，如图：

![](https://storage.deepin.org/thread/202107121810449114_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20210712181012.png)

例如说安装QQ：

```bash
sudo apt install uengine.com.tencent.mobileqq
```

安装微信：

```bash
sudo apt install uengine.com.tencent.mm
```

以及 https://bbs.deepin.org/zh/post/222286 中的包名

当然还可以在 [https://home-store-packages.uniontech.com/appstore/pool/appstore/u/  ](https://home-store-packages.uniontech.com/appstore/pool/appstore/u/)中下载，安装方法看下面

# 三、使用其他人打包的 deb 安装

应用商店的包是 deb，使用 apt 的也是 deb，肯定也有人打包了 deb 供我们使用，例如说 https://bbs.chinauos.com/zh/post/7339 就有大佬打包的deb包，这里以 Microsoft Todo for Android 为例

### （一）使用图形化 deb 安装器安装

首先打开下载的 deb 目录，然后使用 deb 安装器打开，然后点击“安装”输入密码即可

![](https://storage.deepin.org/thread/202107121827222697_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20210712182701.png)

### （二）使用终端安装

**适用于上面的方法无法安装时使用**

首先使用 cd 目录或者使用文件管理器定位然后右键终端打开
然后使用 dpkg 命令进行安装，格式如下

```bash
sudo dpkg -i XXX  # 一定要用 root 权限运行，XXX是 deb 包的文件名
```

然后输入用户密码进行安装

![](https://storage.deepin.org/thread/20210712183044244_%E6%88%AA%E5%9B%BE_deepin-terminal_20210712183028.png)

但如果出现了依赖问题（我实在没图了），就输入

```bash
sudo apt install -f
```

修复其依赖关系

最后打开启动器运行即可

![](https://storage.deepin.org/thread/202107121838141403_%E6%88%AA%E5%9B%BE_20210712183748.png)

# 四、使用第三方软件安装 Android 应用

目前社区有两种安装器，第一种是我开发的运行器和打包器（运行器：https://gitee.com/gfdgd-xi/uengine-runner），还有就是 Maicss 大佬开发的 https://bbs.deepin.org/zh/post/223042 ，这里以我自己开发的 UEngine 运行器为例

## （一）安装

首先下载 deb 包安装，安装过程忽略
![image.png](https://storage.deepin.org/thread/202207230949553090_image.png)


## （二）安装自己的应用

下载自己需要的 APK 文件，右键=>安装/卸载 APK（UEngine 运行器） 打开
![image.png](https://storage.deepin.org/thread/202207230951225194_image.png)
然后点击安装按钮
![image.png](https://storage.deepin.org/thread/20220723095153563_image.png)
然后提示需要输入密码，输入密码继续安装
![image.png](https://storage.deepin.org/thread/202207230952242433_image.png)

当提示操作完成时，就可以打开启动器运行了
![image.png](https://storage.deepin.org/thread/202207230952534147_image.png)


# 五、使用命令安装 Android 应用

这个限制就比较少了,首先要有一个 APK,定位到 apk所在目录,然后输入

```bash
sudo /usr/bin/uengine-session-launch-helper -- uengine install --apk='XXX' 
# XXX是apk路径,如果是用pkexec调用root权限,请输入绝对路径,而非相对路径
# 注意:安装需要root权限,请注意!
```

**接下来就是些其他的了，毕竟是总结吗，还要其他的东西**

## 打成 deb 包给他人使用

在用第三方的安装器时，你会发现有一个打包成 deb 的功能，选择你自己想要打包的应用，点击打包就会把 deb 文件生成于桌面

![image.png](https://storage.deepin.org/thread/202207230948496413_image.png)

打包后的 deb 包就可以发给其他人使用了，安装方法如上面的第三点

## 打开程序列表

在终端输入以下命令即可

```bash
/usr/bin/uengine-launch.sh --package=org.anbox.appmgr --component=org.anbox.appmgr.AppViewActivity
```

或者创建一个 .desktop 文件，把以下内容写入也可以

```ini
[Desktop Entry]
Categories=System;
Comment=uengine 程序菜单
Encoding=UTF-8n
Exec=/usr/bin/uengine-launch.sh --package=org.anbox.appmgr --component=org.anbox.appmgr.AppViewActivity
Icon=anbox
MimeType=
Name=uengine 程序菜单
StartupWMClass=uengine 程序菜单
Terminal=false
Type=Application
```

## 与 uengine 互通文件

在系统的很多地方，如桌面

![](https://storage.deepin.org/thread/202107121906186491_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20210712190550.png)

文件管理

![](https://storage.deepin.org/thread/202107121908471904_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20210712190828.png)

uengine 右键

![](https://storage.deepin.org/thread/202107121908017196_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20210712190749.png)

都能看到它的身影，你可以通过它和 uengine 交换文件（怎么截不了图）

![](https://storage.deepin.org/thread/202107131024463784_%E6%88%AA%E5%9B%BE%E5%BD%95%E5%B1%8F_dde-file-manager_20210713102429.png)

**但注意它访问的不是根目录，如果需要访问请安装Android的第三方文件管理器**

## 卸载 Android 应用

### 1、右键卸载

有些通过 deb 或者 Maicss 大佬安装的都可以右键卸载，但有些不行，例如通过命令安装的以及用我的运行器安装的都不能用右键卸载，那么要用下面的方法

### 2、使用系统设置卸载

打开程序菜单或在终端输入

```bash
/usr/bin/uengine-launch.sh --action=android.intent.action.MAIN --package=com.android.settings --component=com.android.settings.Settings
```

打开系统设置，然后点击应用部分

![](https://storage.deepin.org/thread/202107131032133814_%E6%88%AA%E5%9B%BE%E5%BD%95%E5%B1%8F_20210713103155.png)

然后这里就有安装的应用列表

![](https://storage.deepin.org/thread/202107131035208041_%E6%88%AA%E5%9B%BE%E5%BD%95%E5%B1%8F_20210713103456.png)

然后点击进入你需要卸载的软件，然后点击卸载即可

![](https://storage.deepin.org/thread/202107131038015500_%E6%88%AA%E5%9B%BE%E5%BD%95%E5%B1%8F_20210713103745.png)

### 3、使用第三方程序卸载

其实就指的是我的运行器，安装方法和Maicss 大佬的安装方法一样，然后打开运行器，选择要卸载软件对应的apk包或对应的包名（包名的获取方法请看下一点），输入密码卸载即可

![](https://storage.deepin.org/thread/202107131042192305_%E6%88%AA%E5%9B%BE%E5%BD%95%E5%B1%8F_tk_20210713104103.png)

### 4、使用终端卸载

首先获取包名（需要有对应的 APK）（如果知道包名请忽略），首先安装 appt

```bash
sudo apt install aapt
```

然后定位到APK所在目录，输入

```bash
aapt dump badging XXX  # XXX为APK路径
```

获取 APK 信息，然后找到“package:”开头的那一行，找到“name”后面的那个包名

![](https://storage.deepin.org/thread/202107131047052002_%E6%88%AA%E5%9B%BE%E5%BD%95%E5%B1%8F_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20210713104632.png)

然后输入

```bash
sudo /usr/bin/uengine-session-launch-helper -- uengine uninstall --pkg='XXX'
# XXX 为包名
# 可以使用sudo或者pkexec，需要 root 权限卸载
```

即可

## 出现运行故障

部分 Android 软件是无法运行的，你可以去 anbox 的 Issues 去看看有没有解决方案，因为 uengine 是在 anbox 上二次开发

[项目链接：https://github.com/anbox/anbox/](https://github.com/anbox/anbox/)

最后，实在写不出来了，再让我想想……
