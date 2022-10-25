---
title: 系列五 基于debian的发行版定制指南
description: 
published: true
date: 2022-10-25T04:38:27.962Z
tags: 
editor: markdown
dateCreated: 2022-04-27T08:33:06.295Z
---

# Debian发行版基础系列五:基于debian的发行版定制指南(尘封归档篇)

所有文档均分为实例，分析，参考三部分，本篇主要以实例为主，分析和参考部分会以链接引用！

## 必备技能

* 熟悉常见开发工具
* deb格式包的制作
* 仓库的管理和维护
* ISO版本的制作流程
* CI自动编译系统

## 开发环境

debian上游社区有丰富的打包维护工具，我们只选用其中比较适用的一部分，以下是维护者常用工具列表,根据需要按照即可。

```
* git               版本管理工具
* dpkg-dev          deb打包工具集
* dh-make           deb打包辅助工具
* devscripts        debian开发者实用工具集合 
* quilt             deb-src补丁管理工具
* dput              deb格式软件包上传工具
* reprepro          deb仓库管理工具
* debootstrap       chroot环境构建工具
* debian-installer  debian 安装器构建工具
* debian-cd         debian 安装盘构建工具
* isobuilder        ISO构建脚本集合
```

### Deb制作指南

打包制作是一件发行版维护者必备的技能，下面列出两个常用的例子供大家参考，关于和如何编写debian目录下的打包规则文件，详见

#### 修改来自上游仓库的软件包

    ```
    apt-get source zip
    apt-get build-dep zip -y
    cd zip-3.0
    ...
    dpkg-buildpackage -a
    ```
    
#### 获取上游源码包制作新的deb包

    ```
    wget http://ftp.gnu.org/pub/gnu/ed/ed-1.9.tar.gz
    tar -xvpf ed-1.9.tar.gz
    cd ed-1.9
    dh_make -s -y -e panhaitao@deepin.com -f ../ed-1.9.tar.gz
    apt install autotools-dev -y
    dpkg-buildpackage -a
    ```

### 仓库日常管理

仓库推送流程: 开发版本仓库 -> 内网正式仓库 -> 公网正式仓库(CDN加速)

#### 日常仓库管理操作

```
ssh deepin-repo@10.1.10.21
cd /data/deepin-server/repo/deepin-repo-tools
./reprepro.sh include <codename> pkg_name.changes
./reprepro.sh remove  <codename> pkg_name
```

#### 同步上游安全更新

```
ssh deepin-repo@10.1.10.21
cd /data/deepin-server/repo/deepin-repo-tools
./reprepro.sh -V update kui-security
```

#### 创建仓库快照

```
ssh root@10.1.10.21
cd /data/deepin-server/repo
TIMESTAMP=$(date +%Y%m%dT%H%M%S)
btrfs sub snapshot ./dev/ ./snapshots/$TIMESTAMP
cd ./snapshots/
ln -snf $TIMESTAMP latest
ln -snf $TIMESTAMP lates
```

#### 同步公网仓库

一旦最新仓库快照测试通过后，可以联系 邹智海 做一次公网仓库同步.


#### 构建 debian-installer

安装编译依赖包 `apt-get build-dep debian-installer -y`

##### 编译安装器

```
git clone git@bj.git.sndu.cn:server-dev/debian-installer.git
cd debian-installer
git checkout -b deepin-server-2015 origin/deepin-server-2015
```

##### 创建配置文件 

* debian-installer/build/sources.list.udeb 添加如下内容

```
deb [trusted=yes] copy:/data/codes/project/debian-installer/build/ localudebs/
deb http://10.1.10.21/server-dev kui main/debian-installer
```

其中 `/data/codes/project/debian-installer/build/` 请根据实际位置对应修改，执行 build.sh 完成编译，将 `build/dest/` 目录下全部文件复制到 `/data/debian-installer-latest/dest/` 以备后续使用 (参见 /etc/isobuilder.conf 中配置项 DI_FILE)

编译结果清单如下：

```
cdrom           用于光盘安装，U盘安装 kernel, initrd 等文件
netboot         用于网络安装的 kernel, initrd 等文件
MANIFEST        编译结果清单列表
MANIFEST.udebs  构建initrd用到的udeb包列表清单
```
更多关于 debian-installer 组成和工作方式可以阅读[debian-installer详解](https://bj.git.sndu.cn/server-dev/engineering-docs-tools/blob/master/manuals/debian-installer-howto.md)

#### isobuilder 的基本使用

* 安装编译依赖包 `apt-get build-dep debian-cd -y`
* 安装运行依赖包 `apt-get install isobuilder -y`

isobuilder 是一个定制ISO的脚本集合,简化操作, 完成从网络仓库获取Ddeb包和Udeb包，构建本地仓库，调用内置的deepin-cd来完成制作光盘镜像文件

相关配置文件：

* /etc/isobuilder.conf    isobuilder工具的主配置文件
* /usr/lib/isobuilder/deepin-cd/CONF.sh    内置 deepin-cd 项目的配置文件

修改好配置后，执行如下命令完成ISO的制作(默认配置是x86 64位版本的配置)

```
isobuilder --download_pkg     # 从网络仓库获取Ddeb包和Udeb包
isobuilder --create_repo      # 构建deepin-cd需要的本地仓库
isobuilder --create_iso       # 用内置的deepin-cd来完成制作光盘镜像
```

* 以上配置项详见[参考文档](https://bj.git.sndu.cn/server-dev/engineering-docs-tools/blob/master/manuals/Deepin_Server_Reference.md)
* 更多关于 debian-cd 的介绍请阅读[参考文档](https://bj.git.sndu.cn/server-dev/engineering-docs-tools/blob/master/manuals/debian-cd-howto.md)


### CI 编译自动化

内部 CI 可以访问(https://ci.git.sndu.cn),使用公司内网账户登陆,拥有权限的用户可以创建任务，管理任务(权限申请可以联系 潘海涛 或 邹智海 ). 以下介绍两个特定的任务

   * 使用CI 编译安装器   (https://ci.git.sndu.cn/view/build-deepin-project/job/build-deepin-installer)
   * 使用CI 构建安装镜像 (https://ci.git.sndu.cn/view/build-deepin-project/job/build-server-iso/), 制作完成的镜像可以访问[内网下载地址](http://10.1.10.72/deepin-cd/daily-build/kui/15/)

更多关于CI 编译自动化详见[自动编译系统详解](https://bj.git.sndu.cn/server-dev/engineering-docs-tools/blob/master/manuals/deepin-server-autobuild-system.md)


## 仓库列表

内部开发仓库,仅供系统开发人员使用，和测试验证BUG

```
内部开发仓库
deb http://10.1.10.21/server-dev kui main non-free contrib
deb http://10.1.10.21/server-dev kui-security main non-free contrib

内部稳定仓库
deb http://10.1.10.21/server-releases kui main non-free contrib
deb http://10.1.10.21/server-releases kui-security main non-free contrib

公网仓库
deb http://packages.deepin.com/deepin-server/ kui main non-free contrib
deb http://packages.deepin.com/deepin-server/ kui-security main non-free contrib

扩展仓库
deb [arch=amd64] http://10.1.10.21/server-dev kui-backports main non-free contrib
deb [arch=amd64] http://10.1.10.21/server-dev kui-openstack-mitaka main non-free contrib

龙芯版本仓库
deb http://bj.packages.sndu.cn/mips64el-dev/ 2014 main non-free
deb-src http://bj.packages.sndu.cn/mips64el-dev/ 2014 main non-free
```

##  公网ISO下载地址

```
http://dl.sndu.cn/
```

## 内部参考文档

* [debian-installer 详解](https://bj.git.sndu.cn/server-dev/engineering-docs-tools/blob/master/manuals/debian-installer-howto.md)
* [deepin-cd 详解]       (https://bj.git.sndu.cn/server-dev/engineering-docs-tools/blob/master/manuals/debian-cd-howto.md)
* [deb打包详解]          (https://bj.git.sndu.cn/server-dev/engineering-docs-tools/blob/master/manuals/build-deb-package.md)
* [自动编译系统详解]     (https://bj.git.sndu.cn/server-dev/engineering-docs-tools/blob/master/manuals/deepin-server-autobuild-system.md)

## 上游参考文档

* [Debian 新维护人员手册](https://www.debian.org/doc/manuals/maint-guide/)
* [Debian 开发者参考文档](https://www.debian.org/doc/devel-manuals#devref)
* [Debian 政策文档]      (https://www.debian.org/doc/debian-policy/)
* [Debian PKG] (https://wiki.debian.org/MaintainerScripts)

## 安全更新

https://www.debian.org/security/
https://www.debian.org/security/crossreferences

debian-security-announce@lists.debian.org
debian-security-announce-request@lists.debian.org
