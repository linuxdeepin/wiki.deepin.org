---
title: wine使用教程7-借用容器和wine版本安装MicrosoftOffice2013
description: 
published: true
date: 2022-11-15T08:38:58.149Z
tags: office20113 wine
editor: markdown
dateCreated: 2022-11-15T08:04:53.630Z
---

# wine使用教程7-wine版本安装Office2013

https://bbs.deepin.org/post/239589

## wine使用教程
### 第7辑：在Deepin/UOS家庭版借用容器和wine版本安装Microsoft Office2013的方法

之前给大家介绍过如何利用deepin-wine6-stable安装Microsoft Office 2013的方法（详见教程第6辑）。经测试，此方法安装的Microsoft Office 2013无法连接服务器，以致于无法输入激活密钥

![2022-11-15_66620.png](/2022-11-15_66620.png)


就此，楼主换一种思路，借用星火商店战网客户端的容器和wine版本来安装Microsoft Office 2013。经测试，可以连接服务器了。方法如下：

说明：利用第6辑和第7辑方法安装的Microsoft Office 2013的PowerPoint和OneNote无法使用，暂未找到解决方法。

### 一、下载Microsoft Office2013安装镜像并解压
以下教学所用Microsoft Office2013安装镜像（cn_office_professional_plus_2013_x86_x64_dvd_1149708.iso）从MSDN网站下载。


![2022-11-15_30808.png](/2022-11-15_30808.png)


Microsoft Office2013安装镜像iso文件放在下载文件夹（~/Downloads）

右键解压

![2022-11-15_12477.png](/2022-11-15_12477.png)


### 二、安装星火应用商店战网客户端并首次运行

安装星火商店里的战网客户端，安装好后一定要双击战网的图标运行一次（直到出现战网客户端账号登录界面，就可以关闭客户端了）。首次运行将建立战网客户端的容器（Deepin-Battlenet文件夹）以及wine版本（Lwine7.1文件夹）。

![2022-11-15_99347.png](/2022-11-15_99347.png)

### 三、复制容器和wine版本并改名
（1）复制Deepin-Battlenet容器并改名Spark-Office

（2）复制Lwine7.1并改名为Lwine7.1-my

![2022-11-15_55798.png](/2022-11-15_55798.png)

### 四、设置容器Spark-Office的windows版本
终端命令：

WINEPREFIX=~/.deepinwine/Spark-Office ~/.deepinwine/Lwine7.1-my/bin/winecfg
在弹出的wine设置窗口，将windows版本设置为windows7


![2022-11-15_94484.png](/2022-11-15_94484.png)

### 五、安装Gecko
终端命令：

32位Gecko：

WINEPREFIX=~/.deepinwine/Spark-Office ~/.deepinwine/Lwine7.1-my/bin/wine msiexec /i ~/.deepinwine/Lwine7.1-my/gecko/wine-gecko-2.47.2-x86.msi

![2022-11-15_7233.png](/2022-11-15_7233.png)

64位gecko：

WINEPREFIX=~/.deepinwine/Spark-Office ~/.deepinwine/Lwine7.1-my/bin/wine msiexec /i ~/.deepinwine/Lwine7.1-my/gecko/wine-gecko-2.47.2-x86_64.msi

![2022-11-15_55924.png](/2022-11-15_55924.png)

### 六、安装mono
终端命令：

WINEPREFIX=~/.deepinwine/Spark-Office ~/.deepinwine/Lwine7.1-my/bin/wine msiexec /i ~/.deepinwine/Lwine7.1-my/mono/wine-mono-7.1.1-x86.msi

![2022-11-15_26174.png](/2022-11-15_26174.png)

### 七、安装Microsoft Office 2013
终端命令：

WINEPREFIX=~/.deepinwine/Spark-Office ~/.deepinwine/Lwine7.1-my/bin/wine ~/Downloads/cn_office_professional_plus_2013_x86_x64_dvd_1149708/setup.exe

上述命令结构解析：

（1）字段1：WINEPREFIX=是指定的容器路径

（2）字段2：~/.deepinwine/Lwine7.1-my/bin/wine是你所调用的wine的路径

（3）字段3：最后接你要运行的exe程序的路径

注意：不同字段之间有一个空格（英文输入法）。

弹出Office的安装引导界面后，按提示操作安装即可。

![2022-11-15_53477.png](/2022-11-15_53477.png)


![2022-11-15_62606.png](/2022-11-15_62606.png)

![2022-11-15_36891.png](/2022-11-15_36891.png)

### 八、测试运行
终端命令：

WINEPREFIX=~/.deepinwine/Spark-Office ~/.deepinwine/Lwine7.1-my/bin/wine "c:/Program Files (x86)/Microsoft Office/Office15/WINWORD.EXE"

上述命令结构解析：

（1）字段1：WINEPREFIX=是指定的容器路径

（2）字段2：~/.deepinwine/Lwine7.1-my/bin/wine是你所调用的wine的路径

（3）字段3：最后接英文双引号，双引号内是你要运行的exe程序在容器drive_c（即模拟的c盘）中的路径，这里测试的Word的路径


成功连接服务器，并提示激活Office。（某宝上5块一个激活码）

### ![2022-11-15_29262.png](/2022-11-15_29262.png)


### 九、制作桌面图标
以Access的图标为例，在桌面新建一个txt文件，命名为MSACCESS.txt，复制以下内容到txt文件里：

[Desktop Entry]
Categories=Application
Exec=sh -c 'WINEPREFIX=/home/$USER/.deepinwine/Spark-Office /home/$USER/.deepinwine/Lwine7.1-my/bin/wine "c:/Program Files (x86)/Microsoft Office/Office15/MSACCESS.EXE"'
Icon=MSACCESS
MimeType=
Name=Access
StartupNotify=true
Type=Application
X-Deepin-Vendor=user-custom

保存退出txt，右键重命名，把这个txt文件的后缀改为desktop，最终文件名为：MSACCESS.desktop

![2022-11-15_99413.png](/2022-11-15_99413.png)

注：

Exec= ————sh -c '字段1：用WINEPREFIX指定容器路径 字段2：wine的路径 "字段3：exe程序在虚拟C盘里的路径"' 注意这里有一个单引号和一个双引号（英文输入法）。

Icon= ————指图标路径，如果图标在/usr/share/icons/hicolor/scalable/apps文件夹，就不用写完整路径，只需要写图标文件的文件名（不写文件后缀）。楼主把svg图标已经制作好了，你可以直接下载使用office图标.zip。下载解压，然后里面的6个svg图标复制到/usr/share/icons/hicolor/scalable/apps下。（注意，需要在apps文件夹里右键——以管理员身份打开）

Name= ————图标文件显示的名称，这里填Access

特别说明，Exec=后面不能用~来代替/home/$USER

Word、Excel、PowerPoint等的图标制作方法一样，就不一一介绍了，内容如下：


#### Word：
```
[Desktop Entry]
Categories=Application
Exec=sh -c 'WINEPREFIX=/home/$USER/.deepinwine/Spark-Office /home/$USER/.deepinwine/Lwine7.1-my/bin/wine "c:/Program Files (x86)/Microsoft Office/Office15/WINWORD.EXE"'
Icon=MSWORD
MimeType=
Name=Word
StartupNotify=true
Type=Application
X-Deepin-Vendor=user-custom
```

#### Excel：
```
[Desktop Entry]
Categories=Application
Exec=sh -c 'WINEPREFIX=/home/$USER/.deepinwine/Spark-Office /home/$USER/.deepinwine/Lwine7.1-my/bin/wine "c:/Program Files (x86)/Microsoft Office/Office15/EXCEL.EXE"'
Icon=MSEXCEL
MimeType=
Name=EXCEL
StartupNotify=true
Type=Application
X-Deepin-Vendor=user-custom
```

#### Outlook：
```
[Desktop Entry]
Categories=Application
Exec=sh -c 'WINEPREFIX=/home/$USER/.deepinwine/Spark-Office /home/$USER/.deepinwine/Lwine7.1-my/bin/wine "c:/Program Files (x86)/Microsoft Office/Office15/OUTLOOK.EXE"'
Icon=MSOUTLOOK
MimeType=
Name=Outlook
StartupNotify=true
Type=Application
X-Deepin-Vendor=user-custom
```

#### POWERPNT：
```
[Desktop Entry]
Categories=Application
Exec=sh -c 'WINEPREFIX=/home/$USER/.deepinwine/Spark-Office /home/$USER/.deepinwine/Lwine7.1-my/bin/wine "c:/Program Files (x86)/Microsoft Office/Office15/POWERPNT.EXE"'
Icon=MSPOWERPNT
MimeType=
Name=POWERPNT
StartupNotify=true
Type=Application
X-Deepin-Vendor=user-custom
```

#### OnoNote：
```
[Desktop Entry]
Categories=Application
Exec=sh -c 'WINEPREFIX=/home/$USER/.deepinwine/Spark-Office /home/$USER/.deepinwine/Lwine7.1-my/bin/wine "c:/Program Files (x86)/Microsoft Office/Office15/ONENOTE.EXE"'
Icon=MSONENOTE
MimeType=
Name=OneNote
StartupNotify=true
Type=Application
X-Deepin-Vendor=user-custom
```

图标效果如下：

![2022-11-15_72076.png](/2022-11-15_72076.png)

### 十、双击运行桌面图标测试一下
成功运行

![2022-11-15_16233.png](/2022-11-15_16233.png)


### 十一、字体问题
为了一劳永逸解决wine应用字体显示乱码、方块、显示不出等问题，建议你安装星火应用商店里的“Win字体”

### 十二、收尾工作——清理Spark-Office里战网客户端的文件夹
由于Spark-Office这个容器我们是复制的战网客户端的Deepin-Battlenet容器，里面有战网客户端的一些文件夹。为节省磁盘空间，我们可以把Spark-Office容器里与战网客户端有关的文件夹删掉。

![2022-11-15_5230.png](/2022-11-15_5230.png)
![2022-11-15_39315.png](/2022-11-15_39315.png)
![2022-11-15_1187.png](/2022-11-15_1187.png)
![2022-11-15_28445.png](/2022-11-15_28445.png)
![2022-11-15_10394.png](/2022-11-15_10394.png)
![2022-11-15_87089.png](/2022-11-15_87089.png)

### ————补充————
使用过程中，doc文件可以采用拖入Word中打开，或者在Word界面打开文件的方式打开，不要在doc文件右键选择Microsoft Word打开。如果在doc文件右键选择Microsoft Word打开，所调用的wine版本将不再是Lwine7.1-my/bin/wine，可能是调用的原生wine（也有可能是deepin-wine，究竟是哪一个我不太清楚），这会破坏Spark-Office容器环境。

Excel、Access等同理。


2022-7-3更新
PowerPoint、OneNote无法运行的问题已解决，详见以下两篇帖子：

https://bbs.deepin.org/post/239888

https://bbs.deepin.org/post/239886

补充：经使用发现，用wine运行MS Office后，会有残留后台进程。所以建议大家在退出MS Office的程序后，去系统监视器强制关闭MS Office有关的进程。



































