---
title: Linux文件和目录操作命令
description: 
published: true
date: 2023-01-19T01:19:16.133Z
tags: 
editor: markdown
dateCreated: 2023-01-19T01:16:28.504Z
---

# Linux文件和目录操作命令
## **（1）pwd：显示当前所在位置**

**说明：**

pwd命令是“print working directory”中每个单词的首字母缩写，其功能是显示当前工作目录的绝对路径。在实际工作中，我们在命令行操作命令时，经常会在各个目录路径之间进行切换，此时可使用pwd命令快速查看当前我们所在的目录路径。

**语法**

pwd [option]

1.注意pwd命令和后面的选项之间至少要有一个空格。

2.通常情况下，执行pwd命令不需要带任何参数。

**参数**

> 

![2023-1-19_35432.png](/2023-1-19_35432.png)


PWD系统环境变量，可以用“$”符号输出其值，代码如下：

[root@iZ8vb11v8r15ng6q0eb8dzZ ~]# echo $PWD

/root

在Bash命令行显示当前用户的完整路径。

系统Bash命令行的提示符是由一个称为PS1的系统环境变量控制的。