---
title: 使用xmake构建LFS系统
description: 
published: true
date: 2022-07-06T02:31:37.144Z
tags: 
editor: markdown
dateCreated: 2022-07-06T02:31:34.597Z
---

# 使用 xmake 构建 LFS 系统

## 前言

这一个项目的目的是使用国产的构建工具 xmake，从头构建起 LFS。所谓 LFS 是让用户构建自己的 linux 发行版。

对的，你可以用市面上存在的 linux 发行版，如 ubuntu、debian、gentoo、centos、fedora、redhat、deepin、uos、arch等等，也能自己从头定义一个 linux。毕竟 linux 和其组件大部分都是开源或者可以免费得到的。

不使用每个组件自身的构建系统，而是使用 xmake 来构建他们。这样有什么好处？我希望得到一个统一的、和谐的构建系统。将来用户不需要费劲心思去学习不同构建系统之间的差异，而只是使用同一个易用的 xmake 作为构建界面。

前提是我能完成这个项目。

## 背景知识

### 从源代码开始安装软件

包的种类：

1. 源代码包：让用户根据自己的硬件环境优化构建全新的可执行软件
2. 预编译二进制包：让用户免于编译的等待，但优化可能是通用的，没有针对性，还有不同组件因为编译方式不同导致的兼容问题。

存档格式：

1. tar、zip、gzip等

包里面的关键文件：

1. README: 帮助
2. INSTALL： 安装文档
3. makefile: make 构建工具的脚本
4. configure: configure 配置工具的脚本

常用工具：

1. sed、awk : 文本匹配修改工具
2. diff、patch： 打补丁工具
3. configure、make、cmake： 构建工具
4. perl、python： 依赖的运行组件
5. curl、wget： 下载工具
6. md5sum、sha256sum：计算散列值的工具（验证文档的完整性）

通常的安装流程：

```bash
# 下载源码包
curl -O http://xxx.org/xxx.tar
# 查看散列值是否和官网给出的一致
md5sum xxx.tar
# 解压缩
tar -xavf xxx.tar
cd xxx
# 查看帮助文档
less README
# 配置 makefile
./configure --prefix=/usr
# 构建
make
# 安装
sudo make install
# 清理临时文件
make clean
```

### c 语言机制

多数开源软件都是 c 语言或 c++ 语言开发的。在 c 语言中，程序可以分成多个源文件（`*.c`），如果要使用另一个源文件的内容怎么半？不需要复制，只需要添加对应的头文件(`*.h`)。 

这种方式叫静态链接。很多时候一个组件会被多个应用程序使用，如果都静态链接，会增大程序容量，这时候可以动态链接，即告之操作系统，我这个程序要用到那个组件，操作系统执行的时候再去把相关组件加载进来，而不需要一开始就组合到应用程序内部。

知识点：

1. 头文件：类似源文件的说明文件
2. 源文件：程序的逻辑文本，使用头文件来调用其他源文件的内容
3. 编译：将源文件编译成二进制的格式，即对象文件(object file) （`*.o`)
4. 链接：将多个对象文件组合起来形成一个可执行的应用程序（或库）
5. 动态链接：只记录关联组件的信息，不包含相关组件的内容
6. 静态库： 多个对象文件合并成一个中间库 (`*.a`)
7. 动态库： 用来动态链接的中间库（`*.so`)
8. 程序： 若干个对象文件 + 静态库 + 动态库 链接成程序 (`*.out`)。 

核心工具：

1. 预处理器： 将源代码做简单处理，
   1. .h -> .ads / .adb
   2. .c -> .i 
   3. .cxx -> .ii 
2. 编译器： gcc 将源代码转换为汇编代码： .i -> .s
3. 汇编器： as 将汇编代码转换为对象文件： .s -> .o
4. 归档器： ar 将对象文件合并成静态库: .o -> .a
5. 链接器： ld 将对象文件、静态库等组合为程序: .o|.a|.so -> .out

gcc 是一个智能工具，它可以调用其他工具来完成整个编译流程，因此实际使用中，用户不需要分别调用不同的工具来完成这个流程。

```bash
# 演示生成步骤
# test.h test.c main.c
# 预编译
# -o 表示输出
gcc -E test.c -o test.i # -> test.i
gcc -E main.c -o main.i # -> main.i
# 编译
gcc -S main.i # -> main.s
gcc -S test.i # -> test.s
# 汇编
gcc -c test.s # -> test.o
as main.s # -> test.o
# 归档（静态库）
# -rc 创建新的库 libtest.a
ar -rc libtest.a test.o
# 动态库
gcc -shared test.o -o libtest.so
# 使用静态库构建程序
# -L. 库的路径 . 当前目录
# -l 库的名字 libname.a 或 libname.so(自动添加其余部分)
gcc main.o -o stapp -static -L. -ltest
# 或者
gcc main.o libtest.a -o stapp
# 使用动态库
gcc main.o -o dyapp -L. -ltest

# 手动链接比较麻烦
# 1.o i.o n.o 是 c 语言程序的初始部分
# -I 是动态链接器的位置，虽然 libtest.a 是静态链接，但是 libc.so 是动态链接，每个 c 语言程序都要链接到基本的 c 库。
# -lc 就是 libc 库
ld main.o libtest.a /usr/lib/crt{1.o,i.o,n.o} -I /lib64/ld-linux-x86-64.so.2 -lc -o stapp
# 或静态链接到 c 库
ld main.o libtest.a /usr/lib/crt{1.o,i.o,n.o} -static -lgcc -lc -lgcc_eh -lc -L /usr/lib/gcc/x86_64-pc-linux-gnu/11.1.0/ -o stapp

# 运行动态链接的程序
# LD_LIBRARY_PATH 环境变量告诉动态链接器，该从哪里查找动态库 libtest.so
LD_LIBRARY_PATH=. ./dyapp
```

其他工具：

1. addr2line: 将源代码对应的行记录下来
2. ldd： 查看程序的动态库
3. objcopy： 转换不同格式的对象文件，如：bin -> elf
4. nlmconv: 将对象文件输出为 NLM 格式。
5. objdump:  对象文件逆反为汇编文件。
6. readelf: 查看对象文件（elf格式）的信息。
7. size： 列出程序的各段落的大小情况。
8. gasp: 汇编宏处理器
9. ranlib: 静态库索引
10. strings： 检索文件中的字符串
11. strip: 移除符号
12. gprof： 性能分析
13. c++filt: 解码c++符号
14. dlltool: 创建 windows 动态库
15. nm： 检索对象文件的符号信息
16. windmc： windows 消息资源
17. windres: windows 资源文档编译器

核心库：

1. libc: c 语言官方提供的核心库，也就是说只要是 c 语言开发的程序，都包含这个基础库。这个库有多个提供方，glibc 由 GNU 提供。
2. libstdc++: c++ 语言的基础库。


elf 格式：

1. .text: 指令段
2. .data: 数据段
3. .bss: 为初始化数据段
4. .fini: 析构函数段
5. .init: 构造函数段
6. .rodata: 只读数据
7. .debug: 调试符号表

动态链接器：

1. /lib64/ld-linux-x86-64.so.2 : 64 位动态链接器，当前名字（将来也许会变）
2. /lib/ld-linux.so.2: 32 为动态链接器，当前名字（将来也许会变）
3. 搜索目录
   1. /etc/ld.so.conf : 配置搜索的目录
      1. /etc/ld.so.conf.d
   2. /usr/lib
   3. /lib
   4. LD_LIBRARY_PATH : 配置目录的环境变量
   5. 编译时写入程序，参数： rpath=路径。

交叉编译：

所谓交叉编译是指在当前平台上编译一个能在目标平台（和当前平台不同的）上运行的程序。支持这种编译方式的编译器，叫交叉编译器。

在交叉编译器（注意是针对编译器）这个领域，有一些专业术语需要学习：

1. build：构建平台
2. host：运行平台
3. target：输出目标的平台

比如你编译一个 gcc， 构建的平台就是 build。 这个 gcc 将来能在那种机器上运行，这个运行平台就是 host。 最后，这个 gcc 能够构建在什么机器上运行的程序，这个就是 target。

可见，如果不是编译器，那 target 就是没啥意义的。一般程序你可以理解 build 就是你本机，host 就是目标机器。 也可以认为 target 等于 host。

根据三者的组合，可以得到：

1. build = host = target : 普通编译（本机编译器）
2. build != host = target : 交叉编译一个普通编译器
3. build = host != target: 普通编译一个交叉编译器
4. build != host != target : 交叉编译一个交叉编译器

接下来，我会使用第四种方案，交叉编译一个交叉编译器，设置是： build != host 但 target = build 。

目的是：

1. 构建一个独立于当前环境的编译器 A (build= p1, host=p2, target=p1)
2. 用 A 构建基础的必要的软件包
3. 以此为基础，构建基于新系统的编译器 B (build=p2, host=p1, target=p1)
4. 用 B 构建完整的新系统。

### shell

bash 的启动方式：

1. 登录模式： /bin/login 读取 /etc/passwd 登录，并读取 /etc/profile 和 ~/.bash_profile 或 ~/.profile，退出时读取 ~/.bash_logout 
   1. /etc/issue : 登录时的欢迎信息
2. 交互模式： /bin/bash 或 /bin/su 启动交互式 shell，复制父环境，读取 ~/.bashrc
3. 非交互式（脚本）： 复制父环境

### LSB 标准

核心包：bash、bc、binutils、coreutils、diffutils、file、findutils、gawk、grep、gzip、m4、man-db、ncurses、procps、psmisc、sed、shadow、tar、util-linux、zlib；扩展：at、batch、cpio、ed、fcrontab、lsb-tools、nspr、nss、pam、pax、sendmail、time

运行时： perl；扩展：python、libxml2、libxslt

桌面：alsa、atk、cairo、desktop-file-utils、freetype、fontconfig、gdk-pixbuf、glib2、gtk+2、icon-naming-utils、libjpeg-turbo、libpng、libtiff、libxml2、mesalib、pango、xdg-utils、xrog；扩展：qt5、gtk+3

成像：cups、cups-filters、chostscript、sane

### 包简介

1. acl ：管理访问控制列表，即文件的权限
2. attr：扩展权限
3. autoconf：配置构建
4. automake：配置构建
5. bash：通用 shell
6. bc：计算器
7. binutils：二进制实用工具包
8. bison：yacc 编译器
9. bzip2： bzip 压缩
10. check： 测试
11. coreutils：操作文件和目录的基本工具
12. dejagnu： 测试
13. diffutils：显示文件差异
14. e2fsprogs：ext4 系列文件系统工具
15. eudev：设备管理器
16. expat：xml 解析库
17. file： 文件类型信息
18. findutils：查找文件
19. flex：lex 词法分析器
20. gawk：awk 文本处理
21. gcc：编译器
22. gdbm：数据库
23. gettext：文本本地化
24. glibc：c 标准库
25. gmp：数学库
26. gperf：生成散列程序
27. grep：搜索文件内容
28. groff：格式化文本
29. grub：引导系统
30. gzip：gzip 压缩
31. lana-etc：网络服务
32. inetutils：网络管理
33. intitool：源文件提取翻译字符串
34. iproute2：网络工具包
35. kbd：键盘键位映射
36. kmod：内核模块
37. less：文本阅读
38. libcap：用户权限
39. libelf：elf 库
40. libffi： 解释器接口
41. libpipeline：子进程管道
42. libtool：通用库脚本
43. linux kernel：linux 系统内核
44. m4：文本宏处理器
45. make：构建工具
46. man-db：帮助手册工具
47. man-pages：手册文档
48. meson：构建工具
49. mpc：复数库
50. mpfr：多精度算术库
51. ninja：构建工具
52. ncurses：终端字符屏幕库
53. openssl：密码学工具
54. patch：打补丁工具
55. perl：perl 语言解释器
56. pkg-config：包配置
57. procps-ng：监视进程
58. psmisc：进程信息
59. python3：python 语言解释器
60. readline：命令行编辑和历史
61. sed：文本处理程序
62. shadow：管理用户密码
63. sysklogd：系统日志
64. sysvinit：后台管理系统
65. tar：打包文件
66. tcl：测试工具
67. texinfo：信息转换
68. util-linux：linux 工具包
69. vim：文本编辑器
70. xmlparser：perl xml 解析接口
71. xzutils：xz 压缩
72. zlib： z 压缩
73. zstd： 压缩

## 开始

首先构建交叉编译器和相关库。

环境变量定义：

1. P1 = x86_64-pc-linux-gun ：本机
2. P2 = x86_64-lfs-linux-gun ：中间平台
3. LFS = ~/lfs/rootfs : 新系统的根目录
4. P2ROOT = ~/lfs/crosstools : 中间平台根目录

和交叉编译系统有关的包：

1. binutils : 汇编器，链接器等二进制工具
2. gcc: 编译器
   1. mpfr：多精度算术库
   2. gmp：数学库
3. glibc： c 库
4. glibc++: c++ 库

### binutils

xmake 支持调用第三方构建系统来构建，这样的好处就是最大的兼容现有的项目，同时可以有效的利用 xmake 的设置系统，这比第三方的设置要简易不少。

参数解析：

1. f ： xmake 的设置系统
2. --trybuild : 表示使用的第三方构建器，autotools 是 binutils 项目配置好脚本的工具。
3. --tryconfigs: 表示要传递给第三方构建器的参数，它的值就是接受 lfs 指南的相关参数。
4. --prefix: 是安装目录
5. --with-sysroot: 是系统目录，这会指导 binutils 的工具在该目录下搜索相关资源。（而不会在本机上搜索）
6. --target： 即之前概念 build、host、target中的target。因为 binutils 也属于编译器的范畴，它也支持交叉编译。
7. disable-nls: 关闭本地化
8. disabel-werror: 关闭警告

```bash
# binutils
curl -O https://ftp.gnu.org/gnu/binutils/binutils-2.36.1.tar.xz
tar -xavf binutils-2.36.1.tar.xz
cd binutils-2.36.1
xmake f --trybuild=autotools --tryconfigs="--prefix=$P2ROOT/usr/       \
             --with-sysroot=$P2ROOT        \
             --target=$P2          \
             --disable-nls              \
             --disable-werror"
xmake
```

下载相关包的香港镜像： <https://mirror-hk.koddos.net/lfs/lfs-packages/10.1/>

后续就不介绍下载和解压缩了，直接从编译开始。

### gcc

gcc 依赖 mpfr 和 mpc，需要在 gcc 目录下创建两个符号链接 mpfr 和 mpc 指向他们的源代码（或者也可直接复制到这个目录）。

需要注意，gcc 需要在源文件ls目录外部构建，这种模式叫源外构建，lfs 大多数项目都应该采用源外构建的方式进行。

在此回顾交叉编译的细节：

我们想要的效果是建立一个独立的编译系统 C2，它运行在 P2 平台，然后通过 C2 交叉编译在 P1 平台运行的编译系统 C3。

那么首先得用本机 P1 上的已有编译器 C0 来构造 C2.

构造详细的流程是：

1. C0 -> C1 (build=P1, host=P1, target=P2) 
   1. C1 是 p1->p2 的交叉编译器
2. C1 -> C2 (build=P1, host=P2, target=P1)
   1. C2 是 p2->p1 的交叉编译器
3. C2 -> C3 (build=P2, host=P1, target=P1)
   1. C3 是 p1->p1 的普通编译器
4. C3 -> C4 (build=P1, host=P1, target=P1)
   1. C4 是在新平台上重构的普通编译器

注意 C1 这个中间的编译器是需要的，因为本机上没有 p1 -> p2 的交叉编译器，因此你不能直接从 C0 -> C2，这是交叉编译。

C4 和 C3 都能正常工作，但是构建 C3 时，没有在构建时验证 C3，因为它是交叉编译的。它的验证工作要在目标机上执行。

```bash
# 创建符号链接
ln -s ../mpfr-4.1.0/ mpfr 
ln -s ../gmp-6.2.1/ gmp 

cd .. # 退出 gcc 的源文件目录
xmake f --trybuild=autotools --tryconfigs="--target=$P2                              \
    --prefix=$P2ROOT/usr                            \
    --with-glibc-version=2.11                      \
    --with-sysroot=$P2ROOT                            \
    --with-newlib                                  \
    --without-headers                              \
    --enable-initfini-array                        \
    --disable-nls                                  \
    --disable-shared                               \
    --disable-multilib                             \
    --disable-decimal-float                        \
    --disable-threads                              \
    --disable-libatomic                            \
    --disable-libgomp                              \
    --disable-libquadmath                          \
    --disable-libssp                               \
    --disable-libvtv                               \
    --disable-libstdcxx                            \
    --enable-languages=c,c++"
```

### linux header

glibc 依赖 linux header（系统头文件）。

```bash
make headers
make headers_install INSTALL_HDR_PATH=$P2ROOT/usr
```

### glibc

注意 glibc 不是编译器，它是库，也就是目标程序的一部分。所以它的 host 不是 P1 ，而是 P2。 

glibc 的路径是相对 P2 根目录的。

因为这将进行一次交叉编译，即 p1->p2，所以路径里面要提供我们新构造的 C1 编译器。

```bash
PATH=$PATH:$P2ROOT/usr/bin ../glibc-2.33/configure                             \
      --prefix=$P2ROOT/usr                      \
      --host=$P2                   \
      --build=$P1 \
      --enable-kernel=3.2                \
      --with-headers=$P2ROOT/usr/include  
PATH=$PATH:$P2ROOT/usr/bin make 
PATH=$PATH:$P2ROOT/usr/bin make install # 安装到相对位置DESTDIR=$P2ROOT

# 修复头文件
$P2ROOT/usr/libexec/gcc/x86_64-lfs-linux-gnu/10.2.0/install-tools/mkheaders 
```

### libstdc++

c++ 标准库。类似 c 库的做法。

libstdc++ 属于 gcc 源码包的一部分，在里面找到 libstdc++-v3 目录。

```bash
PATH=$PATH:$P2ROOT/usr/bin ../gcc-10.2.0/libstdc++-v3/configure \
          --host=$P2                 \
          --build=$P1      \
          --prefix=$P2ROOT/usr                   \
          --disable-multilib              \
          --disable-nls                   \
          --disable-libstdcxx-pch         \
          --with-gxx-include-dir=$P2ROOT/usr/$P2/include/c++/10.2.0
PATH=$PATH:$P2ROOT/usr/bin make 
PATH=$PATH:$P2ROOT/usr/bin make install
```

至此，完成了交叉编译器 C1 （build=P1, host=P1, target=P2)。

## 完成 C2

C1 是一个简化版编译器。

它其实还没能脱离宿主机（即本机 P1），构建要用到的工具，也还是本机上的，我们想构建完全独立的 P2 环境，就要使用 C1 交叉编译这些基本的工具。

基本工具包：M4、Ncurses、bash、coreutils、diffutils、file、findutils、gawk、grep、gzip、make、patch、sed、tar、xz。

准备好之后，构建 C2 （build=P1, host=P2, target=P1) 的编译器包：binutils、gcc、glibc、stdc++。

### M4

todo： gcc pass 1 没有完成结尾的修复头文件任务。。
<https://www.linuxfromscratch.org/lfs/view/stable/chapter06/m4.html>
宏处理器。

```bash
# 修复和 glibc 2.28 以上版本的兼容性
sed -i 's/IO_ftrylockfile/IO_EOF_SEEN/' lib/*.c
echo "#define _IO_IN_BACKUP 0x100" >> lib/stdio-impl.h

PATH=$P2ROOT/usr/bin:$PATH ../m4-1.4.18/configure \
            --prefix=$P2ROOT/usr   \
            --host=$P2 \
            --build=$P1
PATH=$P2ROOT/usr/bin:$PATH make
PATH=$P2ROOT/usr/bin:$PATH make install      
```

## 参考

1. 为 Linux 构建和安装软件包: <https://tldp.org/HOWTO/Software-Building-HOWTO.html>
2. 从源代码安装的初学者指南: <http://moi.vonos.net/linux/beginners-installing-from-source/>
3. 源码包的香港镜像： <https://mirror-hk.koddos.net/lfs/lfs-packages/10.1/>
4. 源码包的官方地址： <https://www.linuxfromscratch.org/lfs/view/stable/chapter03/packages.html>