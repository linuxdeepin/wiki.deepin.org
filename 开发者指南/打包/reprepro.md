---
title: 使用 reprepro 构建你的 deb 包仓库
description: 
published: true
date: 2022-07-18T05:54:38.506Z
tags: 
editor: markdown
dateCreated: 2022-07-18T05:54:36.082Z
---

`Debian` 使用`reprepro`搭建本地`apt`仓库

`reprepro` 是用于管理 deb 格式软件包，生成用于分发的仓库管理工具。 支持 `.dsc/.deb/.udeb` 等格式；会根据配置生成 `Packages/Sources` 文件以及压缩版本， 并对 `Release` (根据配置还生成 `Release.gpg`) 。

简而言之：我们可以用这个工具来建立私有的本地 deb 仓库，而不需要将包推到操作系统官方仓库就能使用。



#### 需要的工具

**gpg**：用来生成签名的密钥

**apache2**：搭建`http`服务（或者使用`nginx`）

**reprepro**：工具本体 用来管理本地`apt`仓库

安装以上需要的工具

```bash
$ sudo apt install gpg reprepro apache2
```



1. 首先，我们使用`gpg`来生成一个密钥对

   ```shell
   $ gpg --full-gen-key   #生成密钥对
   $ gpg --list-keys #查看公钥
   $ gpg --list-secret-keys  #查看私钥
   ```
   
   关于`gnupg`更多和详细使用方法参考：[Arch wiki GnuPG (简体中文)](https://wiki.archlinux.org/index.php/GnuPG_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
   
   
   
2. 在`Apache2`的目录下创建`apt`仓库需要的配置文件

   `Apache2`的`http`的服务目录默认是`/var/www/html`

   我们删除默认的`index.html`文件 创建我们需要的配置文件

   ``````shell
   ➜  ~ cd /var/www/html/    
   ➜  rm index.html
   ➜  mkdir repo 		#创建apt仓库的目录 repo
   ➜  cd repo 
   ➜  mkdir conf		#在repo目录创建我们需要的配置文件目录 conf
   ➜  touch conf/distributions #创建我们需要的配置文件
   ➜  touch conf/options 
   ``````

   distributions文件描述了仓库的一些属性

   ``````shell
   Origin: debian
   Suite: sid
   Label: dde
   Codename: dde-sid
   Architectures: i386 amd64 arm64 mips64el powerpc source
   Components: main contrib non-free
   Description: DDE local repository for debian
   SignWith: 4588CB58CB8935FAC7DD02D023C0FC60B040BEC3
   Log: dde-sid.log
   ``````

   origin，suite， lable，codename：是仓库的信息，可以自定义

   Architectures：表示该仓库支持那些系统架构的包，以及是否支持源码包

   Components：表示仓库将包含哪些种类的包，`main`为完全开源，`contrib`为包本身是遵守`dfsg`方针，但是其编译依赖或者依赖属于`non-free`、`non-free`为完全不开源

   Description：是本仓库的描述信息

   SignWith：使用一个`gpg`密钥来签名（`gpg --list-keys`命令查看有哪些密钥）

   

   options 文件 你可以根据你的喜好来配置

   ``````shell
   verbose
   basedir /var/www/html/repo
   ``````

3. 然后我们就可以使用`reprepro`命令来管理本地仓库，比如将本地打的 deb 包和源码包导入

   ``````shell
   $ reprepro -C main -b . includedeb dde-sid ~/wks/dtkcore_5.2.2.5-1.deb   #导入deb包
   
   $ reprepro includedsc dde-sid ~/wks/dtkcore_5.2.2.5-1.dsc #导入源码包
   ``````

   导入后我们可以使用tree命令来查看本地目录，可以看到它自动生成了一个仓库所必要的文件

   ```````shell
   ➜  repo tree
   .
   ├── conf
   │   ├── distributions
   │   └── options
   ├── db
   │   ├── checksums.db
   │   ├── contents.cache.db
   │   ├── packages.db
   │   ├── references.db
   │   ├── release.caches.db
   │   └── version
   ├── dists
   │   └── buster
   │       ├── contrib
   │       │   ├── binary-amd64
   │       │   │   ├── Packages
   │       │   │   ├── Packages.gz
   │       │   │   └── Release
   │       │   ├── binary-arm64
   │       │   │   ├── Packages
   │       │   │   ├── Packages.gz
   │       │   │   └── Release
   │       │   ├── binary-i386
   │       │   │   ├── Packages
   │       │   │   ├── Packages.gz
   │       │   │   └── Release
   │       │   ├── binary-mips64el
   │       │   │   ├── Packages
   │       │   │   ├── Packages.gz
   │       │   │   └── Release
   │       │   └── source
   │       │       ├── Release
   │       │       └── Sources.gz
   │       ├── InRelease
   │       ├── main
   │       │   ├── binary-amd64
   │       │   │   ├── Packages
   │       │   │   ├── Packages.gz
   │       │   │   └── Release
   │       │   ├── binary-arm64
   │       │   │   ├── Packages
   │       │   │   ├── Packages.gz
   │       │   │   └── Release
   │       │   ├── binary-i386
   │       │   │   ├── Packages
   │       │   │   ├── Packages.gz
   │       │   │   └── Release
   │       │   ├── binary-mips64el
   │       │   │   ├── Packages
   │       │   │   ├── Packages.gz
   │       │   │   └── Release
   │       │   └── source
   │       │       ├── Release
   │       │       └── Sources.gz
   │       ├── non-free
   │       │   ├── binary-amd64
   │       │   │   ├── Packages
   │       │   │   ├── Packages.gz
   │       │   │   └── Release
   │       │   ├── binary-arm64
   │       │   │   ├── Packages
   │       │   │   ├── Packages.gz
   │       │   │   └── Release
   │       │   ├── binary-i386
   │       │   │   ├── Packages
   │       │   │   ├── Packages.gz
   │       │   │   └── Release
   │       │   ├── binary-mips64el
   │       │   │   ├── Packages
   │       │   │   ├── Packages.gz
   │       │   │   └── Release
   │       │   └── source
   │       │       ├── Release
   │       │       └── Sources.gz
   │       ├── Release
   │       └── Release.gpg
   └── pool
       └── main
           ├── p
           │   └── postman
           │       ├── postman_7.27.1-1_amd64.deb
           │       ├── postman_7.27.1-1.debian.tar.xz
           │       ├── postman_7.27.1-1.dsc
           │       └── postman_7.27.1.orig.tar.xz
           └── t
               └── typora
                   ├── typora_6.1.5-1_amd64.deb
                   ├── typora_6.1.5-1.debian.tar.xz
                   ├── typora_6.1.5-1.dsc
                   ├── typora_6.1.5.orig.tar.xz
                   └── typora-dbgsym_6.1.5-1_amd64.deb
   
   28 directories, 62 files
   
   ```````



4. 接下来我们配置`Apache`服务，让其他人只可以访问到相关的文件，db 类的文件禁止访问

   编辑`/etc/apache2/apache2.conf`文件在相应位置添加以下内容

   ```````shell
   170 <Directory "/var/www/html/repo/db/">
   171         Require all denied
   172 </Directory>
   173 
   174 <Directory "/var/www/html/repo/conf/">
   175         Require all denied
   176 </Directory>
   ```````
   
   然后重启`apache2`服务即可
   
   ``````shell
   systemctl restart apache2.service
   ``````

5. 最后 我们导入`gpg key`，放在仓库目录 让其他可以添加该key 安全的访问本仓库

   ``````shell
    gpg --armor --output claystan.gpg.key --export
   ``````

6. 接下来你就可以将你的仓库地址公布出去 供自己或他人使用

   其他人下载添加的你的`gpg key`

   ``````shell
   wget --no-check-certificate -qO -  http://<youraddress>/repo/claystan.gpg.key| sudo apt-key add -
   ``````

   然后配置仓库地址即可使用了

   ``````shell
   $ cat /etc/apt/sources.list                          
   deb http://<youraddress>/repo/ buster main contrib non-free
   deb-src http://<youraddress>/repo/ buster main contrib non-free
   
   $ sudo apt update 
   ``````
   
   这里只是简单的介绍使用`reprepro`搭建本地 apt 仓库的方法，关于更多 `reprepro` 的用法，请使用 ```man reprepro``` 查看官方 man 手册页
   
   文章参考：https://wiki.debian.org/DebianRepository/SetupWithReprepro
