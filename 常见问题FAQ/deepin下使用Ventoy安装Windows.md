---
title: deepin下使用Ventoy安装Windows
description: 
published: true
date: 2022-06-27T06:11:55.908Z
tags: 
editor: markdown
dateCreated: 2022-06-15T03:24:14.525Z
---

# deepin下使用Ventoy安装Windows
本文主要讲述如何在deepin下制作Ventoy启动盘和用Ventoy启动盘安装Windows

## 一、Ventoy是什么？
Ventoy 是一个制作可启动U盘的开源工具。Ventoy能轻松实现多镜像启动，支持常见的操作系统和硬件类型。Ventoy强大的插件功能可以让你对它进行自定义、扩展启动盘功能。

## 二、Ventoy的下载与安装
### 从深度应用商店下载
直接在深度应用商店搜索ventoy下载软件即可安装，安装完成后即可点击启动器Ventoy软件图标启动程序。
### 从官网下载
访问[Ventoy官网](https://www.ventoy.net/cn/index.html)，进入Ventoy下载页，从说明中的下载链接选择一个进入(建议选蓝奏云或镜像站)，下载最新的ventoy-x.x.xx-linux.tar.gz压缩包。
![截图_选择区域_20220615103338.png](/截图_选择区域_20220615103338.png)
在文件管理器中下载文件夹内右键解压这个文件，打开解压后文件所在的文件夹，双击`VentoyGUI.x86_64`选择运行，即可启动Ventoy程序。
![2.png](/2.png)
或者在窗口内空白处右击，选择“在终端中打开”，并在终端输入以下命令：`./VentoyGUI.x86_64`，回车即可启动Ventoy程序。

## 三、制作Ventoy启动盘
官方使用说明：https://ventoy.net/cn/doc_start.html    这里介绍GTK/QT图形化界面的使用方式。
1. 准备一个8G或以上的U盘，制作启动盘会删除U盘内存储的数据，在制作前请将U盘中的所有文件复制到电脑上或备份到其他位置。
2. 启动Ventoy程序，Ventoy会自动识别电脑所插入的U盘（若电脑上有多个U盘或Ventoy识别错误，请自行手动选择）
3. 在“设备”处选择U盘，确认后，点击“安装”。
![3.png](/3.png)
4. 安装完成后，U盘在文件管理器中显示的名称为Ventoy（可改）。U盘被划分为两个分区，其中容量较小的分区为EFI分区，用于保存UEFI模式下的启动文件以及Ventoy的其他文件。容量较大的分区可以作为正常的U盘使用，可以存放Windows或其他系统的iso镜像文件，也可以存入一些常用的软件或文件。只需将需要安装的系统的镜像文件复制到较大的分区内（支持多个镜像文件），在安装系统时选择该U盘作为第一启动项，Ventoy默认会遍历所有的目录和子目录，找出所有的镜像文件，并按照字母排序之后显示在菜单中，从菜单中选择要安装的镜像文件即可进入安装。
### 提示
1. 若用这种方式安装Ventoy失败，可以尝试换另外2种方式（见本页 附录 4.1），再不行就换U盘、USB接口等。使用中有问题可阅读常见问题、文档手册或在论坛发帖。
2. 有能力的用户可以使用Vlnk实现启动本地硬盘中的镜像文件。请阅读文档手册，了解Ventoy的更多功能。
3. 使用启动盘（尤其是拷贝完文件）后，一定要先安全弹出再拔下U盘，否则可能使U盘文件系统损坏。如果没有在U盘中存储4GB以上大文件的需求（有些Windows 10/11镜像大小已超过4GB），可以先在任务栏上的托盘处（或文件管理器的侧边栏）卸载（弹出）U盘，再在文件管理器（或磁盘管理器）中选择U盘较大的分区，右击点击“格式化”，将U盘（较大的分区）文件系统格式化成FAT32，减少该问题的发生。
![4.png](/4.png)
4. Ventoy启动盘也可用于安装deepin等Linux发行版，可以参考我的deepin变形记或deepin常用资源整理 “2.1 系统安装” 部分的文章。运行内存比较小的电脑建议启用swap（交换）分区或交换空间。

## 四、附录
### 4.1 另外2种Ventoy启动盘制作方式
#### 4.1.1 使用Web UI模式(适用于1.0.48或者更高版本)
官方说明：https://ventoy.net/cn/doc_linux_webui.html
1. 进入[Ventoy官网](https://www.ventoy.net/cn/index.html)，点击网页顶栏上的下载，在下载页面里下载ventoy-x.x.xx-linux.tar.gz压缩文件。（推荐通过“说明”中的蓝奏云下载地址下载）
![7.png](/7.png)
2. 解压下载的ventoy-x.x.xx-linux.tar.gz文件，从文件管理器打开至解压后文件的目录下，右击空白处，点击“在终端中打开”。
![8.png](/8.png)
3. 在终端中输入`sudo sh ./VentoyWeb.sh`并回车执行，输入密码。在浏览器中输入网址`http://127.0.0.1:24680`并进入，此时会看到Ventoy的Web图形界面。
![9.png](/9.png)
![10.png](/10.png)
4. 准备一个大小为8G或以上的U盘，插入电脑，备份U盘中的所有文件到电脑上。点击“安装”，将Ventoy安装到U盘上。U盘将会被格式化，请务必确保U盘中的文件已做好备份。安装完成后，切换到终端，按Ctrl+C退出。
若上述方法使用时出现问题，请尝试下面的命令行模式。
#### 4.1.2  使用命令行模式
1. 进入[Ventoy官网](https://www.ventoy.net/cn/index.html)，点击网页顶栏上的下载，在下载页面里下载ventoy-x.x.xx-linux.tar.gz压缩文件。（推荐通过“说明”中的蓝奏云下载地址下载）
![11.png](/11.png)
2. 解压下载的ventoy-x.x.xx-linux.tar.gz文件，从文件管理器打开至解压后文件的目录下，右击文件管理器窗口的空白处，点击“在终端中打开”，打开终端。
![12.png](/12.png)
3. 点击文件管理器左边栏上的“计算机”，回到主界面。准备一个大小为8G或以上的U盘，插入电脑，备份U盘中所有的文件。右击文件管理器中显示的U盘，点击属性，记住显示在下图中蓝色方框位置的标识。（我的是**sdd1**，标识因电脑上的硬盘数量和顺序等因素而异，请以自己的实际情况为准）
![14.png](/14.png)
4. 打开ventoy官网的[使用说明](https://www.ventoy.net/cn/doc_start.html)，往下翻到**Linux系统安装Ventoy**部分。使用官网提供的命令`sh Ventoy2Disk.sh -i /dev/XXX`来制作启动盘。具体的来说，首先，这条命令必须以root权限执行，因此命令前要加上`sudo`，其次，`/dev/XXX`表示你选择的U盘，XXX的内容就是上图蓝色方框框选的部分，以我的U盘为例，制作启动盘的命令为`sudo Ventoy2Disk.sh -i /dev/sdd`(sdd是U盘的代号，数字1表示该U盘的第一个分区，所以只要输入字母部分即可）
![15.png](/15.png)
5. 回车执行命令。先输入系统用户的密码给予root权限，然后务必确保U盘中没有重要文件，两次输入y继续操作，之后U盘将会被格式化，开始制作。
![16.png](/16.png)
![17.png](/17.png)
6. 当显示安装成功完成后，关闭终端，回到文件管理器，此时你会看见U盘的名称被修改为“Ventoy”（你也可以通过磁盘管理器更改其名称）。一个Ventoy U盘就制作好了，你可以将系统的iso镜像放在U盘中做启动盘（支持多个iso文件），也可以与此同时用U盘存储其他文件，不会影响启动盘的作用。

### 4.2 常用的Windows系统镜像下载地址
#### 4.2.1 [MSDN](https://msdn.itellyou.cn/)
网络上知名度最高的Windows系统镜像站，但迫于经费，目前网站已停更，最新版更新到Windows10 1903
#### 4.2.2 [Next, I tell you](https://next.itellyou.cn/)
MSDN网站作者的新网站，下载系统镜像需要注册登录，并观看一小段广告，以维持该网站的开支。值得一提的是，除了Windows，该网站也提供了其他操作系统如UOS的镜像
#### 4.2.3 [Hello Windows](https://hellowindows.cn/)
该网站托管在GitHub上，永久在线，值得称赞的是，除了镜像文件外，该网站贴心的提供了下载工具、系统激活工具以及可直装使用的Office办公软件
#### 4.2.4 [果核剥壳](https://www.ghxi.com/)
一些软件推荐站会提供一些精简优化的Windows镜像，如果你的电脑比较老旧的话不妨一试哦（下载镜像一定要认清来源，一些来源不明的镜像中可能含有潜藏的病毒或者流氓广告软件）
> 附注：为保证这些网站能够长久的运行下去，请尽量不要使用迅雷下载，推荐使用qbittorrent、transmission、deluge等bt下载工具