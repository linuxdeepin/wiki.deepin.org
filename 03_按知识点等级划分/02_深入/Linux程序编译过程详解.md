---
title: Linux程序编译过程详解
description: 
published: true
date: 2022-11-09T07:55:57.406Z
tags: 编译
editor: markdown
dateCreated: 2022-11-09T07:55:57.406Z
---

# Linux程序编译过程详解
大家肯定都知道计算机程序设计语言通常分为机器语言、汇编语言和高级语言三类。高级语言需要通过翻译成机器语言才能执行，而翻译的方式分为两种，一种是编译型，另一种是解释型，因此我们基本上将高级语言分为两大类，一种是编译型语言，例如C，C++，Java，另一种是解释型语言，例如Python、Ruby、MATLAB 、JavaScript。

本文将介绍如何将高层的C/C++语言编写的程序转换成为处理器能够执行的二进制代码的过程，包括四个步骤：

预处理（Preprocessing）

编译（Compilation）

汇编（Assembly）

链接（Linking）