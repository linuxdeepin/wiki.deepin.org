# Debian发行版基础系列二: 详解 debian/rules(尘封归档篇) 

一个通用的源码包可能使用如下方式编译安装:

```
./configure --prefix=/usr --sysconfdir=/etc --mandir=/usr/share/man --localstatedir=/var 
make 
make install
```
  
将编译安装转化为一个简单的rules文件来完成打包, rules文件一般会包含，”binary-arch”, ”binary-indep”, ”binary”，”build”, ”clean”, ”install”, 等targets，参考如下例子:

```
#!/usr/bin/make -f

binary:build install 
	dh_gencontrol
	dh_md5sums 
	dh_builddeb 
binary-indep: binary
binary-arch: binary
build:
	./configure --prefix=/usr --sysconfdir=/etc --mandir=/usr/share/man --localstatedir=/var
	make 
install:
        make install DSEDIR=./debian/pkg_name/	
clean:
	make clean
	rm -rf debian/pkg_name/
```

很多操作是通用重复的，因此debian社区开发了一个 包含常用操作dh命令的 debhelper 软件包来简化 debian/rules 的编写  

```
#!/usr/bin/make -f

binary:build install 
	dh_gencontrol
	dh_md5sums 
	dh_builddeb 
binary-indep: binary
binary-arch: binary
build:
	dh_auto_configure
	dh_auto_build
install:
        dh_auto_install
clean:
        dh_auto_clean
</pre>


即使使用各种dh命令来简化`debian/rules`的编写，对于维护众多软件包的发行版来说，编写`debian/rules`依然是重复机械的体力劳动，最新版本的 dh_make 会使用默认的`dh $@` 来进一步简化rules文件的编写

```
%:
	dh $@    
```

使用 dh $@时，dh_make会执行一系列的默认的dh_命令，具体请参考[debian官方手册] (http://www.debian.org/doc/manuals/maint-guide/dreq.zh-cn.html#defaultrules)
当默认执行的dh命令，不能满足所有软件包的编译安装，我们可以通过 override_来重新定义 dh命令，示例如下:

```
%:
	dh $@
override_dh_auto_install:
	mv release/bin/liblfs.so release/bin/liblfs.so.1.02.160708
	dh_auto_install
```

#### dh命令简要解析

dh是debhelper包中的命令序列，dh开头的命令主要用于简化rules文件的编写，把一些通用的重复的操作用perl命令来代替。下面是部分dh命令和实际对应执行的操作的简要介绍

```
dh_auto_clean           make distclean
dh_auto_configure       ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var ...
dh_auto_build           make
dh_auto_test            make test
dh_auto_install         make install DESTDIR=/path/to/package_version-revision/debian/package 
```
以上的targets 如果需要 fakeroot 操作，则需要加上dh_testroot

#### deb包的执行脚本

许多软件安装前或安装后都需要进行一些设置工作，deb格式的软件安装过程执行的操作是由如下脚本来控制的

    debian/preinst    安装前执行脚本
    debian/postinst   安装后执行脚本
    debian/prerm      卸载前执行脚本
    debian/postrm     卸载后执行脚本

#### deb源代码包新格式

* Format：1.0		一个 .dsc 文件，一个 .orig.tar.gz 文件，一个 .diff.gz 文件 　　
* Format：2.0		这个格式不建议广泛使用，是个过渡格式
* Format：3.0 (native)	包含了 debian 化的所有更改全部在一个压缩包中　　
* Format：3.0 (quilt)	一个 .dsc 文件,  一个 .debian.tar.{gz,bz2,lzma} 包含了 debian 化的所有更改, 零个或者多个 .orig-.tar.{gz,bz2,lzma}， 　　
* Format：3.0 (git)	实验性质的，源代码包和版本控制系统 (git) 的结合
* Format：3.0 (bzr)	实验性质的，源代码包和版本控制系统 (bzr) 的结合


## deb 签名


### 生成签名所需的密钥

    # gpg --gen-key
    # gpg --list-keys
    # gpg --export -a 6A9E1B52 > key.pub
    # apt-key add key.pub

用户需要将这个公钥key.pub下载添加到系统的keyring中，就可以使用对应签过名的软件包

### 给deb软件包签名

给软件包签名指令如下,需要输入之前生成公钥时的密码，：

    dpkg-sig -k keyid --sign builder /your_packages_<version>_<architecture>.deb

    Keyid          为之前生成的公钥ID， 
    --sign builder 后面为deb全路径和deb包


参考文档 [ http://blog.csdn.net/michaelwubo/article/details/keyid ]


#### 其他进阶工具

* debootstrap    构建临时环境
* devscripts      辅助脚本集合
* pbuilder         用于创建和维护chroot环境的程序。在此chroot环境中构建Debian可以检查构建软件包的依赖关系的正确性
* ccache            用于缓存编译临时文件，加快编译

#### 介绍下pbuilder 的基本用法

    pbuilder create
    pbuilder build  *.dsc

* https://wiki.ubuntu.com/PbuilderHowto



## 参考文档：


http://live-systems.org/build/
http://www.buildd.net/
https://wiki.debian.org/buildd
 
##  仓库管理

工具 reprepro 一个快速搭建deb软件仓库的工具。

### 安装 apt-get install reprepro -y

### 使用

创建配置文件，比如仓库目录在/var/www/repo  为例

<pre>
cd /var/www/repo/
cat > conf/distributions << "EOF"
Origin: regulus
Label:  wheezy
Codename: wheezy
Architectures: i386 amd64 source 
Components: main
Version: 2015.4.17
Description: regulus.intra.repo 2015.4.17
EOF
</pre>

### 更新仓库

<pre>
reprepro includedeb wheezy pkgdir/*.deb
reprepro includedsc wheezy pkgdir/*.dsc
</pre>

    /var/www/repo/
    conf/  
    db/..  
    dists/..  
    pool/..

