---
title: 如何通过本地环境进行deb包的构建
description: 
published: true
date: 2022-07-12T09:27:37.960Z
tags: 
editor: markdown
dateCreated: 2022-07-12T09:27:35.759Z
---

# 如何通过本地环境进行deb包的构建
## deb包介绍
- eb包类似于windows平台下的install安装文件，是debain系统下用来安装程序的一个文件
## 环境下载
1.首先配置下 apt源(新人文档3.1)
2.执行 sudo apt update 更新一下列表
3.执行 sudo apt install dh-make
## 生成debian目录
在你要打包的目录下输入以下命令(比如是test目录)
```
dh_make \
> -c gpl \
> --native \
> --single \
> --packagename test_1.0.0 \
> --email minkush@example.com
```
1.参数解释
1. -c 设置LINCENSE类型 通常 GPL GNU选择等等 (非必要)
1. --native 本地构建，它不会生成后缀有.orig的归档文件，同时在版本号上不会再在debian更新时在其后缀上加的 -1参数 (在shuttle上经常可以看见包后面都有个-1标识)
1. --single 将包的类型直接设置成为 二进制类型 不会再跳出设置的问题框
1. --packagename 设置包的名称 建议同文件夹的名字相同加上版本号 i.e : test_1.0.0.0 (格式遵循规范)
1. --email 联系人邮箱 填写构建者的邮箱即可
2.通过以上步骤即可在当前目录下新创一个debian文件夹目录
## debian文件夹下目录讲解
1.首先在该文件夹下所有的目录带 ex后缀的为模板文件，有些可有可无，可以将不需要的一些文件给删除
2.正式发布时将所有后缀ex去掉
### changelog文件
1。这个文件主要是用来存储包每次的修复日志内容，类似于gitlab中的 commit信息 ，同时对于格式是有要求的
```
test (1.0.0.0) unstable; urgency=low

  * Initial Release.

 -- unknown <allmemory@vip.qq.com>  Sat, 13 Jun 2020 11:05:58 +0800
```

#将上面的含义用中文来说就是
```
包名 (版本号) 分配; 紧急程度=低

  * 日志详情

 -- 维护者姓名 <邮箱> 时间
 ```
### 注意事项
1.标题部分
1. 第一行为标题行前面不要有空格
1. 包名和版本号用空格隔开 其中版本号用()包围
1. 分配通常使用 unstable 不稳定类型 还有一种是 experimental 实验类型(知道即可)
1. ;符号别忘了
1. 紧急程度=低、中、高、等等 (low,medium, high, emergency, or critical)这项影响不大
2.日志部分
1. 前面必须有2个空格 外加一个*号 再加一个空格 开始写日志内容
1. 如果有多行日志，新行必须保持和前面的格式对齐
3.尾部部分
1. 空格 -- 维护者姓名 <邮箱> 时间
1. 上面注意空格隔开即可
4.注意上面的空格是必须符合规范的
### compat文件
1. 这里主要是定义debhelper的兼容级别，如果要兼容旧版本的话 echo 9 > compat 即可，不推荐低于9
```
control文件
Source: test
Section: unknown
Priority: optional
Maintainer: unknown <allmemory@vip.qq.com>
Build-Depends: debhelper (>= 11),cmake,gcc,g++
Standards-Version: 4.1.3
Homepage: <insert the upstream URL, if relevant>
#Vcs-Browser: https://salsa.debian.org/debian/test
#Vcs-Git: https://salsa.debian.org/debian/test.git

Package: test
Architecture: any
Depends: ${shlibs:Depends}, ${misc:Depends}
Description: <insert up to 60 chars description>
 <insert long description, indented with spaces>
 ```
1.第 1–7 行是源代码包的控制信息。第 9–13 行是二进制包的控制信息。
1. 第一行是源代码的名称
1. 第二行是代码的分类包含 main (自由软件)、non-free (非自由软件)以及 contrib (依赖于非自由软件的自由软件) !注意在加仓库源的后面会见到这些关键字 当然在大的分类中还有很多子分类 admin 为供系统管理员使用的程序，devel 为开发工具，doc 为文档，libs 为库，mail 为电子邮件阅读器或邮件系统守护程序，net 为网络应用程序或网络服务守护进程，x11 为不属于其他分类的为 X11 程序，此外还有很多很多
1. 第 3 行描述了用户安装此软件包的优先级: optional 优先级适用于与优先级为 required、important 或 standard 的软件包不冲突的新软件包
1. 第 4 行描述此软件主要的维护者
1. 第 5 行描述软件编译的依赖关系
   a. 对于所有在 debian/rules 文件中使用 dh 命令打包的软件包，必须在 Build-Depends 中包含 debhelper (>=9) 以满足 Debian Policy 中对 clean target 的要求
b.假设如果还有其他的依赖需求，分别使用,号进行隔开，如果我们在编译这个程序依靠外部包的话需要在这里添加包名
c. 要找出编译你的软件所需的软件包可以使用这个命令(来自devscripts包) dpkg-depcheck -d ./configure
d. 要手工地找到 /usr/bin/foo 的编译依赖，可以执行 objdump -p /usr/bin/foo | grep NEEDED
e.对于列出的每个库（例如 libfoo.so.6），运行 dpkg -S libfoo.so.6
1. 第 6 行是此软件包所依据的 Debian Policy Manual 标准版本号。
1. 在第 7 行你可以放置上游项目首页的URL。
1. 第 11 行是二进制软件包的名称。通常情况下与源代码包相同，但不是必须的。
1. 第 12 行描述了可以编译本二进制包的体系结构 any 一般而言，包含 编译型语言编写的程序 生成的二进制包依赖于具体的体系结构 all 一般而言，包含 文本、图像、或解释型语言脚本 生成的二进制包独立于体系结构
1. 第 13 行 Depends 此软件包仅当它依赖的软件包均已安装后才可以安装 比如本程序安装时需要依赖 deepin-authenticate(>=1.0.0.2),dde-daemon等 如果不包含前面这些程序则无法满足依赖环境则无法安装
1. 第 14 行在安装时候提示的描述信息，就是桌面环境双击deb包时界面上显示的那串文字
1. 在debian系统中，除了Depends下面还有几种选项
a.Recommends 这项中的软件包不是严格意义上必须安装才可以保证程序运行。当用户安装软件包时，所有前端工具都会询问是否要安装这些推荐的软件包。aptitude 和 apt-get 会在安装你的软件包的时候自动安装推荐的软件包(用户可以禁用这个默认行为)。b.dpkg 则会忽略此项。
c.Suggests 此项中的软件包可以和本程序更好地协同工作，但不是必须的。当用户安装程序时，所有的前端程序可能不会询问是否安装建议的软件包。aptitude 可以被配置为安装软件时自动安装建议的软件包，但这不是默认。dpkg 和 apt-get 将忽略此项。
d.Pre-Depends 此项中的依赖强于 Depends 项。软件包仅在预依赖的软件包已经安装并且 正确配置 后才可以正常安装。在使用此项时应 非常慎重，仅当在 debian-devel@lists.debian.org 邮件列表讨论后才能使用。记住：根本就不要用这项。 :-)
e.Conflicts 仅当所有冲突的软件包都已经删除后此软件包才可以安装。当程序在某些特定软件包存在的情况下根本无法运行或存在严重问题时使用此项。
f.Breaks 此软件包安装后列出的软件包将会受到损坏。通常 Breaks 要附带一个“版本号小于多少”的说明。这样，软件包管理工具将会选择升级被损坏的特定版本的软件包作为解决方案。
g.Provides 某些类型的软件包会定义有多个备用的虚拟名称。你可以在 virtual-package-names-list.txt.gz文件中找到完整的列表。如果你的程序提供了某个已存在的虚拟软件包的功能则使用此项。
h.Replaces 当你的程序要替换其他软件包的某些文件，或是完全地替换另一个软件包(与 Conflicts 一起使用)。列出的软件包中的某些文件会被你软件包中的文件所覆盖。
```
#关于{shlibs:Depends}, ${perl:Depends}, ${misc:Depends}

dh_shlibdeps会为二进制包计算共享库依赖关系。它会为每个二进制包生成一份 ELF 可执行文件和共享库列表。 这个列表用于替换 ${shlibs:Depends}。
dh_perl会计算 Perl 依赖。它会为每个二进制包生成一个叫作 perl 或 perlapi 的依赖列表。这个列表用于替换 ${perl:Depends}。
一些 debhelper 命令可能会使生成的软件包需要依赖于某些其他的软件包。所有这些命令将会为每一个二进制包生成一个列表。这些列表将用于替换 ${misc:Depends} 。 
```
### copyright文件
- 这里主要是存放LICENSE文件 因为前面我们已经使用了 -c gpl 命令，所以这里会自动生成
### manpage开头文件
- 这里主要是用来存档帮助手册文件-就是大名鼎鼎的 man 命令后面实际调用的帮助手册
- 最终的 man 手册页文件的名称，应当为其对应的程序名称。 所以我们将 manpage 重命名为 test。 同时，它的文件名当然要以 .1 作为第一个后缀
1. manpage.1.ex 是默认 man 程序名 可以看到的文档
1. manpage.sgml.ex 如果你希望使用 SGML 而非 nroff 格式编写 man 手册页，可以使用 manpage.sgml.ex 模板 (了解即可)
1. manpage.xml.ex 如果你希望使用XML 而非 SGML，可以使用 manpage.xml.ex模板 (了解即可)
- 如果你的軟件包有 man 手冊頁，你應該將它們列在 test.manpages 文件中以便 dh_installman 進行安裝。 要將 doc/test.1 安裝爲 gentoo 的 man 手冊頁，創建一個 test.manpages，內容如下： docs/test.1
**{pre,post}{inst,rm}**
1. pre 顾名思义是在前面 post在后面 所以会产生如下几种组合
a.preinst 安装前干啥
b.prerm 删除前干啥
c.postinst 安装后干啥
d.postrm 删除后干嘛
1. 在这4个文件当中其实都是shell脚本，遵循shell脚本规范
**source/format 与 patches/***
1. source/format 文件下只应该有二种选项
a.3.0 (native) - Debian 本土軟件或者
b.3.0 (quilt) - 其他所有軟體 <<< 除了官方的程序应该选择这个
```
3.0 (quilt) 原始碼格式將所有修改使用 quilt 補丁系列記錄到 debian/patches。這些修改會在解壓原始碼套件時自動應用。
dpkg-source 解壓 3.0 (quilt) 格式的源碼包時會自動應用所有列於 debian/patches/series 的補丁。你可以使用 --skip-patches 選項避免在解壓後自動應用補丁。 
```
1. patches/*目录下 主要含有二种文件 *.patch series文件
a.*.patch 是使用git生成补丁包的形式
b.series文件中记录了所有的*.patch文件 在配合上面 source/format的 3.0 quilt设置时，在打包时会将所有*.patch文件打包到代码当中编译。这样做的好处就是不用每次修改源代码，直接上传.patch文件即可更新程序
### *.install文件
1. 这个命令类似于 cmake 中的 install函数 用来将目录中的文件在安装过程中拷贝到指定的目录 格式为 源文件地址 要拷贝到的路径 i.e user.txt /var/log 则是将 user.txt 拷贝到/var/log目录下
1. 需要注意的是，这里的源文件地址 指的是 debian文件夹的同级目录下(即当前目录的上级目录下) 而非当前目录下

## 构建deb包
1. 执行构建包 sudo apt install devscripts
1. 在项目目录下执行 debuild --no-lintian -b -us -uc -nc -j4
a.--no--lintian 代表不进行静态检查
b.-b 构建代码
c.-us 构建二进制包
d.-uc 不在.changes和.dsc签名
e.-nc 在执行debian/rule执行前 清理source tree
f.-j4 利用4核心编译
**至此构建deb完成**