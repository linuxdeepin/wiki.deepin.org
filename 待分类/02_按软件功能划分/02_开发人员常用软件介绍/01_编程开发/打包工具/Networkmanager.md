---
title: Networkmanager
description: 
published: true
date: 2022-10-25T01:02:25.194Z
tags: 
editor: markdown
dateCreated: 2022-06-28T08:19:10.945Z
---

# 背景

不论是往某个软件里添加调试信息，还是重新编译都离不开“打包”。
因此，介绍一下打包network-manager方法(生成deb包)。
# 步骤
以network-manager软件包为例。

# 获取源码
```
$ apt source network-manager
备注：确保/etc/apt/sources.list 源被打开， 否则下载不了源码
1.配置系统中的仓库地址
sudo nano /etc/apt/source.list
加入
deb [trusted=yes] http://pools.corp.deepin.com/uos eagle main contrib non-free
deb-src [trusted=yes] http://pools.corp.deepin.com/uos eagle main contrib non-free
然后
sudo apt update


<pre>
$ ls
network-manager_1.14.6.4+c1-1+deepin.debian.tar.xz network-manager_1.14.6.4+c1-1+deepin.dsc network-manager_1.14.6.4+c1.orig.tar.xz network-manager-1.14.6.4+c1
```
# 修改代码
随便修改一个文件
```
如:
$ cd network-manager-1.14.6.4+c1/src/
$ vim xxx.c （修改文件）
$ wq 
保存
```

# 提交修改
```
$ cd network-manager-1.14.6.4+c1
$ dpkg-source --commit
会提示让你输入一个patch的名字，输入即可.
之后可以在 ls debian/patches/下面看到刚才生成的文件(patch)
debian/patches/series: 这个文件决定了，patch的应用顺序
```
# 安装依赖
这个安装一次就行了。
```
sudo apt build-dep network-manager-1.14.6.4+c1
这里可能安装失败，需要依赖其他文件，根据他的提示把相应文件安装好即可。
```
# 应用一下
```
$ dpkg-source -b .
```
# 重新编译
```
$ dpkg-buildpackage -us -uc -j8
编译过程遇到如下报错:
“error: ‘SIOCGSTAMP’ undeclared (first use in this function); did you mean ‘SIOCGARP’?”
tools/rctest.c
tools/l2test.c
侧在上述文件中增加头文件#include <linux/sockios.h>
参考:https://patchwork.kernel.org/project/qemu-devel/patch/20190617114005.24603-1-berrange@redhat.com/
修改完毕后重新提交：dpkg-source --commit 
如果遇到编译错误 执行下：debian/rules clean 即可
然后重新按照提交步骤一步步往后执行
```
# 生成文件
```
编译完成之后，在上一级目录生成deb包.
sudo dpkg -i xxx.deb 安装即可
```

