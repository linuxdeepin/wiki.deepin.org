---
title: Linux文件和目录操作命令
description: 
published: true
date: 2023-01-19T01:31:00.792Z
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

**在Bash命令行显示当前用户的完整路径。**

系统Bash命令行的提示符是由一个称为PS1的系统环境变量控制的。

![2023-1-19_78390.png](/2023-1-19_78390.png)

查看当前PS1变量的值

[root@iZ8vb11v8r15ng6q0eb8dzZ home]# echo $PS1

[\u@\h \W]$

修改PS1变量对应的值，来让命令行显示全路径(临时生效)

[root@iZ8vb11v8r15ng6q0eb8dzZ home]# PS1="[\u@\h \w]$"

[root@iZ8vb11v8r15ng6q0eb8dzZ /home]$cd /etc/sysconfig

[root@iZ8vb11v8r15ng6q0eb8dzZ /etc/sysconfig]$

修改PS1变量对应的值，来让命令行显示全路径(永久生效)

1.编辑/etc/bashrc文件，找到以下内容

[ “$PS1” = “\s-\v\$ " ] && PS1=”[\u@\h \W]\$ "

2.修改退出

[ “$PS1” = “\s-\v\$ " ] && PS1=”[\u@\h \w]\$ "

3.最后，注销并重新登录系统或直接执行source/etc/bashrc使得修改的信息生效



**（2）cd：切换目录**

**说明**

cd命令是“change directory”中每个单词的首字母缩写，其功能是从当前工作目录切换到指定的工作目录。

**语法**

cd [option] [dir]

1.注意cd命令以及后面的选项和目录，每个元素之间都至少要有一个空格。

2.cd命令后面的选项和目录等参数都可以省略。默认情况下，单独执行cd命令，可切换到当前登录用户的家目录（由系统环境变量HOME定义）。

3.cd是bash shell的内置命令，查看该命令对应的系统帮助需要使用help cd。

**参数**

![2023-1-19_38635.png](/2023-1-19_38635.png)

执行不带任何参数的cd命令和“cd~”的结果一样。



**（3）tree：以树形结构显示目录下的内容**

**说明**

tree命令的中文意思为“树”，功能是以树形结构列出指定目录下的所有内容，包括所有文件、子目录及子目录里的目录和文件。

**语法**

tree [option] [directory]

1.注意tree命令以及后面的选项和目录，每个元素之间都至少要有一个空格。

2.tree命令后若不接选项和目录就会默认显示当前所在路径目录的目录结构。

**选项**

![2023-1-19_57891.png](/2023-1-19_57891.png)

查询tree命令是否安装

rpm -qa tree

安装tree命令

yum -y install tree

调整系统字符集，防止树形结构显示乱码

LANG=en_US.UTF-8



**（4）mkdir：创建目录**

**说明**

mkdir命令是“make directories”中每个单词的粗体字母组合而成，其功能是创建目录，默认情况下，如果要创建的目录已存在，则会提示此文件已存在；而不会继续创建目录。

**语法**

mkdir [option] [directory]

1.注意mkdir命令以及后面的选项和目录，每个元素之间都至少要有一个空格。

2.mkdir命令可以同时创建多个目录，格式为mkdir dir1dir2…

**选项**

![2023-1-19_23218.png](/2023-1-19_23218.png)

**拓展**

大括号 {} 的特殊用法。

在{}中使用逗号分隔多个字符或单词时，使用echo命令可以将这些被分隔的字符或单词分别输出到屏幕上，示例如下：

[root@iZ8vb11v8r15ng6q0eb8dzZ /]$echo {B,C}

B C

如果{}前有字符时，输出结果如下：

[root@iZ8vb11v8r15ng6q0eb8dzZ /]$echo A{B,C}

AB AC

创建文件夹同理

[root@iZ8vb11v8r15ng6q0eb8dzZ /home]$mkdir B C

[root@iZ8vb11v8r15ng6q0eb8dzZ /home]$ls

B C hello.txt

[root@iZ8vb11v8r15ng6q0eb8dzZ /home]$mkdir A{B,C}

[root@iZ8vb11v8r15ng6q0eb8dzZ /home]$ls

AB AC B C hello.txt

[root@iZ8vb11v8r15ng6q0eb8dzZ /home]$



**（5）touch：创建空文件或改变文件的间戳属性**

**说明**

touch命令有两个功能：

1.是创建新的空文件

2.是改变已有文件的时间戳属性

**语法**

touch [option] [file]

1.注意区分touch和mkdir命令的功能，mkdir命令是创建空目录，而touch是创建空文件。

2.在Linux中，一切皆文件。虽然touch命令不能创建目录，但是可以修改目录的时间戳。

**选项**

![2023-1-19_16050.png](/2023-1-19_16050.png)

**拓展**

GNU/Linux的文件有3种类型的时间戳：

Access: 1985-11-05 15:33:34.991966181 +0800

Modify: 1985-11-05 15:33:34.991966181 +0800

Change: 1985-11-05 15:33:34.991966181 +0800

1.Access 最后访问文件时间

2.Modify 最后修改文件时间

3.Change 最后改变文件状态时间

**对应ls命令，查看上述时间戳的选项如下**

1.mtime: 最后修改时间（ls -lt) 修改文件内容，文件的修改时间(modify time）会改变。

2.ctime:状态改变时间（ls -lc) 修改文件内容、移动文件或改变文件属性等，文件的change时间会改变。

3.atime:最后访问时间（ls -lu) 查看文件内容时，文件的访问时间（ access time）会改变。



**（6）ls：显示目录下的内容及相关属性信息**

**说明**

ls命令可以理解为英文单词list的缩写，其功能是列出目录的内容及其内容属性信息（list directory contents）。

**语法**

ls [option] [file]

1.ls命令以及后面的选项和文件，每个元素之间都至少要有一个空格。

\2. 命令后面的选项和目录文件可以省略，表示查看当前路径的文件信息。

**选项**

![2023-1-19_44898.png](/2023-1-19_44898.png)

ls命令输出内容的属性解读

[root@hsforpyp /]$ls -lhi

total 20K

  24972 lrwxrwxrwx.  1 root root  7 May 11 2019 bin -> usr/bin

   147 dr-xr-xr-x.  5 root root 4.0K Nov 20 14:45 boot

  1026 drwxr-xr-x  19 root root 2.9K Dec 29 23:50 dev

   132 drwxr-xr-x. 94 root root 8.0K Jan 4 11:39 etc

50332430 drwxr-xr-x.  2 root root  23 Jan 4 11:59 home

  24976 lrwxrwxrwx.  1 root root  7 May 11 2019 lib -> usr/lib

   149 lrwxrwxrwx.  1 root root  9 May 11 2019 lib64 -> usr/lib64

   150 drwxr-xr-x.  2 root root  6 May 11 2019 media

16779732 drwxr-xr-x.  2 root root  6 May 11 2019 mnt

33614798 drwxr-xr-x.  2 root root  6 May 11 2019 opt

​    1 dr-xr-xr-x 111 root root  0 Dec 29 23:50 proc

16777345 dr-xr-x---.  5 root root 228 Jan 4 11:39 root

  11624 drwxr-xr-x  30 root root 880 Dec 29 23:50 run

  24977 lrwxrwxrwx.  1 root root  8 May 11 2019 sbin -> usr/sbin

50332431 drwxr-xr-x.  2 root root  6 May 11 2019 srv

​    1 dr-xr-xr-x  13 root root  0 Dec 30 07:50 sys

   148 drwxrwxrwt.  3 root root 126 Jan 4 12:21 tmp

16777371 drwxr-xr-x. 12 root root 144 Nov 20 14:36 usr

33575041 drwxr-xr-x. 21 root root 4.0K Nov 20 06:41 var

1.inode索引节点编号

2.文件类型及权限

3.硬链接个数

4.文件或目录所属用户

5.文件或目录所属组

6.文件或目录的大小

7.文件或目录修改时间（7、8、9列）

8.实际的文件名或目录名



**（7）cp：复制文件或目录**

**说明**

cp命令可以理解为英文单词copy的缩写，其功能为复制文件或目录。

**语法**

cp [option] [source] [dest]

**选项**

![2023-1-19_9574.png](/2023-1-19_9574.png)

**（8）mv：移动或重命名文件**

**说明**

mv命令可以理解为英文单词move的缩写，其功能是移动或重命名文件（move/rename files）

**语法**

mv [option] [source] [dest]

**选项**

![2023-1-19_344.png](/2023-1-19_344.png)