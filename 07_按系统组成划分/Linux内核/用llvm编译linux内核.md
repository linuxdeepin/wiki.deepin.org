---
title: 用llvm编译linux内核
description: 
published: true
date: 2022-07-05T12:55:34.572Z
tags: linux内核 llvm
editor: markdown
dateCreated: 2022-07-05T12:55:31.996Z
---

# 用llvm编译linux内核

## 前言

llvm是一个编译器框架，据说比gcc好。本文带领大家编译一次linux内核。注意要llvm9才能默认支持编译内核。

## 编译步骤

整体构建的过程有三种：

1. llvm 目录包含 子项目
2. llvm 目录同级存在 子项目
3. llvm 和 子项目 独立编译

整体编译简单，独立编译易于独立控制，各有好处。

LLVM项目包含有很多小的子项目，他们之间的作用和依赖关系：

1. LLVM ：总的框架和工具链
2. clang ： 编译器
3. libc ： c标准库
4. libcxx ：c++标准库
5. libcxxabi ： c++ 标准库ABI（二进制兼容接口）
6. libunwind ：栈展开库
7. compiler-rt ： 编译器运行时
8. lld ： 链接器
9. lldb ： 调试器
10. clang-tools-extra ： 编译器扩展工具

编译器和库没有必然的绑定关系，c 语言标准库一般使用系统自带的GNU GCC libc，LLVM也提供了一个，但是隐藏的很深。而c++库就复杂得多，标准库有多个实现，GNU的libstdc++，然后可以选择依赖的二进制接口，GNU的libsupc++等。

## 编译LLVM

llvm官网只提供了linux版的源代码，而没有提供编译好的包，因此首先要自己编译一下最新的llvm代码。deepin系统自带最新版本是llvm-6.0版，就用这个来编译。

第一步 前期准备

```bash
sudo apt install llvm-6.0  #llvm框架
sudo apt install clang-6.0 #c/c++编译前端
sudo apt install lld-6.0 #链接器

#deepin的相关安装并没有产生默认的工具名字，因此要自己弄一个软链接
sudo ln -sr /usr/bin/clang-6.0 /usr/bin/clang #默认的c编译器
sudo ln -sr /usr/bin/clang++-6.0 /usr/bin/clang++ #默认的c++编译器
sudo ln -sr /usr/bin/lld-6.0 /usr/bin/lld #默认的lld编译器

#安装构建工具
sudo apt install cmake ninja-build 
```

第二步 下载源代码

- LLVM源代码 <http://releases.llvm.org/9.0.0/llvm-9.0.0.src.tar.xz> （[.sig](http://releases.llvm.org/9.0.0/llvm-9.0.0.src.tar.xz.sig)）
- clang源代码 <http://releases.llvm.org/9.0.0/cfe-9.0.0.src.tar.xz> （[.sig](http://releases.llvm.org/9.0.0/cfe-9.0.0.src.tar.xz.sig)）
- 编译器rt源代码 <http://releases.llvm.org/9.0.0/compiler-rt-9.0.0.src.tar.xz> （[.sig](http://releases.llvm.org/9.0.0/compiler-rt-9.0.0.src.tar.xz.sig)）
- libc++源代码 <http://releases.llvm.org/9.0.0/libcxx-9.0.0.src.tar.xz> （[.sig](http://releases.llvm.org/9.0.0/libcxx-9.0.0.src.tar.xz.sig)）
- libc++ abi源代码<http://releases.llvm.org/9.0.0/libcxxabi-9.0.0.src.tar.xz>  （[.sig](http://releases.llvm.org/9.0.0/libcxxabi-9.0.0.src.tar.xz.sig)）
- libunwind源代码<http://releases.llvm.org/9.0.0/libunwind-9.0.0.src.tar.xz>  （[.sig](http://releases.llvm.org/9.0.0/libunwind-9.0.0.src.tar.xz.sig)）
- LLD源代码<http://releases.llvm.org/9.0.0/lld-9.0.0.src.tar.xz> （[.sig](http://releases.llvm.org/9.0.0/lld-9.0.0.src.tar.xz.sig)）
- LLDB源代码 <http://releases.llvm.org/9.0.0/lldb-9.0.0.src.tar.xz> （[.sig](http://releases.llvm.org/9.0.0/lldb-9.0.0.src.tar.xz.sig)）
- OpenMP源代码<http://releases.llvm.org/9.0.0/openmp-9.0.0.src.tar.xz>  （[.sig](http://releases.llvm.org/9.0.0/openmp-9.0.0.src.tar.xz.sig)）
- Polly源代码 <http://releases.llvm.org/9.0.0/polly-9.0.0.src.tar.xz> （[.sig](http://releases.llvm.org/9.0.0/polly-9.0.0.src.tar.xz.sig)）
- clang-tools-extra <http://releases.llvm.org/9.0.0/clang-tools-extra-9.0.0.src.tar.xz>    （[.sig](http://releases.llvm.org/9.0.0/clang-tools-extra-9.0.0.src.tar.xz.sig)）
- LLVM测试套件 <http://releases.llvm.org/9.0.0/test-suite-9.0.0.src.tar.xz>（[.sig](http://releases.llvm.org/9.0.0/test-suite-9.0.0.src.tar.xz.sig)）

可以通过下载.sig文件来检验数据pgp签名：345AD05D ，公钥文件：<http://releases.llvm.org/9.0.0/hans-gpg-key.asc>

```bash
wget http://releases.llvm.org/9.0.0/hans-gpg-key.asc
pgp --import hans-gpg-key.asc #导入公钥

wget http://releases.llvm.org/9.0.0/llvm-9.0.0.src.tar.xz http://releases.llvm.org/9.0.0/llvm-9.0.0.src.tar.xz.sig
pgp --verify http://releases.llvm.org/9.0.0/llvm-9.0.0.src.tar.xz.sig #用sig文件验证签名
```

或者可以去清华大学开源镜像站下载： <https://mirrors.tuna.tsinghua.edu.cn/help/llvm/>

第三步 编译安装

将源代码压缩包解压后，注意不要在源代码文件夹内继续操作，而应该新建一个新的目录来构建。



### 构建llvm基础

目录：

- llvm/
- llvm-9.0.0.src/

llvm目录结构：

1. include
2. lib
3. projects 
   1. debuginfo-tests
   2. libcxx  ：c++标准库
   3. libcxxabi ： c++标准库二进制规范
   4. libunwind  ：
   5. lldb : 调试器
   6. compiler-rt ： 编译器运行时
   7. lld ： 链接器
   8. poly
4. test
5. tools    
   1. clang  ： 编译器
6. utils

cmake 一个生成其他构建工具项目的通用构建工具:

- `-G Ninja` cmake默认导出是make项目，速度极度慢，要好几个小时，导出Ninja项目可以缩短时间
- `-DCMAKE_BUILD_TYPE=Release` 构建发行版，大小约1G，默认debug版，大小达到惊人的20G！
- `-DLLVM_ENABLE_LLD=ON` 使用lld链接器
- `-DCMAKE_C_COMPILER=clang` 使用clang 做c编译器
- `-DCMAKE_CXX_COMPILER` 使用clang++ 做c++编译器
- `-DLLVM_TARGETS_TO_BUILD="ARM;X86;AMDGPU;RISCV;WebAssembly"` 指定后端编译架构
- `../llvm-9.0.0.src/` 源代码的目录
- `-DCMAKE_INSTALL_PREFIX` 最终文件安装的目录
- `-DLLVM_BUILD_LLVM_DYLIB=on` 动态链接库
- `-DLLVM_LINK_LLVM_DYLIB=on`
- `-DLLVM_ENABLE_RUNTIMS` 使用当前构建的工具
- `-DLLVM_DISTRIBUTION_COMPONENTS` 同时构建的组件
- `-DLLVM_TUNTIME_DISTRIBUTION_COMPONENTS`  运行时组件
- `-DLLVM_DYLIB_COMPONENTS`  动态库的组件
- `-DLLVM_INSTALL_TOOLCHAIN_ONLY`  不安装开发LLVM本身的相关工具


```bash
cd llvm

cmake -G Ninja -DCMAKE_BUILD_TYPE=Release -DLLVM_ENABLE_LLD=ON -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ -DLLVM_TARGETS_TO_BUILD="X86" -DCMAKE_INSTALL_PREFIX=~/.local -DLLVM_BUILD_LLVM_DYLIB=on -DLLVM_LINK_LLVM_DYLIB=on ../llvm-9.0.0.src/

#ninja 一个快速构建项目的工具
#用ninja构建,大概要半个小时以上
# -j 4 开启 4 线程编译
ninja -j 4 
sudo ninja install #安装构建完的工具
```

### cmake 变量

1. CMAKE_BUILD_TYPE： 构建类型，Release、Debug、RelWithDebInfo、MinSizeRel
2. CMAKE_INSTALL_PREFIX： 安装路径
3. LLVM_LIBDIR_SUFFIX： 库后缀，/usr/lib64
4. CMKAE_C_FLAGS：  C选项
5. CMAKE_CXX_FLAGS： C++选项
6. CMAKE_CXX_STANDARD： C++标准，14、17、20
7. LLVM_TAGETS_TO_BUILD： 平台目标，X86、PowerPC
8. LLVM_BUILD_TOOLS： 生成工具构建文件（默认），make llvm-ar 等
9. LLVM_INCLUDE_TOOLS： 包含工具
10. LLVM_INSTALL_CCTOOLS_SYMLINKS： 生成符号链接，lipo -> llvm-lipo 等
11. LLVM_BUILD_EXAMPLES： 生成示例构建文件（默认）
12. LLVM_INCLUDE_EXAMPLES： 包含示例
13. LLVM_INSTALL_BINUTILS_SYMLINKS： 生成工具的符号链接，ar -> llvm-ar 等
14. LLVM_BUILD_TESTS： 生成测试构建文件
15. LLVM_INCLUDE_BENCHMARKS： 包含基准测试
16. LLVM_APPEND_VC_REV： 嵌入版本修订
17. LLVM_BUILD_BENCHMARKS： 生成基准构建文件
18. LLVM_ENABLE_THREADS： 多线程（默认）
19. LLVM_ENABLE_UNWIND_TABLES：展开表（默认）
20. LLVM_ENABLE_ASSERTIONS：代码声明
21. LLVM_ENABLE_EH： 异常处理
22. LLVM_ENABLE_EXPENSIVE_CHECKS： 内存检查
23. LLVM_ENABLE_IDE： IDE支持
24. LLVM_ENABLE_PIC： -fPIC 标志（默认）
25. LLVM_ENABLE_RTTI： 运行时类型信息
26. LLVM_ENABLE_WARNINGS： 警告（默认）
27. LLVM_EMABLE_PEDANTIC： 学步模式（默认）
28. LLVM_ABI_BREAKING_CHECKS： ABI中断检查（默认）
29. LLVM_BUILD_32_BITS： 32位
30. LLVM_TARGET_ARCH： 目标体系结构，用于交叉编译
31. LLVM_TABLEGEN： llvm-tblgen,用于交叉编译
32. LLVM_LIT_ARGS： 
33. LLVM_LIT_TOOLS_DIR：
34. LLVM_ENABLE_FFI： 外部函数接口库libffi
35. LLVM_EXTERNAL_{CLANG,LLD,POLLY}_SOURCE_DIR： 外部项目的源代码目录，如clang、lld、polly工具的源代码目录
36. LLVM_ENABLE_PROJECTS： 同级目录的相关项目：clang、clang-tools-extra、compiler-rt、debuginfo-tests、libc、libclc、libcxx、libcxxabi、libunwind、lld、lldb、llgo、openmp、parallel-libs、polly、pstl
37. LLVM_EXTERNAL_PROJECTS： 同时构建的外部项目
38. LLVM_USE_OPROFILE： 启用OProfile JIT
39. LLVM_PROFDATA_FILE： -fprofile-instr-use=profdataDIR
40. LLVM_USE_INTEL_JITEVENTS: 启用Intel Events API
41. LLVM_ENABLE_LIBPFM:  使用libpfm计量（默认）
42. LLVM_USE_PERF： 启用Perf JIT
43. LLVM_ENABLE_ZLIB： 启用压缩（默认）
44. LLVM_ENABLE_DIA_SDK： 支持MSVC DIA SDK（默认）
45. LLVM_USE_SANITIZER： 清理程序
46. LLVM_ENABLE_LTO： 启用连接时间优化 -flto
47. LLVM_USE_LINKER： 链接器，搜索优化 lld -> ld.lld
48. LLVM_ENABLE_LIBCXX: 启用 -stdlib=libc++,而非默认的stdlibc++库
49. LLVM_STATIC_LINK_CXX_STDLIB： 静态链接c++标准库
50. LLVM_ENABLE_LLD： 启用ld.lld链接器
51. LLVM_PARALLEL_COMPILE_JOBS： 并行编译线程数
52. LLVM_PARALLEL_LINK_JOBS： 并行链接线程数
53. LLVM_BUILD_DOCS： 生成文档构建文件，doxygen和sphinx
54. LLVM_ENABLE_DOXYGEN： 
55. LLVM_ENABLE_DOXYGEN_QT_HELP
56. LLVM_DOXYGEN_QCH_FILENAME
57. LLVM_DOXYGEN_QHP_NAMESPACE
58. LLVM_DOXYGEN_QHP_CUST_FILTER_NAME
59. LLVM_DOXYGEN_QHELPGENERATOR_PATH
60. LLVM_DOXYGEN_SVG
61. LLVM_INSTALL_DOXYGEN_HTML_DIR
62. LLVM_ENABLE_SPHINX
63. SPHINX_EXECUTABLE
64. SPHINX_OUTPUT_MAN
65. SPHINX_OUTPUT_HTML
66. SPHINX_WARNINGS_AS_ERRORS
67. LLVM_INSTALL_SPHINX_HTML_DIR
68. LLVM_INSTALL_OCAMLDOC_HTML_DIR
69. LLVM_CREATE_XCODE_TOOLCHAIN： 支持macOS xcode
70. LLVM_BUILD_LLVM_DYLIB：  构建共享库
71. LLVM_LINK_LLVM_DYLIB： 将工具链接到共享库
72. BUILD_SHARED_LIBS： （不建议）
73. LLVM_OPTIMIZED_TABLEGEN： 优化调试构建时间
74. LLVM_REVERSE_ITERATION： 逆序无序容器
75. LLVM_BUILD_INSTRUMENTED_COVERAGE： 代码覆盖测试
76. LLVM_CCACHE_BUILD： 缓冲LLVM，加速重建LLVM
77. LLVM_FORCE_USE_OLD_TOOLCHAIN： 不检查编译器和标准库版本
78. LLVM_TEMPORARILY_ALLOW_OLD_TOOLCHAIN： 旧版本工具只是警告
79. LLVM_USE_NEWPM：启用新通行证管理器
80. LLVM_ENABLE_BINDINGS： OCaml绑定
81. LLVM_ENABLE_Z3_SOLVER： 启用clang静态分析器Z3约束求解器
82. LLVM_INCLUDE_TESTS : 包含测试
83. LLVM_ENABLE_WERROR: 警告即停止编译
84. LLVM_ENABLE_RUNTIMES： 使用刚构建的运行库，libcxx、compiler-rt、libcxxabi、libunwind 等
85. LLVM_DISTRIBUTION_COMPONENTS： 构建运行时组件
86. LLVM_DYLIB_COMPONENTS： 动态链接库的组件
87. LLVM_INSTALL_TOOLCHAIN_ONLY： 只安装工具链
88. PYTHON_EXECUTABLE： 强制指定python

## 理清各库文件的关系

程序依赖公共的库文件，因此要厘清他们之间的关系，这样才不会陷入依赖陷阱。

| 名称        | 典型文件                            | 项目组 | 说明                                       |
| ----------- | ----------------------------------- | ------ | ------------------------------------------ |
| compiler-rt | `libclang_rt.builtins.<arch>.a`     | LLVM   | 编译器运行时，支持非原生的硬件操作、原子库 |
| libunwind   | libunwind.a                         | LLVM   | unwind 栈异常展开                          |
| libgcc_s    | libgcc_s.so.1                       | GNU    | 编译器运行时、Unwind ABI                   |
| libatomic   |                                     | GNU    | 原子库                                     |
| sanitizer   | `libclang_rt.<sainitizer>.<arch>.a` | LLVM   | 边界检测                                   |
| libc++ abi  | libc++abi.a                         | LLVM   | c++ ABI                                    |
| libsupc++   |                                     | GNU    | c++ ABI                                    |
| libc++      | libc++.so                           | LLVM   | c++标准库                                  |
| libstdc++   | libstdc++.so.6                      | GNU    | c++标准库                                  |
| libc        | libc.so.6                           | GNU    | c标准库                                    |

LLVM依赖关系如下：

1. libc++ -> libc++abi -> libunwind
2. libc
3. compiler-rt

GNU依赖如下：

1. -> libstdc++ ->libsupc++ -> libgcc_s
2. -> libc


### 构建 clang

- clang/
- cfe-9.0.0.src/

```bash
cd clang
# -DLLVM_PATH=../llvm-9.0.0.src/ 指定LLVM目录
cmake -G Ninja -DCMAKE_BUILD_TYPE=Release -DLLVM_ENABLE_LLD=ON -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ -DLLVM_PATH=../llvm-9.0.0.src/ ../cfe-9.0.0.src/

ninja
sudo ninja install
```

### 构建 clang-tools-extra

```bash

```

### 构建 lld

- lld/
- lld-9.0.0.src/

```bash
cmake -G Ninja -DCMAKE_BUILD_TYPE=Release -DLLVM_ENABLE_LLD=ON -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ -DLLVM_PATH=../llvm-9.0.0.src/ ../lld-9.0.0.src/

ninja
sudo ninja install
```

### 构建 lldb

- lldb/
- lldb-9.0.0.src/

可选参数：

- CMAKE_C_FLAGS   # 提供c编译参数字符串
- CMAKE_CXX_FLAGS  # 提供c++编译参数字符串
- CMAKE_EXE_LINKER_FLAGS # 提供链接器参数字符串

```bash
# 这个要求
cmake -G Ninja  -DCMAKE_BUILD_TYPE=Release  -DCMAKE_EXE_LINKER_FLAGS="-Wl,--rpath=/opt/glibc/lib -Wl,--dynamic-linker=/opt/glibc/lib/ld-linux-x86-64.so.2" -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++  -DCMAKE_LINKER=$(which ld.lld) -DLLVM_ENABLE_LLD=ON  ../lldb-9.0.0.src/

ninja
sudo ninja install
```

### 构建 compiler-rt

- compiler-rt/
- compiler-9.0.0.src/

可选参数：

- LLVM_BUILD_LLVM_DYLIB  #目标动态库
- LLVM_LINK_LLVM_DYLIB   #链接动态库
- LLVM_TARGETS_TO_BUILD  #后端目标架构 
- LLVM_ENABLE_PROJECTS  #组件
- LLVM_ENABLE_RUNTIMES  #运行时
- LLVM_DISTRIBUTION_COMPONENTS  #组件选件
- LLVM_RUNTIME_DISTRIBUTION_COMPONENTS  #运行时选件
- LLVM_DYLIB_COMPONENTS #动态库选件
- LLVM_INSTALL_TOOLCHAIN_ONLY  #辅助工具


```bash
# 编译这个感觉比较奇怪，要求在上级目录有名为libcxx和libcxxabi的源文件
ln -sr libcxxabi-9.0.0.src/ libcxxabi
ln -sr libcxx-9.0.0.src/ libcxx
cd complier-rt

cmake -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_LINKER=$(which ld.lld) -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++  ../compiler-rt-9.0.0.src/ 

ninja
sudo ninja install
rm libcxxabi libcxx
```

### 构建 libunwind:

- libunwind/
- libunwind-9.0.0.src/

```bash
cmake -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_LINKER=lld -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++  ../libunwind-9.0.0.src/

ninja
sudo ninja install
```

### 构建 libcxxabi

- libcxxabi
- libcxxabi-9.0.0.src/

```bash
cmake -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_LINKER=lld -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ -DLIBCXXABI_LIBCXX_INCLUDES=../libcxx-9.0.0.src/include/ ../libcxxabi-9.0.0.src/

ninja
sudo ninja install
```

### 构建 libcxx

- libcxx
- 

```bash
cmake -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_LINKER=lld -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++  ../libcxx-9.0.0.src/

ninja
sudo ninja install
```

## 整体编译

将所有子项目下载下来，放在llvm 同级目录中。
比如：

clang、compiler-rt、libcxxabi、lldb、clang-tools-extra 、libcxx、lld、llvm、libunwind、libc

选项说明：

- `-DLIBCXX_USE_COMPILER_RT=YES` libcxx使用compiler-rt而不是libgcc_s
- `-DLIBCXXABI_USE_COMPILER_RT=YES` libcxxabi 同上
- `-DLLVM_ENABLE_PROJECTS="libunwind;clang;compiler-rt;debuginfo-tests;libclc;llgo;mlir;parallel-libs;pstl;openmp;polly;libcxxabi;clang-tools-extra;libcxx;lld;lldb;libc"` 同时编译子项目
- `-DLLVM_ENABLE_LIBCXX=on` 使用libc++而不是libstdc++
- `-DLIBCXX_CXX_ABI=libcxxabi` 使用libcxxabi，而不是其他 libsupc++ 等
- `-DLIBCXXABI_USE_LLVM_UNWINDER=on` 使用libunwind
- `-DLLVM_ENABLE_RUNTIMS` 使用当前构建的工具
- `-G Ninja` cmake导出Ninja项目可以缩短时间
- `-DCMAKE_BUILD_TYPE=Release` 构建可发行版
- `-DLLVM_ENABLE_LLD=ON` 使用lld链接器
- `-DCMAKE_C_COMPILER=clang` 使用clang 做c编译器
- `-DCMAKE_CXX_COMPILER=clang++` 使用clang++ 做c++编译器
- `-DLLVM_TARGETS_TO_BUILD="host"` 指定后端编译架构
- `-DCMAKE_INSTALL_PREFIX=~/.local` 最终文件安装的目录
- `-DLLVM_INSTALL_TOOLCHAIN_ONLY=on`  不安装开发LLVM本身的相关工具
- `-DCLANG_ENABLE_BOOTSTRAP=on` 二次编译（生成当前版本，然后用当前版本再次编译）
- `-DLLVM_PARALLEL_COMPILE_JOBS=4`： 并行编译线程数
- `-DLLVM_PARALLEL_LINK_JOBS=4`： 并行链接线程数

```bash
cmake -G Ninja -DLLVM_PARALLEL_COMPILE_JOBS=4 -DLLVM_PARALLEL_LINK_JOBS=4 -DLLVM_TARGETS_TO_BUILD="host" -DCMAKE_BUILD_TYPE=Release -DLLVM_ENABLE_LLD=ON -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ -DLLVM_ENABLE_LIBCXX=on -DLIBCXX_CXX_ABI=libcxxabi -DLLVM_INSTALL_TOOLCHAIN_ONLY=on -DLLVM_ENABLE_PROJECTS="libunwind;clang;compiler-rt;debuginfo-tests;libclc;llgo;mlir;parallel-libs;pstl;openmp;polly;libcxxabi;clang-tools-extra;libcxx;lld;lldb;libc" -DCMAKE_INSTALL_PREFIX=~/.local ../llvm-project/llvm

cmake --build .

```

## 让动态链接库生效

动态链接库由ldconfig 配置，默认跟踪:

- /usr/lib
- /lib

可以通过以下配置增加目录：

- /etc/ld.so.conf
- /etc/ld.so.conf.d/*

`sudo ldconfig`  更新动态链接库缓存

环境变量搜索路径：

1. -I
2. C_INCLUDE_PATH
3. CPLUS_INCLUDE_PATH
4. OBJC_INCLUDE_PATH
5. LD_LIBRARY_PATH

构建
同样方法，可以构建其他工具：clang，complier-rt；
lld需要使用-DLLVM_ENABLE_LLD=OFF,lldb需要先安装[swig4](https://sourceforge.net/projects/swig/)、libeidt-dev、libncurses5-dev、build-essential、正确的python版本（我在最后链接阶段失败了）。


## 不想编译可以直接用apt包

```bash
# apt源秘钥
wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key|sudo apt-key add -
```

apt source:

```bash
deb http://apt.llvm.org/buster llvm-toolchain-stretch main
deb-src http://apt.llvm.org/buster llvm-toolchain-stretch main
```

## 内核编译 

cc命令是个链接，链接到gcc，改成clang。c++命令同样如此，改成clang++。然后编译内核即可。

```bash
sudo make CC=clang xconfig #CC指定编译器设置编译配置
make deb-pkg CC=clang -j4
```

### 更详细的设置

事实上，以上方法只是修改了环境参数CC指向clang，即llvm的c编译器，编译系统可以识别这个参数。另外还可以精细的设置相关环境变量：

1. CC=clang : c编译器
2. CXX=clang++ : c++编译器
3. AS=llvm-as : 汇编器
4. LD=ld.lld : 链接器
5. CPP= : 这不是c++的简写，而是C preprocessor（c语言预处理器）
6. AR=llvm-ar : 存档编辑器
7. NM=llvm-nm ：符号表
8. STRIP=llvm-strip :对象剥离
9. OBJCOPY=llvm-objcopy : 对象复制
10. OBJDUMP=llvm-objdump : 对象转储
11. OBJSIZE=llvm-size : 大小信息

如果是debian的.config配置，最好是把安全启动给关闭，因为后续安装其他模块和工具会比较麻烦。

```bash

```


## 参考

1. linux 添加动态链接库路径: <https://blog.csdn.net/liu0808/article/details/79012187>
2. llvm中文： <https://llvm.comptechs.cn/>
3. linux 头文件以及库的路径: <https://blog.csdn.net/longxj04/article/details/9118891>
4. LLVM cross-compiled Linux From Scratch： <https://12101111.github.io/>

