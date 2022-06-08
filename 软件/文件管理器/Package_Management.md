---
title: Package_Management
description: 
published: true
date: 2022-05-07T07:48:25.014Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:39:41.051Z
---

# **Main tools**

- Apt - The main package management tools
- aptitude - CLI and ncurses front end for Apt (recommended)
- Synaptic - Graphical front end for Apt (recommended)
- apt-get - CLI Apt package handling utility
- dpkg – Debian package file installer

## **dpkg**

dpkg is a medium-level package manager for Debian. It's also used in Deepin Operating System.

### Functions

- Install or manage a single software package
- Upgrade software suite
- Install the application security patch

### Why "medium-level"?

- There's dpkg-deb, which performs lower-level actions on .deb (binary) packages, such as reading the control information and directly extracting the contained files.
- dpkg checks dependencies and will refuse to install a package whose dependencies aren't met, but it won't help you find and install those dependencies. You need a higher-level tool (e.g. aptitude, apt-get or dselect) for that.

## **APT**

It's a set of tools for managing Debian packages, and therefore the applications installed on your system.

### **Functions**

- Install applications
- Remove applications
- Keep your applications up to date
- and much more...

APT resolves dependency problems and retrieves requested packages from designated package repositories. APT delegates the actual installation and removal of packages to dpkg. APT is primarily used by commandline tools, but there are many GUI tools to let you use APT without having to touch the command line.
tools like aptitude can interact with APT via both commandline and TUI interfaces.

# **Commands**

## dpkg Commands

dpkg命令一般需要root权限执行，所以一般跟着sudo命令 例如: sudo dpkg xxxx
格式：
    dpkg [<选项> ...] <命令>
    dpkg -s package | grep Status  ##查询软件包是否安装
    dpkg -s package    ##查看软件包是否安装，获取其他有用信息
    dpkg -S filesname ##查找文件属于哪个安装包
    dpkg -C   ##搜索损坏的软件包
    dpkg -i <package.deb>   ##安装软件包
    dpkg -r package    ##删除已安装的软件包，但保留配置文件
    dpkg -P package   ##删除已安装软件包,完全清除包（含配置文件）
    dpkg -h   ##获取更多关于dpkg用法的信息
    dpkg -S file ##这个文件属于哪个已安装软件包。
    dpkg -L package ##列出软件包中的所有文件。
    dpkg –force-all –purge packagename ##有些软件很难卸载，而且还阻止了别的软件的应用，就可以用这个，不过有点冒险。

## apt commands

    apt-get:智能的解决软件包的依赖关系，方便软件的安装和升级（最新版只用apt，去掉了-get）
    apt-cache:查询apt的二进制软件包缓存文件
    apt-file:软件包查找工具，可以查到软件包所含的文件和安装的位置
    apt-get/apt命令[编辑]
    apt-get/apt命令一般需要root权限执行，所以一般跟着sudo命令 例如: sudo apt-get xxxx 格式：
    apt-get在最新版中已更新为apt
    sudo apt-get [options1]  [options2]
    apt-get update ##在修改/etc/apt/sources.list或者/etc/apt/preferences之後运行该命令。此外您需要定期运行这一命令以确保您的软件包列表是最新的
    apt-get install package_name ##安装一个新软件包
    apt-get remove package_name  ##卸载一个已安装的软件包（保留配置文件）
    apt-get --purge remove package_name ##卸载一个已安装的软件包（删除配置文件）
    apt-get autoclean  ##删除已卸载软件的软件包
    apt-get clean ##这个命令会把安装的软件的备份也删除，不过这样不会影响软件的使用的。
    apt-get upgrade ##更新所有已安装的软件包
    apt-get dist-upgrade ##将系统升级到新版本
    apt-get autoclean ##清除那些已经卸载的软件包的.deb文件
    apt-get autoremove ##删除某个包的同时，删除依赖于它的包

## apt-cache命令

    apt-cache show package_name ##显示软件包记录，类似于dpkg –print-avail。
    apt-cache search package_name ##在软件包列表中搜索字符串
    apt-cache showpkg package_name ##显示软件包信息。
    apt-cache policy package_name  ##显示软件包的安装状态和版本信息。
    apt-cache depends package_name  ##显示指定软件包所依赖的软件包。
    apt-cache rdepends package_name  ##显示软件包的反向依赖关系，即有什么软件包需依赖你所指定的软件包。
    apt-cache dumpavail package_name  ##打印可用软件包列表。
    apt-cache pkgnames package_name  ##打印软件包列表中所有软件包的名称。

## apt-file命令

    apt install apt-file  ##安装apt-file命令。
    apt-file update  ##更新软件包的文件库，第一次使用或apt-get update后都需运行一次。
    apt-file search  package_name ## 查找包含特定文件的软件包（不一定是已安装的），这些文件的文件名中含有指定的字符串。  
    apt-file list package_name  ##显示该软件包的文件。

## aptitude命令

aptitude 与 apt-get 一样，是 Debian 及其衍生系统中功能极其强大的包管理工具。
与 apt-get 不同的是，aptitude 在处理依赖问题上更佳一些。举例来说，aptitude 在删除一个包时，会同时删除本身所依赖的包。这样，系统中不会残留无用的包，整个系统更为干净。
aptitude命令一般也需要root权限执行，所以一般跟着sudo命令 例如: sudo aptitude xxxx
格式：
    aptitude [-S 文件名] [-u|-i]
    aptitude [选项] <动作> ...动作
    (如果未指定，aptitude 将进入交互模式)：
    aptitude install <pkgname>   ##安装/升级软件包
    aptitude remove <pkgname>   ##卸载软件包
    aptitude purge <pkgname>    ##卸载软件包并删除其配置文件
    aptitude forbid-version   ##禁止 aptitude 升级到某一特定版本的软件包
    aptitude update  ##下载新/可升级软件包列表
    aptitude safe-upgrade   ##执行一次安全的升级
    aptitude full-upgrade    ##执行升级，可能会安装和卸载软件包
    aptitude search  ##按名称 和/或 表达式搜索软件包
    aptitude show   ##显示一个软件包的详细信息
    aptitude clean  ##删除已下载的软件包文件
    aptitude autoclean  ##删除旧的已下载软件包文件
    aptitude download   ##下载软件包的 .deb 文件
    aptitude reinstall    ##下载并(可能)重新安装一个现在已经安装了的软件包
    aptitude --help   ##查看更多关于aptitue的用法

## dselcet命令

dselcet使用频率比较少,在此不多介绍,但是可以阅读：dselect用法小结
若系统没有，则可以使用下面命令安装：
    sudo apt-get install dselect
相关知识

删除APT缓存

我们使用apt-get安装软件，会自动到软件源服务器下载需要的文件，这些文件存放与/var/cache/apt/archives 目录，如果您不需要可以将其删除，方法为终端执行：
    sudo apt-get clean
或者
    sudo apt-get autoclean
区别

    apt-get clean删除/var/cache/apt/archives/ 和 /var/cache/apt/archives/partial/目录下所有包(锁定的软件包除外)
    apt-get autoclean仅删除/var/cache/apt/archives/ 和 /var/cache/apt/archives/partial/目录下旧版本的软件包和无用的软件包(锁定的软件包除外)
实例

在/var/cache/apt/archives/ 文件夹下我们有gimp2.3和gimp2.6版本的两个deb包、chrome28版本的deb包、firefox15版本的deb包(没有锁定的包),我们的系统当前安装了gimp的2.6版本和chrome28版本,并且没有安装firefox软件.
运行sudo apt-get clean,将删除gimp2.3和gimp2.6版本的两个deb包、chrome28版本的deb包、firefox15版本的deb包(也就是全部删除)
运行sudo apt-get autoclean,将删除gimp2.3版本的deb包和firefox15版本的deb包(只删除老旧的软件包和系统当前不需要的包)
删除软件配置[编辑]

当我们需要重置一个软件的设置或者彻底卸载该软件（软件+配置）删除该软件对于的配置文件即可. 配置文件存放的位置有： 1.用户设置(特定用户配置) ～/ ～/.config
2.全局设置(系统全局配置)
    /usr/share
注释：～/为您的主文件夹.
实例

    /home/cxbii/.config/deepin-music-player  ##这个路径的配置文件只对cxbii用户有效(即特定用户配置)
    /usr/share/deepin-music-player  ##这个路径的配置文件对该系统下所有用户都有效(即系统全局配置)
如果我们在删除一个软件包的同时需要删除该软件包的配置,可以加上--purge参数,例如:
sudo apt-get remove --purge package_name
卸载软件包

如果我们需要删除特定的软件包,可以终端执行:
    sudo apt-get remove  package_name
如果需要清除无关的依赖包,可以终端执行:
    sudo apt-get autoremove
此命令可以删除系统内部不需要的依赖包,但是此操作有一定的危险性,可能误删需要的依赖包,如果无特殊需要,请不要执行该操作.
实例

    apt-get remove为删除某个包的同时，删除依赖于它的包
例如： A 依赖于 B, B 依赖于 C apt-get remove 删除B的同时，将删除A(A依赖于B，B被删了，A也就无法正常运行了)
    apt-get autoremove的行为重点是卸载所有自动安装
例如：C 依赖于 B, D 依赖于B, 且D没有被其他手动安装的包依赖
    apt-get remove C 将删除C, 同时提示你用apt-get autoremove去清除B,D apt-get autoremove C 将删除B, C, D
    aptitude remove C 将删除B, C, D(删除C, 那么B,D 这两个包既是自动安装的,且没有其他手动安装的包依赖于它们, 则可以判定B,D也是没必要的)
升级软件包[编辑]

升级系统分下面几个步骤:
第一步，获得最近的软件包的列表(刷新源)；列表中包含一些包的信息，比如这个包是否更新过。
第二步，如果这个包没有发布更新，就不管它；如果发布了更新，就把包下载到电脑上，并安装。
对应到命令操作,我们可以终端执行:
    sudo apt-get update ##刷新软件源
然后选择执行一下任一命令:
    sudo apt-get upgrade  ##更新软件包
    sudo apt-get dist-upgrade  ##更新系统
    upgrade和 dist-upgrade的区别:
相同点

无论是使用哪一种方式更新,都需要要运行update更新。
不同点

upgrade只是简单的更新包，不管这些依赖，它不能添加新的软件包，或是删除软件包。并且不更改相应软件的的配置文件(包括内核)
dist-upgrade可以根据依赖关系的变化，添加软件包，删除软件包,并且更改相应软件的配置文件(包括内核)
适用范围

upgrade适合发行版本正常的软件更新和安装补丁更新.
dist-upgrade适合发行版从测试版等非正式版更新至正式版,或者跨越大版本进行更新(例如深度操作系统11.12更新至深度操作系统12.06).
添加PPA

安装支持“add-apt-repository”命令：
sudo apt-get install python-software-properties
sudo apt-get install software-properties-common
sudo apt-get update
常见问题

虽然apt-get是很智能的包管理器,但是不可避免的也会出现一些问题,因此本页面集中收集最为常见的错误与解决方法
问题一

终端出现:
    E:Sub-process /usr/bin/dpkg returned an error code (1)
解决方法,终端执行：
    cd /var/lib/dpkg
    sudo mv info info.bak
    sudo mkdir info
    sudo dpkg --configure -a
    sudo apt-get install -f
    sudo mv /var/lib/dpkg/info/* /var/lib/dpkg/info.bak
    sudo rm -rf /var/lib/dpkg/info
    sudo mv /var/lib/dpkg/info.bak /var/lib/dpkg/info
问题二

使用apt-get命令安装软件时，终端提示：
    E: 无法获得锁 /var/lib/dpkg/lock - open (11: 资源暂时不可用)
    E: 无法锁定管理目录(/var/lib/dpkg/)，是否有其他进程正占用它？
解决方法如下:
方法一
已经打开的包管理程序（例如：apt-get 或 aptitude）在运行，请先关掉它。 如果不知道是哪个程序，打开终端查看与apt-get有关的程序,sudo kill 前面的数字。或者 可以重启电脑
方法二
打开终端,依次执行下面的命令：
    sudo rm /var/cache/apt/archives/lock
    sudo rm /var/lib/dpkg/lock sudo rm /var/lib/apt/lists/lock
注意：方法二适用方法一无效的时候。
问题三

使用apt-get刷新源,终端出现:
    E: Some index files failed to download. They have been ignored, or old ones used instead.
解决方法如下:
方法一
如果是因为修改了官方默认源.恢复默认源即可 深度操作系统默认源
方法二
可能是服务器出问题,请等待一段时间后再次刷新本地源列表,如果依然不行,尝试终端执行:
    sudo rm /var/lib/apt/lists/partial/*
    sudo apt-get update
问题四[编辑]
使用apt-get刷新源,终端提示: W: GPG error: <http://apt.tt-solutions.com> dapper Release: 由于没有公钥，下列签名无法进行验证： NO_PUBKEY 06EA41DE4F6C1E86
解决方法,终端执行:
    gpg --keyserver subkeys.pgp.net --recv 4F6C1E86
    gpg --export --armor 4F6C1E86 | sudo apt-key add -
说明： 若缺少其他公钥，则将命令中两处4F6C1E86改为NO_PUBKEY 06EA41DE4F6C1E86中最后8位即可！
如果是PPA源,则执行:
    sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com
问题五

使用apt-get安装软件,终端提示:
    E: dpkg 被中断,您必须手工运行 sudo dpkg --configure -a解决此问题。
解决方法,按照提示提示.终端执行:
    sudo dpkg --configure -a
如果依然不行则执行：
    sudo rm /var/lib/dpkg/updates/*
    sudo apt-get update
    sudo apt-get upgrade
问题六

终端提示：
    E: Unable to correct problems, you have held broken packages.
出现此问题一般是依赖出现问题，尝试终端执行：
    sudo apt-get install -f
如果无效则执行：
    sudo dpkg--configure -a
或者可以按照终端的完整提示删除导致依赖出现问题的软件包，终端执行：
    sudo apt-get remove xxx ##xxx为导致依赖出现问题的软件包名
然后终端执行：
    sudo apt-get update
问题七

终端出现: 'E:Encountered a section with no Package: header, E:Problem with MergeList /var/lib/apt/lists/archive.canonical.com_dists_maverick_partner_binary-i386_Packages, E:无法解析或打开软件包的列表或是状态文件。'
解决方法,终端执行：
    sudo rm -rf /var/lib/apt/lists/* -vf
    sudo apt-get update
问题八

为什么不能同时安装一个以上的软件
首要原因是深度操作系统使用DPKG包管理,统一由DPKG安装软件.(源代码编译软件为例外),并且Linux下的软件有软件依赖这一特殊性,如果同时安装一个以上的软件会让DPKG无法安全的记录软件的依赖包和主程序情况
如果同时运行两个或者以上的DPKG包管理则会出现: 无法锁定管理目录,并且导致软件依赖出现问题,因此只能一个个的安装软件.
注释:深度操作系统使用DPKG包管理,因此此文只适合DPKG包管理的Linux发行版本.
问题九

降级软件包
在一些时候，我们需要更低版本而非最近版本的软件，而软件包管理器却已为我们升至最新版本，这时，我们就需要降级某个软件包。
下面以降级 Firefox 为例，说明一下如何降级软件包。在深度操作系统中，Firefox 已升级至 16.0.x 版本，而我们需要更低版本来实现对于某些扩展的兼容。
首先，我们可以使用下面的命令查看一下软件仓库中有哪些可用的 Firefox 版本：
    apt-cache madison firefox
得到的输出结果如下：
    firefox | 15.0.1+build1-0ubuntu0.12.04.1 | <http://packages.linuxdeepin.com/ubuntu/> precise-security/main i386 Packages
    firefox | 15.0.1+build1-0ubuntu0.12.04.1 | <http://packages.linuxdeepin.com/ubuntu/> precise-updates/main i386 Packages
    firefox | 11.0+build1-0ubuntu4 | <http://packages.linuxdeepin.com/ubuntu/> precise/main i386 Packages
    firefox | 11.0+build1-0ubuntu4 | <http://packages.linuxdeepin.com/ubuntu/> precise/main Sources
    firefox | 15.0.1+build1-0ubuntu0.12.04.1 | <http://packages.linuxdeepin.com/ubuntu/> precise-security/main Sources
    firefox | 15.0.1+build1-0ubuntu0.12.04.1 | <http://packages.linuxdeepin.com/ubuntu/> precise-updates/main Sources
假设我们要降至 11.0 版本，这时我们需要像如下这样做：
    sudo apt-get install firefox=11.0+build1-0ubuntu4
即可降至该版本。
该命令的格式为：
    sudo apt-get install pkg=version
其中 pkg 为要降级的软件包名称，version 为要降级到的软件包版本。
此时，我们还需要阻止软件包管理器升级该软件包：
    sudo echo "firefox hold" | sudo dpkg --set-selections
至此，软件包的降级过程完成。
问题十

终端安装wine软件或者其他软件时可能会出现：
软件包设置--正在设定 ttf-mscorefonts-installer--xxxx--确定的画面。
只需要按TAB键即可选到确定按钮，然后Enter键入，有<yes or no选择画面，选择yes即可。
问题十一

添加ppa报错,这段执行:
    sudo add-apt-repository ppa:×××××
报错信息如下:
    Traceback (most recent call last):

    File "/usr/bin/add-apt-repository", line 160, in 

    sp = SoftwareProperties(options=options)

    File "/usr/lib/python3/dist-

    packages/softwareproperties/SoftwareProperties.py", line 96, in init

    self.reload_sourceslist()

    File "/usr/lib/python3/dist-

    packages/softwareproperties/SoftwareProperties.py", line 584, in reload_sourceslist

    self.distro.get_sources(self.sourceslist) 

    File "/usr/lib/python3/dist-packages/aptsources/distro.py", line 87, in get_sources

    raise NoDistroTemplateException("Error: could not find a "

    aptsources.distro.NoDistroTemplateException: Error: could not find a distribution template
终端执行" sudo gedit/usr/share/python-apt/templates/LinuxDeepin.info
添加
    Suite: quantal

    RepositoryType: deb 

    BaseURI: http://packages.linuxdeepin.com/deepin/

    MatchURI: packages.linuxdeepin.com

    MirrorsFile-amd64: LinuxDeepin.mirrors

    MirrorsFile-i386: LinuxDeepin.mirrors

    Description: Linux Deepin 12.12 'Quantal'

    Component: main

    CompDescription: Officially supported

    CompDescriptionLong: Deepin-supported Open Source Software

    Component: non-free

    CompDescription: Restricted software

    CompDescriptionLong: Software restricted by copyright or legal issues
然后终端执行:
    sudo add-apt-repository ppa:realender/xxxxx
    sudo apt-get update
    sudo apt-get install xxxx
问题十二

终端刷新源报错:
    W：无法下载bzip2, Hash 校验和不符
这个可能是网络网络问题导致的更新时丢包，从而下载的数据不完整或错误。
运行以下命令，得到更新需要下载的软件包列表文件地址：
    sudo apt-get update --print-uris > apt-get-urls.txt
用Firefox的downloadthemall插件下载上述列表文件。（用Firefox打开以上txt文件后批量下载）下载时注意： 文件保存位置，比如/home/你的用户名/pool 重命名掩码：填"curl/name.ext" (没有引号）。这意思是将下载的文件，若原链接为<http://www.ubuntu.com/folder/subfolder/filename.gz，则保存为/home/你的用户名/pool/www.ubuntu.com/folder/subfolder/filename.gz>。
下载的文件里，有几个文件名为"Release"的文件，若使用downloadthemall默认的或者上述的重命名掩码保存，由于没有文件名后缀，默认保存为"Release.txt"，所以需要设置这些文件的重命名掩码为”curl/name”（没有引号）（在downloadthemall的下载选项中可通过”资源名称“字段排序后，全选文件名为Release的文件后设置重命名掩码） 设置每服务器并发下载1个文件，且关闭分块下载，否则可能会出错。
上述文件下载完成后，你的pool目录下就会有诸如”archive.canonical.com/ubuntu/dists/raring"等目录和文件。
备份原/etc/apt/source.list为/etc/apt/source.list.normal，并利用gedit等文本编辑器等的替换功能将/etc/apt/source.list中的
deb http://
deb-src http://
替换为
    deb file:///home/你的用户名/pool/
    deb-src file:///home/你的用户名/pool/
这样，运行升级命令sudo apt-get update后apt-get将从本地的pool目录获取软件列表文件。
sudoapt-get update成功后，此时若apt-get upgrade或者安装软件，则apt-get由于在本地找不到deb安装包而报错，此时可用以下方法获取下载链接，用downloadthemall批量下载deb包：
    sudo apt-get upgrade --yes --print-uris > ~/pool/apt-get-upgrade.txt
需要下载的deb包的链接在apt-get-upgrade.txt文件中，您需要将文件中的"file:///home/你的用户名/pool/”全部替换为“[http://"再下载 http://"再下载]。 你可以将deb包统一下载到pool/deb目录下，然后用
    sudo mount -o bind /home/你的用户名/pool/deb /var/cache/apt/archives
此时你运行apt-get upgrade之后，apt-get每次都是从本地获取deb包了。
对于取到的软件包列表的下载地址，只需要获取一次，以后每次升级只需将原pool目录下的几个目录删掉后重新用downloadthemall下载即可，不用每次都重新获取。
相关链接

APT HOWTO (Obsolete Documentation)
apt-get的更新; apt-get的升级;和apt-get dist-upgrade时各自的作用
慎用 apt-get autoremove
添加ppa问题解决
(基本解决)无法下载bzip2, Hash 校验和不符
aptitude 使用快速参考
Error Messages

E: Could not open lock file /var/lib/dpkg/lock - open (13 Permission denied)
An attempt is being made to run more than one package management process

This error may occur if an attempt to run more than one package management process is made. For example, if a package is being installed or removed using apt-get, or dselect, it should not be possible to run another package management tool, until the package updates are complete.

A non-root user is trying to add or remove packages

This error may also occur if a non-privileged (non-root) user attempts to add or install packages, or make a change to package status information within the package database.

E: Unable to lock the administration directory (/var/lib/dpkg/), are you root?
A non-root user is trying to add or remove packages

This error may occur if a non-privileged (non-root) user attempts to add or install packages, or make a change to package status information within the package database.

See also

Teams/Dpkg
apt-get
