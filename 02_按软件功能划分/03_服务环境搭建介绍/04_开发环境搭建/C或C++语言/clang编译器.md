---
title: Clang
description: LLVM的Clang前端
published: true
date: 2022-10-25T05:57:11.582Z
tags: c, c++, objective c, objective c++, 编译器, clang, clang++
editor: markdown
dateCreated: 2022-06-09T15:47:24.747Z
---

# 简介

Clang 是一个用 C++ 编写、基于LLVM，支持（C、C++、Objective C/C++、OpenCL、CUDA和RenderScript）语言的编译器。提供了与GCC兼容clang编译器和与MSVC的cl.exe兼容的clang-cl.exe编译器。 

## 安装

`sudo apt-get install clang`
clang只是个空包，安装时会自动选择1个预设的版本来安装，也可以手工指定包名clang-x（x为大版本号）来安装对应的版本。
## 卸载

`sudo apt-get remove clang`

## 仓库地址

[http://packages.deepin.com/deepin/pool/main/l/llvm-defaults/](http://packages.deepin.com/deepin/pool/main/l/llvm-defaults/)

# 基于LibTooling的Clang工具

在这部分，将介绍各种可以处理C/C++程序的工具，这些工具都把Clang前端当作一个软件库来使用。

## clang-tidy工具

clang-tidy是基于Clang的代码检查工具，代码检查工具一般负责分析代码并找出编程风格不合规的部分。

### 用clang-tidy检查代码

要检查相关代码是否符合LLVM编码规范，可以使用如下命令：

```sh
clang-tidy -checks="llvm-*" file.cpp
```

其中file.cpp为需要检查的代码文件。

clang-tidy的命令行格式为：

```sh
clang-tidy [options] <source0> [... <sourceN>] [-- <compiler command>]
```

> 这部分工具只有安装Clang及其外部工具后才可以使用。



## 常见问题

## 相关链接
官方网站：https://clang.llvm.org/

维基百科：