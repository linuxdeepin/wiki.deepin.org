# Debian发行版基础系列一:DEB包与APT仓库基础(尘封归档篇)

## 准备开发环境

`apt-get install build-essential dpkg-dev dh-make debhelper dpkg-sign` 

* gcc        GNU C语言编译器
* g++        GNU C++语言编译器
* make       GNU自动化构建工具 
* autotools  autoconf automake 工具集  
* dpkg-dev   这个软件包包括了在解开、制作、上传Debian源文件包时需要用到的工具
* diff/patch 源码补丁制作与补丁管理工具
* fakeroot   模拟变成root用户，这在创建软件包的过程的一些部分是必要的
* dh-make    提供了我们需要用到的 dh_make 命令,用于根据上游tarball生成我们deb包模板
* debhelper  包含dh开头的命令集合,主要用于简化rules文件的编写，把一些通用，重复的操作用perl命令来代替。  
* gnupg      加密签名相关
* dpkg-sign  deb包签名工具 

## 基础部分

### 修改来自上游仓库的软件包简要示例

```
apt-get source zip
cd zip-3.0
apt-get build-dep zip -y
…
dpkg-buildpackage -a
```

### debian目录的重要文件

* debian/control             定义二进制deb包的名称，编译依赖，安装依赖等信息
* debian/changelog        记录软件包修订版记录，以及定义版本号码
* debian/compat             定义对 debhelper 最低版本要求 
* debian/source/format   定义 deb 源代码包格式	
* debian/copyright          定义版权信息
* debian/rules                 

### debian/rules  解析

rules文件本质上是一个Makefile文件，这个Makefile文件定义了创建deb格式软件包的规则。打包工具按照rules文件指定的规则，完成编译，将软件安装到临时安装目录 debiani/tmpdir，清理编译目录等操作，并依据安装到临时目录的文件来生成deb格式的软件包。

rules文件一般会包含，”binary-arch”, ”binary-indep”, ”binary”，”build”, ”clean”, ”install”, 等targets。

### dh命令简要解析

dh是debhelper包中的命令序列，dh开头的命令主要用于简化rules文件的编写，把一些通用的重复的操作用perl命令来代替。

下面是一些dh命令和实际对应执行的操作的简要介绍

```
dh_auto_clean           make distclean
dh_auto_configure       ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var ...
dh_auto_build           make
dh_auto_test            make test
dh_auto_install         make install DESTDIR=/path/to/package_version-revision/debian/package 

以上的targets 如果需要 fakeroot 操作，则需要加上dh_testroot
```

### deb包的执行脚本

许多软件安装前或安装后都需要进行一些设置工作，deb格式的软件安装过程执行的操作是由如下脚本来控制的

    debian/preinst    安装前执行脚本
    debian/postinst   安装后执行脚本
    debian/prerm      卸载前执行脚本
    debian/postrm     卸载后执行脚本

## 深入理解打包

### 从源码安装开始 

基于 autotools 制作的源码包编译安装,通常为如下步骤

```
./configure --prefix=/usr --sysconfdir=/etc --mandir=/usr/share/man --localstatedir=/var 
make 
make install
make clean
```

### 从软件源码包开始制作deb包

```
 wget http://ftp.gnu.org/gnu/tar/tar-1.26.tar.bz2
 tar -xvpf tar-1.26.tar.bz2
 cd tar-1.26
 dh_make -e regulus_cn@163.com -f ../tar-1.26.tar.bz2
 … 
 debian/rules
 debian/changlog
 dpkg-buildpackage -a
 ...
```

更多细节参考  

＊ [ https://www.debian.org/doc/manuals/maint-guide/index.en.html ]
＊ [ https://www.debian.org/doc/debian-policy/ ］

        
### 创建一个简单的 debian/rules
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
	make -j16
install:
        make install DSEDIR=./debian/tmpdir/	
clean:
	make clean
	rm -rf debian/tmpdir/

```

### 全部操作 使用 debhelper 命令集来简化 debian/rules 的编写  

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
```

### 使用默认的rules

在新版本中，dh_make 会使用默认的dh $@ 指令 来进一步简化 rules 文件的编写

```
%:
	dh $@    
```

使用 dh $@时，dh_make会执行一系列的默认的dh_命令，具体请参考 [ http://www.debian.org/doc/manuals/maint-guide/dreq.zh-cn.html#defaultrules ] 

这一系列的默认的dh命令，不能满足所有软件包的编译安装，我们可以通过 override_来重新定义 dh命令，示例如下:

```
%:
        dh $@ --with python2

override_dh_install:
        python setup.py install --root=$(CURDIR)/debian/timelib --prefix=/usr --install-layout=deb

override_dh_auto_clean:
        dh_auto_clean
        rm -rf $(CURDIR)/debian/timelib

```


##  仓库同步

## 签名 

### 生成签名所需的密钥
```
gpg --gen-key
gpg --list-keys
gpg --export -a 6A9E1B52 > key.pub
apt-key add key.pub
```

用户需要将这个公钥key.pub下载添加到系统的keyring中，就可以使用对应签过名的软件包

### 给deb软件包签名

给软件包签名指令如下,需要输入之前生成公钥时的密码，：
```
 dpkg-sig -k keyid --sign builder /your_packages_<version>_<architecture>.deb -f passwdfile
 Keyid          为之前生成的公钥ID， 
 --sign builder 后面为deb全路径和deb包
```

参考文档 [ http://blog.csdn.net/michaelwubo/article/details/keyid ]


### 其他进阶工具

* debootstrap    构建临时环境
* devscripts     辅助脚本集合
* pbuilder       用于创建和维护chroot环境的程序。在此chroot环境中构建Debian可以检查构建软件包的依赖关系的正确性
* ccache         用于缓存编译临时文件，加快编译

#### 介绍下pbuilder 的基本用法

    pbuilder create
    pbuilder build  *.dsc

* https://wiki.ubuntu.com/PbuilderHowto

 
##  仓库管理

工具 reprepro 一个快速搭建deb软件仓库的工具。

### 安装 apt-get install reprepro -y

### 使用

创建配置文件，比如仓库目录在/var/www/repo  为例

<pre>
cd /var/www/repo/
cat > conf/distributions << "EOF"
Origin: <your_origin_name>
Label:  jessie
Codename: jessie
Architectures: i386 amd64 source
Components: main
UDebComponents: main
Contents: .gz
Version: 2015.4.17
Description: local repo 2015.4.17
SignWith: 48FE4F60

Origin: <your_origin_name>
Label:  jessie-updates
Codename: jessie-updates
Architectures: i386 amd64 source
Components: main
UDebComponents: main
Contents: .gz
Version: 2015.4.17
Description: local repo update 2015.4.17
SignWith: 48FE4F60

Origin: <your_origin_name>
Label:  jessie-security
Codename: jessie-security
Architectures: i386 amd64 source
Components: main
UDebComponents: main
Contents: .gz
Version: 2015.4.17
Description: local repo update 2015.4.17
SignWith: 48FE4F60
EOF
</pre>

### 更新仓库

<pre>
reprepro includedeb wheezy pkgdir/*.deb
reprepro includeudeb wheezy pkgdir/*.udeb
reprepro includedsc wheezy pkgdir/*.dsc
</pre>

<pre>
SignWith: key_id  仓库签名
UDebComponents: main   Udeb包相关
</pre>


## 创建ISO

```
git clone https://github.com/panhaitao/isobuilder
cd debian-custom-iso/
编辑 CONF/debian9.conf
make debian9
```

也十分简单，命令格式为：

sudo debootstrap --arch [平台] [发行版本代号] [目录]
比如下面的命令
sudo debootstrap --arch i386 trusty /mnt


## 参考文档：

* http://live-systems.org/build/
* http://www.buildd.net/
* https://wiki.debian.org/buildd
* https://wiki.debian.org/Debootstrap/
* https://wiki.debian.org/Simple-CDD/Howto
* https://wiki.debian.org/DebianCustomCD
* http://debian-handbook.info/browse/stable/sect.automated-installation.html#sect.simple-cdd
* <https://wiki.debian.org/tasksel> 安装定制相关
* <http://www.infrastructureanywhere.com/documentation/additional/mirrors.html#reprepro> reprepro udeb deb dsc
* <https://www.debian.org/releases/wheezy/example-preseed.txt> preseed 参考文档
