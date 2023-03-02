---
title: 如何在deepin下调试coredump文件
description: 在deepin下当程序异常终止或崩溃时，如何对调试coredump文件
published: true
date: 2023-03-02T13:15:59.112Z
tags: 
editor: markdown
dateCreated: 2023-03-02T12:50:29.596Z
---

## 介绍
在现代操作系统中，Core Dump 指当程序异常终止或崩溃时，将程序此时的内存中的内容拷贝到磁盘文件中存储的技术。

## 检查当前的Core dump 策略
在终端中输入如下命令可以检查当前系统的Core dump策略。
```bash
ulimit -c
```
如果输出是0,说明默认是关闭core dump。在这种情况下，即使程序异常终止也不会生成core dump文件。

## 如何开启Core Dump

### 临时开启
在开始菜单中找到”终端“，打开终端。在终端内输入 ulimit -c
unlimited 。上面这个表示临时开启Core Dump，并且设置大小不受限。用上面的命令只会对当前的终端环境有效。

### 永久开启
下面有三种方法：
1. 修改 /etc/security/limits.conf 
在最后增加一行： * soft core unlimited 
2. 在 /etc/profile 中加入 ulimit -c unlimited
3. 在 ~/.bashrc 中添加 ulimit -c unlimited 。
然后执行 source ~/.bashrc 让修改立刻生效。

方法1和方法2需要重启系统，才能生效。方法3只对当前用户有效。

## 实际测试

### 编写产生错误的程序
这里提供一个错误的例子。
为了进行如下的操作请先安装 build-essential 和 gdb 。
在终端中输入以下指令：
```bash
sudo apt update && sudo apt install build-essential gdb
```

代码如下：
main.c
```c
#include <stdio.h>
int main()
{
    int a = 0;
    int b = 1;
    printf("%d",b/a);
    return 0;
}
```