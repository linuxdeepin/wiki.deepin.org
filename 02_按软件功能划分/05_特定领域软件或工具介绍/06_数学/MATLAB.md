---
title: MATLAB
description: 一款科学计算的常用软件
published: true
date: 2022-07-14T09:30:30.983Z
tags: 科学计算
editor: markdown
dateCreated: 2022-07-13T16:14:40.521Z
---

## 简介
MATLAB是一个编程和数值计算平台，数百万工程师和科学家用MATLAB来分析数据、开发算法和创建模型。

## 安装
进入 MATLAB 官网 https://ww2.mathworks.cn/products/matlab.html
用高校的 EDU 邮箱注册 MathWorks 账户、获取许可证后，点击“获取 MATLAB“

![matlab1.png](/matlab1.png)

然后点击安装 MATLAB

![matlab2.png](/matlab2.png)

点击下载 Linux 版本

![matlab3.png](/matlab3.png)

下载完成后，**在压缩包所在的文件夹，右击->在终端中打开**
执行下面命令解压：
```
unzip -X -K matlab_R2022a_glnxa64.zip
```
（这里用的是R2022a版本，所以文件名是matlab_R2022a_glnxa64.zip。请根据实际情况修改文件名）
**注意：不要右击解压压缩包！！否则可能会遇到无法安装的问题**
等待解压完成后，执行下面命令安装：
```
./install
```
打开 MATLAB 安装程序后，登录 MathWorks 账户，接受协议。
选择许可证，点击下一步。

![matlab5.png](/matlab5.png)

默认安装目录会提示无权限安装，此时需要修改安装目录
如果遇此情况，建议安装到```~/MATLAB/2022a/```

![matlab6.png](/matlab6.png)

点击下一步，然后选择需要的产品进行安装。
等待安装完成之后，按```Ctrl+Alt+T```打开终端，执行
```
~/MATLAB/R2022a/bin/matlab
```
即可打开 MATLAB。

## 卸载
删除 MATLAB 安装文件夹即可。
如果你是安装在 ~/MATLAB/2022a，执行下面命令即可：
```
rm -rf ~/MATLAB/2022a
```

## 常见问题
### 1.如何创建 MATLAB 桌面入口（类似 Windows 的快捷方式）？
在桌面上右击->新建文档->文本文档，将文件名改为 MATLAB.desktop 并打开
将下面内容粘贴到文档内，并保存：
```
#!/usr/bin/env xdg-open
[Desktop Entry]
Version=R2022a
Type=Application
Terminal=false
MimeType=text/x-matlab
Exec=/home/你的用户名/MATLAB/R2022a/bin/matlab -desktop
Name=MATLAB
Icon=matlab
Categories=Development;Math;Science
Comment=Scientific computing environment
StartupNotify=true
```
**注意：需要将“你的用户名”替换成自己账户的用户名。可通过在终端执行```whoami```查看当前用户名。**
### 2.在高分辨率屏幕上，文字显示太小
打开MATLAB后，**在MATLAB的命令行窗口**，执行如下命令：
```
s = settings;s.matlab.desktop.DisplayScaleFactor
s.matlab.desktop.DisplayScaleFactor.PersonalValue = 2
```
然后重启MATLAB即可解决。
### 3.字体渲染不正常
在终端执行下面命令安装openjdk：
```
sudo apt install openjdk-8-jre
```
然后执行下面命令，切换到Root用户：
```
sudo -i
```
修改全局环境变量：
```
echo export MATLAB_JAVA=/usr/lib/jvm/java-8-openjdk-amd64/jre >> /etc/profile
```
然后重启电脑。
## 相关链接
维基百科：https://zh.wikipedia.org/zh-cn/MATLAB
MATLAB 官方网站：https://ww2.mathworks.cn/products/matlab.html
ArchWiki MATLAB 页面：https://wiki.archlinux.org/title/MATLAB_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)