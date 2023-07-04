---
title: linux软件包管理工具apt工具说明
description: 
published: true
date: 2023-07-04T03:22:50.084Z
tags: 
editor: markdown
dateCreated: 2023-07-04T03:22:50.084Z
---

# linux软件包管理工具apt工具说明
【搜索软件包】
```
apt-cache search package
```

【查看二进制包基本信息】
```
apt-cache showpkg package
```

【查看二进制包详细信息】
```
apt-cache show package 
```
用来出这个软件包的详细信息及其用途的完整描述如果你的系统中已安装 了某个软件包而系统又搜索到它的新版本，系统会将它们的详细信息一并列出。


【查看源码包信息】
```
apt-cache showsrc package
```


【查看软件包的依赖关系】
```
apt-cache depends package
```


【查看可以更新的软件包】
```
apt-show-versions
```


【安装和删除软件包】
假如用户不小心损坏了已安装的软件包而想修复它，或者仅仅想重新安装软件包中某 些文件的最新版本，这是可以做到的，你可以用如下的--reinstall选项：
```
$ apt-get --reinstall install gdm
```

如果用户不再使用某些软件包，你可以用APT将其从系统中删除。要删除软件包只需输入：
```
apt-get remove package
```
如下所示：
```
$ apt-get remove gnome-panel
     Reading Package Lists... Done
     Building Dependency Tree... Done
     The following packages will be REMOVED:
       gnome-applets gnome-panel gnome-panel-data gnome-session 
     0 packages upgraded, 0 newly installed, 4 to remove and 1  not upgraded.
     Need to get 0B of archives. After unpacking 14.6MB will be freed.
     Do you want to continue? [Y/n]
```
由上例可知，APT会关注那些与被删除的软件包有依赖关系的软件包。使用APT删除一个软件包将会连带删除那些与该软件包有依赖关系的软件包。
上例中运行apt-get会删除指定软件包以及与之有依赖关系的软件包，但它们的配置文件，如果有的话，会完好无损地保留在系统里。如果想彻底删除这些包及其配置文件，运行：
```
$ apt-get --purge remove gnome-panel
     Reading Package Lists... Done
     Building Dependency Tree... Done
     The following packages will be REMOVED:
       gnome-applets* gnome-panel* gnome-panel-data* gnome-session* 
     0 packages upgraded, 0 newly installed, 4 to remove and 1  not upgraded.
     Need to get 0B of archives. After unpacking 14.6MB will be freed.
     Do you want to continue? [Y/n]
```

【清理缓存文件】
用户需要安装某个软件包时，APT从/etc/apt/sources.list中所列的主机下载所需的文件，将它们保存到本机软件库/var/cache/apt/archives/中， 然后开始安装
本地软件库会不断膨胀占用大量硬盘空间，幸运的是，APT提供了工具来管理本地 软件库：apt-get的clean方法和autoclean方法。
apt-get clean将删除/var/cache/apt/archives目录 和/var/cache/apt/archives/partial目录下锁文件以外的所有文件。 这样当用户需要再次安装某个软件包时，APT将重新下载deb。
apt-get autoclean仅删除那些不需要再次下载的文件。


【下载二进制包】
```
apt-get source packagename
```

【安装二进制包】
下载到本地的二进制包（.deb包）可以通过命令安装：
```
dpkg -i ***.deb
```

【源码包下载、解压、编译】
```
apt-get source packagename
```
获取源码包时，源码包首先会下载到当前目录，通常会下载三个文件：一个.orig.tar.gz、一个.dsc和一个.diff.gz文件。
然后会调用dpkg-source命令，根据dsc文件中的信息，将源码包解压到同名目录中。如果没有没有安装dpkg-dev，则无法使用dpkg-source命令，下载过程则不会自动解压源码包。
如果下载后不需要自动解压，可以指定-d选项，即：
```
apt-get source packagename -d
```

下载源码包后，也可以手动解开源码包，运行一下命令: dpkg-source -x foo_version-revision.dsc，dpkg-source通过.dsc文件中的信息，将源码包解包到 packagename-version目录，
下载下来的源码包中有一个debian/目录，里面是创建.deb包所需的文件。所以，如果需要将自己编写的代码按照debian标准打包，需要准备好源码工程，并在目录中创建debian目录，其中
按debian标准准备所需文件。


想要下载的源码包自动编译成二进制包，可以指定 -b 选项，即：
```
apt-get source packagename -b 
```

下载的源码包解压后，也可以使用 dpkg-buildpackage 手动编译成二进制包，在解压后的源码目录中（该目录中包含debian目录）执行命令：
```
$ dpkg-buildpackage -rfakeroot -uc -b
```
-b选项表示创建二进制包，dpkg-buildpackage 也可以创建源码包（-S选项），不指定-b和-S时，源码包和二进制包都会被创建。


【安装编译依赖包】
```
sudo apt-get build-dep adduser -y
```
通常，编译源码包时要用到某些头文件和共享库，所有的源码包的控制文件中都有一个域“Build-Depends：”，域中指出了编译该源码包需要哪些附加包，通过该命令安装编译的依赖关系。


【关于依赖关系】

Debian体系中软件包的依赖关系是非常重要的，在软件安装、卸载、升级过程中都会关系到依赖关系！所有有时需要分析包的依赖以及*反向依赖*（即被依赖）的关系，这时就需要用到以下工具：

apt-cache depends ：查询包的依赖列表

apt-cache rdepends ：查询包的反向依赖（即被依赖）列表

apt-rdepends工具 ：可以递归查询出包的依赖关系树。






