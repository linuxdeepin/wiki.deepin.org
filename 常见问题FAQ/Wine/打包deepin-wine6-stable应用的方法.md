---
title: 打包deepin-wine6-stable应用的方法
description: 
published: true
date: 2022-06-20T08:59:30.229Z
tags: wine6 stable
editor: markdown
dateCreated: 2022-06-20T08:57:09.981Z
---

# wine使用教程  


## 第3辑：打包deepin-wine6-stable应用的方法  


本帖衔接上一帖《[wine使用教程2-用deepin-wine6安装/运行exe程序的方法](https://bbs.deepin.org/post/238765)》内容，将上一帖已安装的windows软件7-zip（版本21.7.0.0）打包为deb格式。

### 一、准备deb的基本信息

建议将以下1-6点信息写到一个txt文件中备用，比如像这样  


![1](https://storage.deepin.org/thread/202206110036522199_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220611003645.png)  


#### 1、容器名

建议格式：投稿平台名称-应用名称

案例：Spark-7zip

BOTTLENAME="Spark-7zip"  


#### 2、版本号

建议格式：该exe软件的windows版本号+投保平台名称+投稿版本

案例：21.7.0.0spark0 （其中21.7.0.0是该软件windows版本号，spark是指星火应用商店，0是指投稿的初始版本）

APPVER="21.7.0.0spark0"  


#### 3、exe的主执行程序路径

案例：c:/Program Files/7-Zip/7zFM.exe

EXEC_PATH="c:/Program Files/7-Zip/7zFM.exe"  


#### 4、包名（应用标识）

建议格式：软件官网网址倒着写，www替换为你要投稿的应用商店平台名称，深度应用商店为deepin，星火应用商店为spark

案例：org.7-zip.spark  


#### 5、图标Exce执行路径

格式："/opt/apps/包名/files/run.sh" -f %f

案例："/opt/apps/org.7-zip.spark/files/run.sh" -f %f  


#### 6、短述：一句话介绍你打包的软件

案例：7-Zip是一款高压缩比的压缩软件。  


### 二、创建空白文件夹

建议在下载文件下创建package文件夹；

在package文件夹创建**files、entries、build**三个文件夹  


![2](https://storage.deepin.org/thread/202206101931562540_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220610182846.png)  


### 三、制作桌面图标

#### 1、下载或提取图标文件

在网上下载该软件的图标文件或使用图标提取工具提取该软件的图标文件，通常格式是png、ico、svg。  


#### 2、将图标文件格式转为svg格式

在线转格式网址：https://convertio.co/zh/ico-svg/

或者用应用商店里的Inkscape另存为svg格式。

#### 3、重命名svg文件

将svg图标文件存入第二步的entries文件夹，并将svg文件重命名为：包名.svg

案例：org.7-zip.spark.svg

![3](https://storage.deepin.org/thread/202206101948069106_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220610194724.png)

### 四、压缩容器

#### 1、更改当前用户名

将目标容器C盘（即drive_c）下面的users文件下面当前用户名（也就是你自己的用户名）文件夹改为@current_user@

![4](https://storage.deepin.org/thread/202206102100202790_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220610210009.png)

#### 2、批量修改注册表里的当前用户名

在目标容器里右键——在终端中打开，终端输入：

sed -i "s#$USER#@current_user@#" ./*.reg

![5](https://storage.deepin.org/thread/202206102100373015_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220610183410.png)

#### 3、压缩目标容器为files.7z

在目标容器里右键——在终端中打开，终端输入：

7z a -t7z -r files.7z 容器路径/*

案例：

7z a -t7z -r files.7z ~/.deepinwine/Wine-7zip/*

![6](https://storage.deepin.org/thread/202206102101055541_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220610183626.png)

#### 4、将压缩好的files.7z移动到第二步的files文件夹

![7](https://storage.deepin.org/thread/20220610210137408_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220610183745.png)

### 五、压缩deepin-wine6-stable

由于deepin-wine6-stable是Deepin/UOS系统自带的，无需压缩deepin-wine6-stable。

### 六、制作执行程序run.sh

我们将wine版微信的run.sh文件和dlls文件夹复制到第二步的files文件夹。

注：wine版微信的run.sh文件和dlls文件夹的路径在：/opt/apps/com.qq.weixin.deepin/files/

![8](https://storage.deepin.org/thread/202206102103136831_image.png)

替换run.sh里面的内容：右键用文本编辑器打开run.sh，然后将第一步1-4的信息替换run.sh里面原有的信息。替换好后保存并退出run.sh。

![9](https://storage.deepin.org/thread/20220610193850456_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220610185655.png)

### 七、用UOS打包工具（Debreate）打包为deb包

没有安装UOS打包工具的小伙伴，可以去在Deepin/UOS应用商店或星火应用商店下载。

以下步骤需要用到本帖开头第一步的信息，需要把第一步的信息对号入座填进去。

#### 1、control

架构选择amd64或i386

![10](https://storage.deepin.org/thread/202206101939273300_%E6%88%AA%E5%9B%BE_init.py_20220610192420.png)

2、依赖和冲突
由于本教程exe是用deepin-wine6-stable运行，所以依赖项要填写deepin-wine6-stable和deepin-wine-helper。若其他linux系统上没有安装deepin-wine6-stable和deepin-wine-helper，是无法运行我们打包的deb包的。

可以参考我这个依赖项：

deepin-wine6-stable:amd64 (>= 6.0.0.19-1)

deepin-wine-helper (>= 5.1.30-1)

![11](https://storage.deepin.org/thread/20220610193945694_%E6%88%AA%E5%9B%BE_init.py_20220610191159.png)

#### 3、info文件

应用权限根据软件实际需要勾选

![12](https://storage.deepin.org/thread/202206101941115089_%E6%88%AA%E5%9B%BE_init.py_20220610191310.png)

#### 4、desktop文件

图标那里填写包名即可。

分类根据实际情况选择。

![13](https://storage.deepin.org/thread/202206101952005466_%E6%88%AA%E5%9B%BE_init.py_20220610195153.png)

#### 5、icon文件

添加第三步entries文件夹里的图标文件

![14](https://storage.deepin.org/thread/202206102001401528_%E6%88%AA%E5%9B%BE_init.py_20220610200114.png)

#### 6、files目录文件

添加第六步files文件夹里的files.7z、run.sh、dlls文件夹

![15](https://storage.deepin.org/thread/202206101942161211_%E6%88%AA%E5%9B%BE_init.py_20220610191835.png)

#### 7、添加脚本

添加一个可以在卸载该软件时自动删除它的容器的Post-Remove脚本:

```
#!/bin/bash
for username in `ls /home`  
    do
      echo /home/$username
        if [ -d /home/$username/.deepinwine/Spark-7zip ]  
        then
        rm -rf /home/$username/.deepinwine/Spark-7zip
        fi
```

![16](https://storage.deepin.org/thread/202206101942281445_%E6%88%AA%E5%9B%BE_init.py_20220610192302.png)

注：这里的Spark-7zip是案例软件的容器，若您的软件不一样，请替换为你自己在第一步第1点所设定的容器名。

#### 8、更新日志（非必须，可略过）

#### 9、版权文件（非必须，可略过）

#### 10、构建

点击中间绿色的开始构建图标，选择第二步的build文件夹就可以开始自动打包了。

![17](https://storage.deepin.org/thread/202206101940448511_%E6%88%AA%E5%9B%BE_init.py_20220610192544.png)

打包好后在build文件夹就可以看到deb文件了。

![18](https://storage.deepin.org/thread/202206101940568162_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220610192616.png)

打包好的deb文件可以直接在Deepin/UOS上双击安装。

![19](https://storage.deepin.org/thread/202206102005076258_%E6%88%AA%E5%9B%BE_deepin-deb-installer_20220610200333.png)

安装好后就可以在启动器里看到软件图标了。

![20](https://storage.deepin.org/thread/202206102005438047_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220610200409.png)



