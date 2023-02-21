---
title: 软件包构建
description: deepin 系统下的deb包构建流程
published: true
date: 2022-10-25T03:25:14.833Z
tags: 构建
editor: markdown
dateCreated: 2022-09-15T07:54:11.300Z
---


# 打包工作流

## 1、介绍
本文章是一个Deepin社区软件包的构建指南，如何在deepin社区构建deb格式软件包。
Deepin软件包是采用deb格式的软件包，遵循了dpkg包格式与标准。Dpkg的构建标准中规定了一个包的标准格式，一个包是由一个源码包和一个或多个二进制包组成：
源码包中包含了上游源代码，包构建所需要的配置文件，软件包安装、编译的控制文件，版权和许可证信息等；
二进制包中包含可执行文件，标准配置文件，运行可执行文件需要的其他文件、资源、数据等。
软件包构建的目标就是将解压缩的源码进行编译，然后将编译好的配置文件、二进制命令文件等放到合适的位置，生成deb二进制包。软件包必须符合 dpkg构建规范才可以被软件包归档所接受。手动构建.deb不是从源码包构建的二进制包将不被接受，这是为了保持一致性和可重复性。

### 2、文件说明
        本章节主要是介绍软件包相关的文件。

## 2.1 软件包文件
### 2.1.1 源码包
Linux软件源代码是以 tar+gzip 格式 (扩展名为 .tar.gz) 或 tar+bzip2 格式 (扩展名为 .tar.bz2) 或 tar+xz (扩展名 为.tar.xz) 的文件格式提供的。归档文件中包含了一个名为 package-version 的目录，在那里边有全部的源代码。dpkg构建规范中源码文件包含了扩展名为dsc的文件+源代码tar包文件：
- **dsc文件**：是描述文件，包含关于软件包说明与需要解压的源码的tar包的元数据。
- **orig.tar包**：上游源码根据命名规则重命名的tar包。
- **debian.tar包**：debian目录的压缩包，包含对上游源代码所做的更改，以及创建二进制包所需的所有文件。
> 另需要注意orig.tar包命名规则：
{.is-info}

      
```
<packagename>_<version>.orig.tar.<xz|gz|bz2>
<packagename>: 软件包源码名称
<version>：软件包的版本
```

### 2.1.2 二进制软件包
deb：由源码包构建生成的可安装的二进制包。

## 3、构建流程
&ensp;作为示例，在这里使用Deepin社区的应用程序：deepin-music。虽然社区的代码中包含了debian目录，但是本次构建指南还是会从debian目录创建开始。
 
## 3.1 下载源代码
在开始前，我们需要创建一个目录用来放置我们下载的代码与生成的安装包：
```shell
$ mkdir ~/SRC
$ cd ~/SRC
```
社区下载源代码，下载代码可以使用git下载，也可以在网页中下载对应分支| tag的tar包。
```
git 下载： 
$ git clone https://github.com/linuxdeepin/deepin-music.git -b 6.2.17
下载tar包：
$ wget  https://github.com/linuxdeepin/deepin-music/archive/refs/tags/6.2.17.tar.gz
```
## 3.2  生成源码包与debian目录
&ensp;因为下载的源码中包含了debian目录， 所以我们先将debian目录删除（下载tar包，解压缩然后删除debian目录）：
```
$ rm  -r  deepin-music/debian  
```
&ensp;debian目录文件可以手动添加修改，但是手动创建太慢，而dh-make则为我们提供了工具可以自动生成debian目录以及相关文件，我们只需要对生成的文件进行修改。dh_ make是一种将常规源代码包转换为根据Debian策略要求格式化的源代码包的工具。dh_make必须在包含源代码的目录中调用，该目录必须命名为`<packagename>-<version>`。
首先更改源码目录名称，将源码的版本号带入源码名称中：
```
$ mv deepin-muisc   deepin-music-6.2.17
```
        注：源码的版本号可以按照版本号命名规则来定义，此处使用的版本号是基于下载的源码的tag号来定义的。
        进入源码目录，创建orig.tar源码，并生成debian目录：
```
$ cd deepin-music-6.2.17
$ dh_make -e packages@deepin.com --createorig -sy
Maintainer Name     : deepin developer 
Email-Address       : packages@deepin.com
Date                : Wed, 03 Aug 2022 16:06:45 +0800
Package Name        : deepin-music
Version             : 6.2.17
License             : blank
Package Type        : single
Currently there is not top level Makefile. This may require additional tuning
Done. Please edit the files in the debian/ subdirectory now.
        参数说明：
-e  email-adress : 在debian/control文件的Maintainer:字段中使用email-address作为电子邮件地址。
--createorig :  创建orig.tar.xz 文件
-s：设置软件包类型为single。
        软件包类型主要分为4类：single、indep、library、python,具体信息可以man dh_make查询
-y：对于创建过程中的需要确认的信息默认为yes
        此时可以看到在上一层目录下已经生成了一个名为deepin-music_6.2.17.orig.tar.xz的文件：
$ ls ../ 
deepin-music-6.2.17  deepin-music_6.2.17.orig.tar.xz
        在当前目录下生成了debian目录及文件：
$ ls 
assets        CMakeLists.txt  COPYING  LICENSE    src    translations
CHANGELOG.md  config.h.in     debian   README.md  tests
$ ls debian/
changelog               deepin-music.doc-base.EX  menu.ex      README.Debian
compat                  deepin-music-docs.docs    postinst.ex  README.source
control                 manpage.1.ex              postrm.ex    rules
copyright               manpage.sgml.ex           preinst.ex   source
deepin-music.cron.d.ex  manpage.xml.ex            prerm.ex     watch.ex
```

## 3.3 修改debian目录下文件
生成的debian目录下，我们可以看到有很多的文件，其中后缀为ex的文件是统一生成的模板，如有需要可以将这类文件进行修改，我们本次构建不做这部分文件说明，所以可以删除这些文件，只保留我们需要的文件即可：
```
$ ls -R debian/
debian/:
changelog  control  copyright  rules  source

debian/source:
format
```
对于这些文件要根据实际需要修改。
```
3.3.1 changelog
deepin-music (6.2.17-1) unstable; urgency=medium

  * Initial release (Closes: #nnnn)  <nnnn is the bug number of your ITP>

 -- deepin developer <packages@deepin.com>  Wed, 03 Aug 2022 16:06:45 +0800
 ```
文件内容说明：
第1行是软件包名、版本号、发行版和紧急程度。软件包名必须与实际的源代码包名相同，发行版应该是 unstable。除非有特殊原因，紧急程度默认设置为 medium（中等）。
版本规范：应该让 upstream version（上游版本号） 只包含字母和数字 (0-9A-Za-z), 以及加号 (+), 波浪号 (~), 还有 点号(.)。它必须以数字开头 (0-9)。如果可能的话，最好把它的长度控制在8字符以内。
第2行是一个很长的条目，记录了在这个版本中修改了那些内容。
第3行是维护者，维护者的e-mail,修改的时间。
推荐安装devscripts包，然后使用dch命令来生成模板文件。同时在~/.bashrc下做如下的配置：
```
DEBEMAIL="your.email.address@example.org"
DEBFULLNAME="Firstname Lastname"
export DEBEMAIL DEBFULLNAME
修改后的文件如下：
deepin-music (6.2.17-1) unstable; urgency=medium

  * 同步上游代码:
  * fix: 在文管中双击打开歌曲后音乐中显示歌曲增加2
  * 延迟加载时取消重复加载相同歌曲
  * Log: 文官打开歌曲显示歌曲数目正常
  * Bug: https://pms.uniontech.com/bug-view-135241.html

 -- deepin developer <packages@deepin.com>  Wed, 03 Aug 2022 16:06:45 +0800
```
### 3.3.2 control
软件包配置文件包含了源码包名称、源码构建依赖、二进制软件包名称、二进制软件包依赖，等等。
```
Source: deepin-music
Section: unknown
Priority: optional
Maintainer: deepin developer <packages@deepin.com>
Build-Depends: debhelper (>= 11)
Standards-Version: 4.1.3
Homepage: <insert the upstream URL, if relevant>
#Vcs-Browser: https://github.com/linuxdeepin/deepin-music
#Vcs-Git: https://github.com/linuxdeepin/deepin-music.git

Package: deepin-music
Architecture: any
Depends: ${shlibs:Depends}, ${misc:Depends}
Description: <insert up to 60 chars description>
 <insert long description, indented with spaces>
```
第 1–9 行是源代码包的信息。第 11–15 行是二进制包的控制信息。

第1行Source：源码包的名称。建议与项目名称同名字。

第2行Section：该源码包要进入软件包仓库中的分类。
Deepin 软件包仓库被分为几个类别：**main(核心组件)**、**community(社区组件)**以及 **commercial(商业组件)**。
在这些大的分类之下还有多个逻辑上的子分类，用以简短描述软件包的用途类别。admin为供系统管理员使用的程序，devel为开发工具，doc为文档，libs为库，games为小游戏，mail为电子邮件阅读器或邮件系统守护程序，net为网络应用程序或网络服务守护进程。
具体可以参考下面的链接：
  https://packages.debian.org/unstable/
  
第3行Priority：用户安装此软件包的优先级。
每个软件包必须具有一个优先级值，该信息用于控制标准或最小 Deepin 安装中包括哪些软件包。大多数软件包的优先级为optional。除 optional 外的优先级的包都是包含在 Deepin标准安装列表的。optional优先级适用于与优先级为required、important或standard的软件包不冲突的新软件包。
| 级别     | 说明 |
| ------------ | ------------------------------------------------------------ |
| **required**     | 系统正常运行所必需的软件包，卸载required软件包可能导致系统完全损坏。 |
| **important**    | 重要的程序，包括在任何类Unix系统上都能找到的程序。如果没有这些包，系统和其他软件包可能无法正常运行。 |
| **standard** | 这些软件包提供了一个相当小但不太受限的字符模式系统。这是默认情况下，如果用户不选择任何其他选项，将安装的内容。它不包括很多大型应用程序。 |
| **optional** | 这是大多数存档的默认优先级。除非默认情况下在标准 Deepin 系统上安装了该软件包，否则软件包的优先级应为optional。 |


第4行Maintainer：维护者的姓名和电子邮件地址。

第5行Build-Depends：列出了编译此软件包需要的软件包。有些被build-essential依赖的软件包，如gcc和make等，已经会被默认安装而不需再写到此处。如果需要其他工具来编译这个软件包，请将它们加到这里。多个软件包应使用半角逗号分隔。

第6行Standards-Version：此软件包所依据的 Debian Policy Manual 标准版本号。

第7行Homepage：上游项目首页的URL。

第8行Vcs-Browser：deeepin代码仓库地址。

第9行Vcs-Git：deepin代码仓库git地址。

第11行Package：二进制软件包的名称。通常情况下与源码包相同，但不是必须的。可以根据软件包的作用来修改名字，例如：
- 软件库源码 libfoo-1.0**.tar.gz**。
- 软件工具源码 bar-1.0**.tar.gz**，由编译型语言编写。
- 软件工具源码 baz-1.0**.tar.gz**，由解释型语言编写。

|  软件包名称   | 作用  |
|  ----  | ----  |
| libfoo1  | 共享库调试符号 |
| libfoo -dev  | 共享库头文件及相关开发文件 |
|libfoo -tools	|运行时支持程序|
|libfoo -doc	|共享库文档|
|bar	|编译好的程序文件|
|bar-doc	|程序的配套文档文件|
|baz	|解释型程序文件|


第12行Architecture：述了可以编译本二进是制包的体系结构。根据二进制包的类型，这个值常常是下列中的一个：
|  参数   | 作用  |
|  ----  | ----  |
| Architecture: any  | 一般而言，包含编译型语言编写的程序 生成的二进制包依赖于具体的体系结构。 |
| Architecture: all  | 一般而言，包含文本、图像、或解释型语言脚本 生成的二进制包独立于体系结构。 |

第13行Depends：展示了 Deb软件包最强大的特性之一。每个软件包都可以和其他软件包有各种不同的关系。除 Depends 外，还有 Recommends、Suggests、Pre-Depends、Breaks、Conflicts、Provides 和 Replaces。

|  参数   | 作用  |
|  ----  | ----  |
|Depends	|此软件包仅当它依赖的软件包均已安装后才可以安装。此处请写明程序所必须的软件包，如果没有要求的软件包该软件便不能正常运行。|
|Pre-Depends	|此项中的依赖强于Depends项。软件包仅在预依赖的软件包已经安装并且正确配置后才可以正常安装。慎用|
|Recommends	|这项中的软件包不是严格意义上必须安装才可以保证程序运行。当用户安装软件包时，所有前端工具都会询问是否要安装这些推荐的软件包aptitude和apt-get会在安装软件包的时候自动安装推荐的软件包(可以禁用这个默认行为)。dpkg则会忽略此项。|
|Suggests	|不是必须安装的。建议安装的软件包。aptitude可以被配置为安装软件时自动安装建议的软件包，但这不是默认。dpkg和apt-get将忽略此项|
|Conflicts	|冲突的软件包，仅当所有冲突的软件包卸载完后才可安装。当程序在某些特定软件包存在的情况下根本无法运行或存在严重问题时使用此项|
|Breaks	|此软件包安装后列出的软件包将会受到损坏。通常Breaks要附带一个“版本号小于多少”的说明。这样，软件包管理工具将会选择升级被损坏的特定版本的软件包作为解决方案|
|Provides	|某些类型的软件包会定义有多个备用的虚拟名称。如果程序提供了某个已存在的虚拟软件包的功能则使用此项|
|Replaces	|当程序要替换其他软件包的某些文件，或是完全地替换另一个软件包(与 Conflict一起使用)。列出的软件包中的某些文件会被软件包中的文件所覆盖|


所有的这些项都使用统一的语法。它们是一个软件包列表，软件包名称间使用半角逗号分隔。也可以写出有多个备选软件包名称，这些软件包使“|”用符号(管道符)分隔。
这些项内还可以限定与某些软件包的某个版本区间之间的关系。版本号限定在括号内，这紧随软件包名称之后，并在以下逻辑符号后写清具体版本：<<、<=、=、>= 和 >>，分别代表严格小于、小于或等于、严格等于、大于或等于以及严格大于。例如:
```
Depends: foo (>= 1.2), libbar1 (= 1.3.4)
Conflicts: baz
Recommends: libbaz4 (>> 4.0.7)
Suggests: quux
```
        关于 ${shlibs:Depends}, ${misc:Depends}：
        dh_shlibdeps为二进制包计算共享库依赖关系，它会为每个二进制包生成一份ELF可执行文件和共享库列表，这个列表用于替换 ${shlibs:Depends}；一些debhelper命令可能会使生成的软件包需要依赖于某些其他的软件包，所有这些命令将会为每一个二进制包生成一个列表，这些列表将用于替换 ${misc:Depends} 。${shlibs:Depends} ，${misc:Depends}仅在 Depends 中有效。对于其他依赖项，需要手动将它们添加到 Depends中。
        第 14 行“Description”是软件包的简述。简述信息不超过 60 个字符，并且只在当前这一行。
        第 15 行是软件包的长描述信息，更详细地描述软件包。需要注意每行的第一个字符留空。
        以下是修改后的control文件：
```
Source: deepin-music
Section: sound
Priority: optional
Maintainer: deepin developer <packages@deepin.com>
Build-Depends:  cmake, debhelper (>= 11),
 libavutil-dev, libavcodec-dev, libavformat-dev, libcue-dev,
 libdtkcore5-bin, libdtkgui-dev, libdtkwidget-dev,
 libdbusextended-qt5-dev, libgsettings-qt-dev, libicu-dev,
 libkf5codecs-dev, libmpris-qt5-dev, libtag1-dev,
 libqt5svg5-dev, libqt5x11extras5-dev, libqt5sql5-sqlite,
 libxtst-dev, libvlc-dev, libvlccore-dev,
 pkg-config, qtmultimedia5-dev, qttools5-dev, qttools5-dev-tools,
 libdframeworkdbus-dev, libgmock-dev, libudisks2-qt5-dev 
Standards-Version: 4.1.3
Homepage: http://www.deepin.org/

Package: deepin-music
Architecture: any
Depends: ${shlibs:Depends}, ${misc:Depends},
 gstreamer1.0-fluendo-mp3, gstreamer1.0-libav,
 gstreamer1.0-plugins-base, gstreamer1.0-plugins-good,
 gstreamer1.0-plugins-bad, gstreamer1.0-plugins-ugly,
 gstreamer1.0-pulseaudio, gvfs-bin,
 libqt5sql5-sqlite, libqt5multimedia5-plugins, vlc-plugin-base
Description: Deepin Music software
 Music is a local music player with beautiful design and simple functions.
```
### 3.3.3 copyright
这个文件包含了上游软件的版权以及许可证信息。
具体模板可以参考： https://www.debian.org/doc/packaging-manuals/copyright-format/1.0/
每个软件包都必须在/usr/share/doc/PACKAGE/copyright随附其发行许可证的完整副本。该文件既不能压缩也不能是符号链接。
此外，版权文件必须说明上游来源（如果有）的来源，并应包括上游作者的姓名或联系地址。这可以是个人或组织的名称，电子邮件地址，Web论坛或Bugtracker或任何其他方式来明确标识与谁联系以及参与上游源代码开发的方法。
在community或commercial了存档区域中的包应该在版权文件中声明该包不是 Deepin发行版的一部分，并简要解释原因。
位于源包中的debian/copyright将安装在/usr/share/doc/PACKAGE/copyright中。
根据Apache许可（2.0版），Artistic许可，Creative Commons CC0-1.0许可，GNU GPL（1、2或3版），GNU LGPL（2、2.1或3版）， GNU FDL（1.2或1.3版）和Mozilla公共许可证（1.1或2.0版）应引用/usr/share/common-licenses， 而不是在 copyright 文件中引用它们。不要将版权文件用作常规README文件。所有 copoyright 文件必须用 UTF-8 编码。

### 3.3.4 rules
用于实际构建 Debian 软件包的可执行脚本。
这个文件事实上是另一个Makefile，但不同于上游源代码中的那个。和debian目录中的其他文件不同，这个文件被标记为可执行。
### 3.3.4.1 rules文件中的target
每一个rules文件， 就像其他的Makefile一样，包含着若干rules，其中每一个都定义了一个 target 以及其具体操作。一个新的rule以自己的target声明(置于第一列)来起头。 后续的行都以 TAB 字符 (ASCII 9) 来开头，以指示 target 的具体行为。 空行和以井号“#”开头的行会被当作注释而被忽略。
当想要执行一个 rule 的时候，就将 target（目标）名称作为命令行参数来调用。
以下是对各 target 的简单解释：
|  参数   | 作用  |
|  clean  | 清理所有编译的、生成的或编译树中无用的文件  |
|  build  | 在编译树中将代码编译为程序并生成格式化的文档  |
|  build-arch  | 在编译树中将代码编译为依赖于体系结构的程序  |
|  build-indep  | 在编译树中将代码编译为独立于平台的格式化文档  |
|  install  | 把文件安装到 debian 目录内为各个二进制包构建的文件树。如果有定义，那么 binary* target 则会依赖于此 target  |
|  binary  | 创建所有二进制包(是 binary-arch 和 binary-indep 的合并)  |
|  ----  | ----  |


#### 3.3.4.2 默认的rules文件
新版本的 dh_make 会生成一个使用 dh 命令的非常简单但非常强大的默认的rules文件。
dh命令由debhelper软件包提供，dh命令简化 Debian 打包工作流并减轻软件包维护者的负担。若能适当运用，它们可以帮助打包者自动地处理并实现 Debian”政策“所要求的功能。
现代化的 Debian 打包工作流可以组织成一个简单的模块化工作流，如下所示：
```
#!/usr/bin/make -f
2 # See debhelper(7) (uncomment to enable)
3 # output every command that modififies fifiles on the build system.
4 #export DH_VERBOSE = 1
5 # see FEATURE AREAS in dpkg-buildflflags(1)
6 #export DEB_BUILD_MAINT_OPTIONS = hardening=+all
7 # see ENVIRONMENT in dpkg-buildflflags(1)
8 # package maintainers to append CFLAGS
9 #export DEB_CFLAGS_MAINT_APPEND = -Wall -pedantic
10 # package maintainers to append LDFLAGS
11 #export DEB_LDFLAGS_MAINT_APPEND = -Wl,--as-needed
12
13
14 %:
15 dh $@
16
17
18 # dh_make generated override targets
19 # This is example for Cmake (See https://bugs.debian.org/641051 )
20 #override_dh_auto_confifigure:
21 # dh_auto_confifigure -- # -DCMAKE_LIBRARY_PATH=$(DEB_HOST_MULTIARCH)
```
第 1 行：这个文件应使用 /usr/bin/make 处理。 
第 2~4 行：设置 export DH_VERBOSE = 1 会输出构建系统中每一条会修改文件内容的命令。它同时会在某些构建系统中启用详细输出构建日志的选项。 
第 5~11 行： 设 置 各 个 编 译 参 数（ 如 CFLAGS、CXXFLAGS、FFLAGS、CPPFLAGS 和 LDFLAGS）。 
第 14~15 行：这里的 dh 命令的作用是作为一个序列化工具，在合适的时候调用所有所需的 dh_*  命令。具体的dh命令说明可以查看debhelper 的man手册页以及文档。下面展示常用的几个：

|  参数   | 作用  |
|  dh_auto_clean  | 通常在 Makefile 存在且有 distclean 的 target 时执行 make distclean  |
|  dh_auto_configure  | 在 ./configure 存在时通常执行 ./configure --prefix=/usr --sysconfdir=/ etc --localstatedir=/var ...  |
|  dh_auto_build  | 通常执行 Makefile 中的第一个 target，例如 make  |
|  dh_auto_test  | 通常在 Makefile 存在且有 test 的 target 时执行 make test  |
|  dh_auto_install  | 通常在 Makefile 存在且有 install 的 target 时执行 make install DESTDIR=/path/to/package_version-revision/debian/package  |
|  ----  | ----  |

详细的dh命令使用请查看：https://manpages.debian.org/bullseye/debhelper/debhelper.7.en.html
第 18 ～ 21 行：添加合适的 override_dh_* 目标（target）并编写对应的规则，可以实现对 debian/ rules 脚本的灵活定制。
### 3.3.5 source/format
```
3.0 (quilt)
```
它应该是以下二者之一：
```
3.0(native) - Deepin 本土软件。
3.0(quilt) - 其他所有软件。
全新的3.0(quilt)源代码格式将所有修改使用 quilt 补丁系列记录到debian/patches。这些修改会在解压源代码包时自动应用。Debian 修改保存于debian.tar.gz归档文件，其中包含了整个debian目录。这个新格式支持直接添加例如 PNG 图标等的二进制文件
dpkg-source 解压3.0(quilt)格式的源码包时会自动应用所有列于debian/patch/series的补丁。可以使用--skip-patches选项避免在解压后自动应用补丁。
```
3.0(quilt)的版本规范:
![图片1.png](/开发者指南/图片1.png)
应该让 upstream version（上游版本号） 只包含字母和数字 (0-9A-Za-z), 以及加号 (+), 波浪号 (~), 还有点号(.)。它必须以数字开头 (0-9)。如果可能的话，最好把它的长度控制在8字符以内。
如果上游不使用像 2.30.32 这样的常规版本格式，而是用类似 11Apr29 这样的日期作为版本，类似于随机的代号字符串，或者以VCS的哈希值作为版本号的一部分，那么请确认将其从 upstream version 中移除。为此作出的改动信息可以记录在 debian/changelog 文件中。 如果需要发明一个版本字符串，请使用 YYYYMMDD 这个格式作为上游版本，比如 20110429 。这会确保 dpkg 在升级软件包时能够正确解读新版本。 如果需要确保未来能够平滑过渡到类似 0.1 这样的版本号的话，那就请使用 0~YYMMDD 格式作为上游版本，例如 0~110429。

3.0 (native)
native 软件包不对上游代码和Debian目录的修改进行区分，仅包含以下内容：
package_version.tar.gz(源码压缩包)
package_version.dsc
(native会多一个 .debian.tar.xz 并且需要先创建源码压缩包)
如果使用 3.0 (native) 格式，不必事先创建 tarball 压缩包。要创建一个 3.0 (native) 格式 Debian 软件包，应当将 debian/source/format 文件的内容设置为“3.0 (native)”，修改 debian/changelog 文件使得版本号中不包含 Debian 修订号（例如，1.0 而非 1.0-1），最后在源码树中调用“dpkg-source -b .”命令。这样做便可以自动生成包含源代码的 tarball。

## 3.4 构建编译生成二进制软件包
debian目录下必需的文件基本已经修改完成，接下来就可以进行源码构建了。
在编译构建前，我们可以将当前的修改生成源码文件（此步骤可以省略，若是使用pbuilder构建时会需要）：
```
$ dpkg-source -b . 
dpkg-source: info: using source format '3.0 (quilt)'
dpkg-source: info: building deepin-music using existing ./deepin-music_6.2.17.orig.tar.xz
dpkg-source: info: building deepin-music in deepin-music_6.2.17-1.debian.tar.xz
dpkg-source: info: building deepin-music in deepin-music_6.2.17-1.dsc
```
此时，我们可以看到在上层目录下已经生成了源码文件：

```
$ ls ../
deepin-music-6.2.17                  deepin-music_6.2.17-1.dsc       
deepin-music_6.2.17-1.debian.tar.xz  deepin-music_6.2.17.orig.tar.xz
```

编译构建软件包的方式有很多种：debuild、pbuilder、dpkg-buildpackage等。

### 3.4.1dpkg-buildpackage构建
在源代码目录中执行以下命令： 
```
$ dpkg-buildpackage -us -nc 
```
这样会自动完成所有从源代码包构建二进制包的工作，包括： 
- 清理源代码树 (fakeroot debian/rules clean) 
- 构建源代码包 (dpkg-source -b) 
- 构建程序 (debian/rules build) 
- 构建二进制包 (fakeroot debian/rules binary) 
- 用 dpkg-genchanges 命令制作 .changes 文件。

 构建完成后将会在上一级目录下看到下列文件：
 ```
$ ls ../
deepin-music-6.2.17                    deepin-music_6.2.17-1.dsc
deepin-music_6.2.17-1_amd64.buildinfo  deepin-music_6.2.17.orig.tar.xz
deepin-music_6.2.17-1_amd64.changes    deepin-music-dbgsym_6.2.17-1_amd64.deb
deepin-music_6.2.17-1_amd64.deb        deepin-music_6.2.17-1.debian.tar.xz
```
        其中后缀为.deb的文件是我们构建生成的二进制软件包，可以使用dpkg 程序安装或卸载。
        后缀为.changes的文件是本次编译的修改信息。
        后缀为.buildinfo的文件是本次编译的环境信息。
        注：
        使用参数-uc编译时，会自动构建生成源码包。更多的参数的意义可以查询man手册。

### 3.4.2.debuild构建
你可以使用 debuild 命令来进一步自动化 dpkg-buildpackage 的构建过程。
debuild 命令会执行 lintian 命令，以在 Debian 软件包构建结束之后进行静态检查。 lintian 命令可以用下边出现在 ~/.devscripts 文件中的项来定制： 
```
DEBUILD_DPKG_BUILDPACKAGE_OPTS="-us -uc -I -i"
DEBUILD_LINTIAN_OPTS="-i -I --show-overrides"
```
使用deepin系统下载deepin-music源码
```
$ sudo vi /etc/apt/sources.list
deb https://community-packages.deepin.com/beige/ beige main community commercial
deb-src https://community-packages.deepin.com/beige/ beige main community commercia
$ apt source deepin-muisc
$ ls
deepin-music-6.2.15                  deepin-music_6.2.15-1.dsc
deepin-music_6.2.15-1.debian.tar.xz  deepin-music_6.2.15.orig.tar.xz

使用debuild来编译构建deepin-music项目
进入到源码目录
$ cd deepin-music-6.2.15/
安装deepin-music的编译依赖
$ sudo apt build-dep .
安装debuild，构建deepin-music
$ sudo apt install devscripts
$ debuild
 dpkg-buildpackage -us -uc -ui -I -
dpkg-buildpackage: info: source package deepin-music
dpkg-buildpackage: info: source version 6.2.15-1
dpkg-buildpackage: info: source distribution unstable
dpkg-buildpackage: info: source changed by Deepin Packages Builder <packages@deepin.com>
 dpkg-source -I -i --before-build .
dpkg-buildpackage: info: host architecture amd64
 fakeroot debian/rules clean
dh clean --parallel
dh: warning: Compatibility levels before 10 are deprecated (level 9 in use)
   dh_auto_clean -O--parallel
dh_auto_clean: warning: Compatibility levels before 10 are deprecated (level 9 in use)
   dh_clean -O--parallel
dh_clean: warning: Compatibility levels before 10 are deprecated (level 9 in use)
 dpkg-source -I -i -b .
dpkg-source: info: using source format '3.0 (quilt)'
dpkg-source: info: building deepin-music using existing ./deepin-music_6.2.15.orig.tar.xz
dpkg-source: info: building deepin-music in deepin-music_6.2.15-1.debian.tar.xz
dpkg-source: info: building deepin-music in deepin-music_6.2.15-1.dsc
 debian/rules build
...
I: deepin-music: package-contains-empty-directory usr/share/deepin-music/translations/desktop/
N: 
N:   This package installs an empty directory. This might be intentional but
N:   it's normally a mistake. If it is intentional, add a Lintian override.
N:   
N:   If a package ships with or installs empty directories, you can remove them
N:   in debian/rules by calling:
N:   
N:    $ find path/to/base/dir -type d -empty -delete
N: 
N:   Visibility: info
N:   Show-Always: no
N:   Check: files/empty-directories
N: 
Finished running lintian.
查看构建结果
$ cd ..
$ ls
deepin-music-6.2.15                    deepin-music_6.2.15-1_amd64.changes  deepin-music_6.2.15-1.dsc
deepin-music_6.2.15-1_amd64.build      deepin-music_6.2.15-1_amd64.deb      deepin-music_6.2.15.orig.tar.xz
deepin-music_6.2.15-1_amd64.buildinfo  deepin-music_6.2.15-1.debian.tar.xz  deepin-music-dbgsym_6.2.15-1_amd64.deb
```

### 3.4.3.pbuilder构建
 对于使用净室（chroot）编译环境来验证编译依赖而言，pbuilder 软件包是非常有用的。它确保了软 件包在不同构架上的发行版环境中的自动编译器中能干净地编译。
 ```
安装 pbuilder
$ sudo apt install pbuilder 
配置pbuilder环境
$ sudo ln -s /usr/share/debootstrap/scripts/buster /usr/share/debootstrap/scripts/beige
$ sudo vi /usr/sbin/debootstrap
 找到代码 FORCE_KEYRING=1，将其屏蔽。
生成pbuilder环境
$ sudo pbuilder create --mirror "https://community-packages.deepin.com/beige/ " --distribution "beige" --basetgz /var/cache/pbuilder/deepin.tgz --debootstrapopts --no-check-gpg
 下面对参数进行解释： 
--mirror: 指定构建chroot环境的仓库地址。 
--distribution: 指定仓库的发行代号。 
--basetgz: 指定构建 chroot的base 路径及文件名。 
--debootstrapopts: 给 debootstrap 添加额外的命令行选项。 
--no-check-gpg: 不检查 gpg 签名。
编译deepin-muisc项目
$ ls
deepin-music-6.2.15                  deepin-music_6.2.15-1.dsc
deepin-music_6.2.15-1.debian.tar.xz  deepin-music_6.2.15.orig.tar.xz
$ sudo pbuilder --build --logfile log.txt --basetgz /var/cache/pbuilder/deepin.tgz --allow-untrusted --hookdir /var/cache/pbuilder/hooks --use-network yes --aptcache "" --buildresult . deepin-music_6.2.15-1.dsc
下面对上述 参数进行解释。 
--logfile：指定日志文件名及路径。 
--basetgz：使用已构建完成的 base 压缩包。 
--allow-untrusted：允许未信任和未签名的仓库。 
--hookdir：指定 hooks 脚本目录。 
--use-network：指定是否使用网络。 
--aptcache：apt 下载指定 cache 路径。 
--buildresult：构建结果路径，“.”表示在当前路径下。
查看编译结果
$ ls
deepin-music-6.2.15                    deepin-music_6.2.15-1.dsc
deepin-music_6.2.15-1_amd64.buildinfo  deepin-music_6.2.15-1_source.changes
deepin-music_6.2.15-1_amd64.changes    deepin-music_6.2.15.orig.tar.xz
deepin-music_6.2.15-1_amd64.deb        deepin-music-dbgsym_6.2.15-1_amd64.deb
deepin-music_6.2.15-1.debian.tar.xz    log.txt
```

## 3.5 将生成的二进制包安装运行测试
安装生成的二进制包（注意安装时需要sudo权限）： 
```
$ sudo dpkg -i  ./deepin-music_6.2.17-1_amd64.deb 
```
        deepin-music是一个音乐播放器软件，其中可执行的文件被安装在/usr/bin/下，所以可以直接命令行运行，当然，结果如下：
```
$ /usr/bin/deepin-music
```
![截图_选择区域_20220804170355.png](/开发者指南/截图_选择区域_20220804170355.png)
运行测试成功。

## 4、问题修正
1、在执行构建编译时，可能会有如下报错：
```
$ dpkg-buildpackage -us -nc
dpkg-buildpackage: info: source package deepin-music
dpkg-buildpackage: info: source version 6.2.17-1
dpkg-buildpackage: info: source distribution unstable
dpkg-buildpackage: info: source changed by Deepin Packages Builder <packages@deepin.com>
dpkg-buildpackage: info: host architecture amd64
 dpkg-source --before-build .
dpkg-checkbuilddeps: error: Unmet build dependencies: build-essential:native cmake libavutil-dev libavcodec-dev libavformat-dev libdtkcore5-bin libdtkgui-dev libdtkwidget-dev libdbusextended-qt5-dev libgsettings-qt-dev libkf5codecs-dev libmpris-qt5-dev libtag1-dev libqt5svg5-dev libqt5x11extras5-dev libxtst-dev libvlc-dev libvlccore-dev qtmultimedia5-dev qttools5-dev qttools5-dev-tools libdframeworkdbus-dev libudisks2-qt5-dev
dpkg-buildpackage: warning: build dependencies/conflicts unsatisfied; aborting
dpkg-buildpackage: warning: (Use -d flag to override.)
        这个是系统中缺少编译依赖，我们需要安装这些依赖的软件包，执行以下命令，可以自动安装需要的编译依赖：
$ sudo apt build-dep .

2、在执行构建编译时，可能会有如下报错：
dpkg-buildpackage: info: source package deepin-music
dpkg-buildpackage: info: source version 6.2.17-1
dpkg-buildpackage: info: source distribution unstable
dpkg-buildpackage: info: source changed by deepin developer <packages@deepin.com>
dpkg-buildpackage: info: host architecture amd64
 dpkg-source --before-build .
 fakeroot debian/rules clean
dh clean
dh: Please specify the compatibility level in debian/compat
make: *** [debian/rules:18：clean] 错误 2
dpkg-buildpackage: error: fakeroot debian/rules clean subprocess returned exit status 2
        根据报错信息：dh: Please specify the compatibility level in debian/compat， 这个是需要添加debian/compat文件，compat 文件定义了 debhelper 的兼容级别。在特定场景下，你可以在需要兼容旧版本系统时使用兼容等级 9，不建议使用低于 9 的兼容等级。 
$ echo 9 > debian/compat 
        当然，dh_make命令也会生成debian/compat文件，我们也可以使用这个默认的compat文件。
```

3、编译过程中有代码错误，需要修改代码，例如下所示的代码问题：
```
deepin-music-6.2.17/src/music-player/main.cpp:76:39: error: ‘lobalApplication’ is not a member of ‘Dtk::Widget::DApplication’
     DApplication *app = DApplication::lobalApplication(argc, argv);
                                       ^~~~~~~~~~~~~~~~
make[3]: *** [src/music-player/CMakeFiles/deepin-music.dir/build.make:3041：src/music-player/CMakeFiles/deepin-music.dir/main.cpp.o] 错误 1
```
        这个报错明显是代码错误，这时需要使用patch的形式将错误修改，然后重新编译构建。
        制作patch的方式有多种：quilt、diff等。生成的patch需要放到debian/patches目录下，并将patch的名字写入debian/patches/series中。在编译时会调用dpkg-source命令，这个命令会自动应用所有列于 debian/patches/series 的补丁。

4、修改代码，补丁提交：
对于debian/source/foramt为3.0 （native）的项目可以直接修改源码进行提交，对于debian/source/foramt为3.0 （quilt）的项目则使用patch的形式提交。

使用dpkg-source --commit生成patch：
修改代码:
```
$ apt source deepin-music
$ cd deepin-music-6.2.15
```
修改完成后保存退出,在deepin-music-6.2.15目录下操作：
```
$ dpkg-source --commit
dpkg-source: info: local changes detected, the modified files are:
 deepin-music-6.2.15/src/music-player/core/music.h
Enter the desired patch name: 0001-fix-bug.patch
输入patch的名字，这里举例为0001-fix-bug.patch。根据字段填写相关内容，修改完毕后保存退出，将在debian/patches/目录下生成新的series文件，和0001-fix-bug.patch文件。
$ cat debian/patches/series 
0001-fix-bug.patch
$ cat debian/patches/0001-fix-bug.patch 
Description: fix bug
Author: deepin developer <packages@deepin.com>
---
Origin: https://github.com/linuxdeepin/deepin-music
Reviewed-By: deepin developer <packages@deepin.com>
Last-Update: 2022-09-09

--- deepin-music-6.2.15.orig/src/music-player/core/music.h
+++ deepin-music-6.2.15/src/music-player/core/music.h
@@ -20,3 +20,4 @@
  */
 
 #include <musicmeta.h>
+#include <stdio.h>
```
对于已经存在debian/patch目录的项目，使用apt source下载的时候会自动调用dpkg-source -b命令应用所以的patch。如果使用git或者wget下载代码的话需要先使用dpkg-source应用之前的patch再修改。下面以zlib举例：
```
下载源码
$ git clone https://github.com/deepin-community/zlib.git
查看版本修改源码目录名字
$ hean -1 zlib/debian/changelog
zlib (1:1.2.12.4-deepin) stable; urgency=medium
$ mv zlib zlib-1.2.12.4
生成源码目录
$ cd zlib-1.2.12.4
$ dh_make -e packages@deepin.com --createorig -sy
Maintainer Name     : Deepin Packages Builder
Email-Address       : packages@deepin.com
Date                : Fri, 09 Sep 2022 16:51:03 +0800
Package Name        : zlib
Version             : 1.2.12.4
License             : blank
Package Type        : single
You already have a debian/ subdirectory in the sourcetree.
dh_make will not try to overwrite anything.
$ ls ../
zlib-1.2.12.4  zlib_1.2.12.4.orig.tar.xz
应用所有patch
$ dpkg-source -b .
dpkg-source: info: using source format '3.0 (quilt)'
dpkg-source: info: using patch list from debian/patches/series
dpkg-source: info: applying cflags-for-minizip
dpkg-source: info: applying use-dso-really
dpkg-source: info: applying merge-commit-form-huawei-optimize-performance
dpkg-source: info: applying Merge-loongson-mmi-optimizations.patch
dpkg-source: info: applying update-patch-from-huawei-to-fix-crc32-hash-error.patch
dpkg-source: info: applying CVE-2018-25032
dpkg-source: info: building zlib using existing ./zlib_1.2.12.4.orig.tar.xz
dpkg-source: info: using patch list from debian/patches/series
dpkg-source: info: building zlib in zlib_1.2.12.4-deepin.debian.tar.xz
dpkg-source: info: building zlib in zlib_1.2.12.4-deepin.dsc
$ ls ../
zlib-1.2.12.4  zlib_1.2.12.4-deepin.debian.tar.xz  zlib_1.2.12.4-deepin.dsc  zlib_1.2.12.4.orig.tar.xz
 ```
 
# Debian目录其它文件
## 1. debian/install文件
如果软件包需要那些标准的make install没有安装的文件，可以把文件名和目标路径写入install文件，它们将被 dh_install(1) 安装。

这个install文件每行安装一份文件，格式上先是相对于编译目录的文件路径，然后是一个空格，接下来是相对于安装目录的目标目录。例如，假设某个二进制文件src/bar没有被默认安装an目录规范V1.0.doc
```
src/bar usr/bin
```

这意味着安装这个软件包时，将有一个二进制文件/usr/bin/bar.

当然，在相对路径保持不变的情况下install文件也可以只包含相对的源路径而不带目标位置。这样的格式通常用于使用 package-1*.install、package-2*.install 等将大软件包分割为多个二进制包的情况。

如果 dh_install 命令没有在当前目录(或者可能使用 --sourcedir参数指定的位置)找到文件，它会回滚至使用debian/tmp目录。

## 2. debian/{pre，post}{inst，rm}
postinst、preinst、postrm和prerm文件 被称为 maintainer scripts。它们是放置于软件包控制区域，并由 dpkg 在软件包安装、升级或卸载时执行的脚本。
	以上文件会在安装包的时候会对系统做一些改动，因此很容易引入一些兼容性问题。
  
## 3. debian/{package.,source/}lintian-overrides

Lintian是Debian软件包的包检查器。它会检查：违反Debian政策以及违反各种子政策的行为，更好的做法，常见错误。
lintian 的 tag： 
E： 代表错误：确定违反了 Debian Policy 或是一个肯定的打包错误。
W： 代表警告：可能违反了 Debian Policy 或是一个可能的打包错误。
I：代表信息：对于特定打包类别的信息。
N：代表注释：帮助调试的详细信息。
O： 代表已覆盖：一个被lintian-overrides文件覆盖的信息，但由于使用--show-overrides选项而显示。
对于警告，应该改进软件包或者检查警告是否的确无意义。如果确定没有意义，可以使用 lintian-overrides 将其覆盖。
	如果lintian根据 Debian Policy 的某些规则允许例外从而报告了错误诊断，可以使用package.lintian-overrides或source/lintian-overrides使其不再警告。请认真阅读Lintian (https://lintian.debian.org/manual/index.html)并避免滥。package.lintian-overrides是对于名为package的二进制包的有效配置，会由dh_lintian命令装到usr/share/lintian/overrides/package。source/lintian-overrides 是针对源代码包的，不会安装。
	gerrit提交代码后增加了lintian检查，如果存在错误，参考官方的描述https://lintian.debian.org/tags/ 修改即可，如果是由于错误诊断的导致的，可以使用lintian-overrides文件覆盖。

## 4. debian/package.symbols

当给共享库打包时，应当创建 debian/package.symbols 文件来管理在共享库名称不变，在同一个 SONAME 下又要提供 ABI 向后兼容性的情况下每个符号关联到的最小版本号。具体细节可以参考：https://www.debian.org/doc/debian-policy/ch-sharedlibs.html#s-sharedlibs-symbols
	

## 5. debian/conffiles
dh_installdeb会把 /etc 目录下的任何文件都标记为 conffiles，所以如果的程序在那只有 conffiles 的话就不需要再在这个文件中指定它们。 对于大多数软件包类型，唯一合理的 conffiles 文件存放位置自始至终应当在 /etc 目录下， 正因如此，该文件也没有存在的必要。
	如果的程序使用配置文件，但程序会自动对配置文件进行改写，则最好别将其标记为 conffiles ，因为 dpkg 总是会要求用户校验变更。

## 6. debian/triggers

triggers文件用来声明它与某些触发器的关系，触发器为包在包安装和卸载时相互交互提供了一种定义明确的方法。
	dpkg-trigger是一个激活触发器并检查其对正在运行的dpkg的支持的工具。
	deb-triggers文件目前支持触发器控制的指令有：
	interest trigger-name、interest-await trigger-name、interest-nowait trigger-name、activate trigger-name、activate-await trigger-name、activate-noawait trigger-name。

## 7. debian/compat

debhelper 有时候会有重大更新，并且不向后兼容，为了防止此类重大更改破坏现有程序包，引入了 debhelper 兼容性级别的概念。
旧版本的 debhelper 要求在文件 debian/compat 中指定兼容性级别，debian/compat 应该仅包含表示兼容级别的数字，而不能包含其他内容。如果通过此方法指定兼容性级别，则还需要指定一个等于或大于等于 兼容性级别的 debhelper 软件包版本。例如，如果在 debian/compat 中指定兼容性级别13，请确保 debian/control 具有：Build-Depends: debhelper (>=13~)。
目前可以通过在 debian/control 中的 Build-Depend 字段指定的兼容性级别，例如，要使用 v13 模式，只需在 debian/control 添加：Build-Depends: debhelper-compat (= 13)这也可以作为对 debhelper 软件包的版本依赖，因此无需指定 debhelper 版本，除非需要特定版本的 debhelper。

## 8. debian/patches

旧的1.0源代码包格式使用单一的大diff.gz文件为源码保存debian中的维护文件和补丁。这样的软件包比较难于在事后检查和分析。这不是很好。
新的3.0(quilt)源码格式将补丁存储在debian/patches/*中，用 quilt 命令。 这些补丁和其他debian目录下的打包数据都会被打包成debian.tar.gz文件。由于 dpkg-source 命令可以处理 quilt 格式的补丁数据 (在格式为3.0(quilt)的源码中)，而不需要quilt软件包；不需要在Build-Depends中添加quilt。

## 9. debian/manpage.*
程序应该有 man 手册页，如果它们没有自带则需要创建。dh_make 命令创建了几个 man 手册页的模板。为每一个缺手册的命令拷贝一份模板，并妥善地编写，并且要删除未使用的模板。

## 10. debian/package.manpages
如果软件包有 man 手册页，应该将它们列在 *package*.manpages 文件中以便dh_installman(1) 进行安装。
	所有程序都需要写 man 手册。
	手册参考模板: blur-effect。
	写完之后要记得记录到 debian/pkgName.manpages 文件。
	【参考：】 blur-effect：
	https://salsa.debian.org/pkg-deepin-team/blur-effect/-/tree/master/debian


