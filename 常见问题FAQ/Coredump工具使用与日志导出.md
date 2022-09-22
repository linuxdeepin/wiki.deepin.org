---
title: Coredump工具使用与日志导出
description: 
published: true
date: 2022-07-27T10:07:31.749Z
tags: 
editor: markdown
dateCreated: 2022-07-07T02:41:15.851Z
---

# Coredump工具使用与日志导出
## Coredump的安装
1.打开终端，输入命令:sudo apt install systemd-coredump.在配置正确源的情况下，能正确的安装成功。
![703px-ff.jpg](/703px-ff.jpg)


安装命令截图

**当然也有安装不成功的情况：**

1.遇到依赖不够，这个情况，更具提示安装提示的依赖。

2.遇到直接找不到 systemd-coredump这个安装包，解决方案

（1）更新一下，执行命令：sudo apt update 之后安装，重启电脑，再安装 systemd-coredump。

（2）也有更新过后还是安装不了，这个一般情况是源的问题，在配置文件下添加新的源地址（命令：vim /etc/apt/sources.list）
```
deb [allow-insecure=yes] http://pools.corp.deepin.com/desktop-professional eagle main contrib non-free

deb [allow-insecure=yes] http://pools.corp.deepin.com/ppa/dde-eagle eagle/sp2 main contrib non-free

deb [by-hash=force trusted=yes] http://10.0.32.52:5002/public-repos/eagle-sp2/release eagle-sp2 main

deb [by-hash=force trusted=yes] http://10.0.32.52:5002/public-repos/nonfree-eagle-sp2/release nonfree-eagle-sp2 main
```

保存后，再次执行安装命令，或者更新后重启在执行安装。

## Coredump的使用
### （一）Coredump的开启
在上面安装完毕后，我们就开启coredump,我们怎么知道我们的Coredump开启了吗？

执行命令：```ulimit -c```

结果出现是：```0```

那么我们就是没有开启成功，执行一下命令开启
```
ulimit -c unlimited

再次执行 ulimit -c

出现的结果为： unlimited 就成功开启了coredump
```
**以上情况都是你一路顺利的情况下，当然我们有不顺利，如何结操作了？**

1.有的机器或者景象版本的影响，输入```ulimit -c unlimited```之后，查询结果还是0，这种情况就需要一下方式处理。

就需要进入到```/etc/profile```或者```/etc/security/limits.conf```文件中进行配置

（1）首先打开```/etc/profile```文件，一般都可以在文件中找到这句语句：```ulimit -S -c 0 > /dev/null 2>&1```，根据上面的例子，我们只要把那个```0 ```改为 ```unlimited ```就ok了。也有情况更本就没有这段代码，你就需要在文档最后位置加上一句话：```ulimit -c unlimited```或者```ulimit -S -c unlimited > /dev/null 2>&1```，随你发挥操作，后面的你需要在输入一个命令让你修改的配置文件生效：```source /etc/profile```。

（2）或者想配置只针对某一用户有效，则修改此用户的```~/.bashrc```或者```~/.bash_profile```文件：```limit -c unlimited``` 这段你可以加入任意位置，但是不要加入人家函数中哦。这个也需要执行命令让你配置生效。

（3）可以通过修改```/etc/security/limits.conf```文件来设置，首先以root权限登陆，然后打开```/etc/security/limits.conf```文件，进行配置：

![505px-dfdf.jpg](/505px-dfdf.jpg)

修改Value值为如图所示，这个方法我也没用过，嘿嘿嘿。这个应该也需要让你配置生效。

### 	（二）获取Coredump日志
以上配置完毕，就可以抓取coredump日志。下面是获取coredump日志

1.我们首先打开终端输入命令：```coredumpctl``` 或者 ```coredumpctl list``` 就能查看到coredump日志了。

![627px-erer.jpg](/627px-erer.jpg)
如图所示，我们就能看到很多coredump日志，我们一般就需要看3个地方TIME，PID和EXE。

TIME：这个是崩溃差生的时间

PID：是我们需要用到查询这个id查询这个coredump日志情况

EXE：这个是我们查看那个应用产生的，如图最后一条EXE的信息为：```/usr/bin/dde-file-manager```,这个coredump日志是文件管理器产生的日志。

这些崩溃日志产生的路径为：```/var/lib/systemd/coredump ```我们也可以直接进去查看，但是这个是压缩文件，你需要解压。

![752px-swsw.jpg](/752px-swsw.jpg)

**当然以上都是很顺利的情况下，会存在抓不到Coredump日志，这个原因有可能程序没有走到你要抓取应用的代码，也就是你第三方软件造成了，你测试应用的崩溃。这样2边日志都会抓不到。还有另外一种情况不产生coredump日志.你就重新安装了**。

2.查看日志我们需要用到外部工具gdb，下载一个gdb就可以查看了，这个就不详细说这个gdb工具，只说如何产看coredump日志。

我们需要打开终端命令：coedumpctl gdb pid(如：25843)

![445px-qwqw.jpg](/445px-qwqw.jpg)

执行以上命令后，我们就打开了coredump日志。

![776px-cxcx.jpg](/776px-cxcx.jpg)

如上图所示，被gdb工具打开后，我们执行命令：bt 就能查看详细的coredump日志信息了，如下图

![774px-asas.jpg](/774px-asas.jpg)

上图就是日志信息，可以找相关开发查看。

## Coredump加强学习
### 什么是coredump：
    我们经常听到大家说到程序core掉了，需要定位解决，这里说的大部分是指对应程序由于各种异常或者bug导致在运行过程中异常退出或者终止，并且在满足一定条件下会产生一个叫做core的文件。

    通常情况下，core文件会包含了程序运行时的内存，寄存器状态，堆栈指针，内存管理信息还有各种函数调用堆栈信息等，我们可以理解为是程序工作当前状态存储生成的一个文件，许多的程序出错的时候都会产生一个core文件，通过工具分析这个文件，我们可以定位到程序异常退出的时候对应的堆栈调用等信息，找出问题所在并进行及时解决。

ulimit详解
命令：ulimit -
| 命令参数   | 描述                                                     | 例子                                                         |
| ---------- | -------------------------------------------------------- | ------------------------------------------------------------ |
| -H       | 设置硬资源限制，一旦设置不能增加。                       | ulimit – Hs 64；限制硬资源，线程栈大小为 64K。               |
| -S       | 设置软资源限制，设置后可以增加，但是不能超过硬资源设置。 | ulimit – Sn 32；限制软资源，32 个文件描述符。                |
| -a       | 显示当前所有的 limit 信息                                | ulimit – a；显示当前所有的 limit 信息                        |
| -c       | 最大的 core 文件的大小， 以 blocks 为单位                | ulimit – c unlimited； 对生成的 core 文件的大小不进行限制    |
| -d       | 进程最大的数据段的大小，以 Kbytes 为单位                 | ulimit -d unlimited；对进程的数据段大小不进行限制            |
| -f       | 进程可以创建文件的最大值，以 blocks 为单位               | ulimit – f 2048；限制进程可以创建的最大文件大小为 2048 blocks |
| -l       | 最大可加锁内存大小，以 Kbytes 为单位                     | ulimit – l 32；限制最大可加锁内存大小为 32 Kbytes            |
| -m       | 最大内存大小，以 Kbytes 为单位                           | ulimit – m unlimited；对最大内存不进行限制                   |
| -n       | 可以打开最大文件描述符的数量                             | ulimit – n 128；限制最大可以使用 128 个文件描述符            |
| -p       | 管道缓冲区的大小，以 Kbytes 为单位                       | ulimit – p 512；限制管道缓冲区的大小为 512 Kbytes            |
| -s       | 线程栈大小，以 Kbytes 为单位                             | ulimit – s 512；限制线程栈的大小为 512 Kbytes                |
| -t       | 最大的 CPU 占用时间，以秒为单位                          | ulimit – t unlimited；对最大的 CPU 占用时间不进行限制        |
| -u       | 用户最大可用的进程数                                     | ulimit – u 64；限制用户最多可以使用 64 个进程                |
| -v       | 进程最大可用的虚拟内存，以 Kbytes 为单位                 | ulimit – v 200000；限制最大可用的虚拟内存为 200000 Kbytes    |
## 造成程序coredump的原因
造成程序coredump的原因有很多，这里总结一些比较常用的经验吧：

### （1）内存访问越界：

   a) 由于使用错误的下标，导致数组访问越界。

   b) 搜索字符串时，依靠字符串结束符来判断字符串是否结束，但是字符串没有正常的使用结束符。

   c) 使用strcpy, strcat, sprintf, strcmp,strcasecmp等字符串操作函数，将目标字符串读/写爆。应该使用strncpy, strlcpy, strncat, strlcat, snprintf, strncmp, strncasecmp等函数防止读写越界。

### （2）多线程程序使用了线程不安全的函数：

### （3）多线程读写的数据未加锁保护：

    对于会被多个线程同时访问的全局数据，应该注意加锁保护，否则很容易造成coredump。

### （4）非法指针：

    a) 使用空指针；

    b) 随意使用指针转换。一个指向一段内存的指针，除非确定这段内存原先就分配为某种结构或类型，或者这种结构或类型的数组，否则不要将它转换为这种结构或类型的指针，而应该将这段内存拷贝到一个这种结构或类型中，再访问这个结构或类型。这是因为如果这段内存的开始地址不是按照这种结构或类型对齐的，那么访问它时就很容易因为bus error而core dump。

### （5）堆栈溢出：

    不要使用大的局部变量（因为局部变量都分配在栈上），这样容易造成堆栈溢出，破坏系统的栈和堆结构，导致出现莫名其妙的错误。  