---
title: Linux文件和目录操作命令
description: 
published: true
date: 2023-02-22T02:27:46.308Z
tags: 
editor: markdown
dateCreated: 2023-02-21T09:47:57.218Z
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

**（9）rm：删除文件或目录**

**说明**

rm命令可以理解为英文单词remove的缩写，其功能是删除一个或多个文件或目录（remove files or directories）。这是Linux系统里最危险的命令之一，请慎重使用。

**语法**

rm [option] [file]

**选项**

![2023-1-19_75242.png](/2023-1-19_75242.png)

**经验**

1.用mv替代rm，不要急着删除，而是先移动到回收站/tmp。

2.删除前务必备份，最好是异机备份，若出现问题随时可以还原。

3.如果非要删除，那么请用find替代rm，包括通过系统定时任务等清理文件方法。

4.如果非要通过rm命令删除，那么请先切换目录再删除，能不用通配符的就不用通配符。对文件的删除禁止使用“rm-rf文件名”，因为“rm-rf”误删目录时并不会有提示，非常危险。最多使用“rm-f文件名”，推荐用“rm文件名”。



**（10）rmdir：删除空目录**

**说明**

rmdir命令用于删除空目录（remove empty directories），当目录不为空时，命令不起作用。

**语法**

rmdir [option] [directory]

**选项**

![2023-1-19_22513.png](/2023-1-19_22513.png)

**（11）ln：硬链接与软链接**

**说明**

ln命令可以理解为英文单词link的缩写，其功能是创建文件间的链接（make links between files），链接类型包括硬链接（hard link）和软链接（符号链接，symbolic link）

**语法**

ln [option] [source] [target]

**选项**

![2023-1-19_15293.png](/2023-1-19_15293.png)

**细说链接知识**

链接分为硬链接（hard link）和软链接（符号链接，symbolic link）两种，它们的含义具体如下。

硬链接（Hard Link）：创建语法为“ln源文件目标文件”，硬链接生成的是普通文件（-字符）。

软链接或符号链接（Symbolic Link or Soft Link）：创建语法为“ln-s源文件目标文件（目标文件不能事先存在）”，软链接生成的是符号链接文件（l类型）。

**硬链接**

硬链接是指通过索引节点（Inode）来进行链接。在Linux（ext2、ext3、ext4）文件系统中，所有文件都有一个独有的inode编号。在Linux文件系统中，多个文件名指向同一个索引节点（inode）是正常且允许的。这种情况下的文件就称为硬链接。硬链接文件相当于文件的另外一个入口。它的作用之一就是允许一个文件拥有多个有效路径名（多个入口），这样用户就可以建立硬链接到重要文件，以防止误删源数据。

1.具有相同inode节点号的多个文件互为硬链接文件。

2.删除硬链接文件或者删除源文件任意之一，文件实体并未被删除。

3.只有删除了源文件以及源文件所有对应的硬链接文件，文件实体才会被删除。

4.所有的硬链接文件及源文件被删除之后，再存放新的数据时会占用这个文件的空间，或者磁盘fsck检查的时候，删除的数据也会被系统回收。

5.硬链接文件就是文件的另外一个入口（相当于超市的前门后门）。

6.可以通过给文件设置硬链接文件，来防止重要文件被误删。

7.执行命令“ln源文件硬链接文件”，即可完成硬链接的创建。

8.硬链接文件可以用rm命令删除。

9.对于静态文件（没有进程正在调用的文件）来讲，当对应硬链接数为0（i_link）时，文件就会被删除。i_link的查看方法是ls-lih，查看结果的第三列，即硬链接数。

**软链接**

软链接或符号链接（Symbolic Link or Soft Link）有点像Windows里的快捷方式。

硬链接文件的类型是普通文件，而软链接是真正的链接文件（l）。

1.软链接类似于Windows的快捷方式（可以通过后面的readlink命令查看其指向）。

2.软链接类似于一个文本文件，里面存放的是源文件的路径，指向源文件实体。

3.即使删除了源文件，软链接文件也还是依然存在，但是无法访问指向的源文件路径内容了。

4.失效的时候一般是白字红底闪烁提示。

5.执行命令“ln-s源文件软链接文件”，即可完成创建软链接（软链接文件名事先不能存在）

6.软链接和源文件是不同类型的文件，也是不同的文件，inode号也不相同。

7.删除软链接文件可以使用rm命令。



**（12）readlink：查看符号链接文件的内容**

**说明**

使用cat命令查看软链接文件时，会发现只能看到源文件的内容，看不到软链接文件的真实内容。因此需要使用readlink命令，它能够帮助我们查看符号链接文件的真实内容。

**语法**

readlink [option] [file]

**选项**

![2023-1-19_99336.png](/2023-1-19_99336.png)

[root@iZ8vb11v8r15ng6q0eb8dzZ /]$ll -h /usr/bin/awk

lrwxrwxrwx. 1 root root 4 May 11 2019 /usr/bin/awk -> gawk

[root@iZ8vb11v8r15ng6q0eb8dzZ /]$readlink /usr/bin/awk

gawk

[root@iZ8vb11v8r15ng6q0eb8dzZ /]$readlink -f /usr/bin/awk

/usr/bin/gawk



**（13）find：查找目录下的文件**

**说明**

find命令用于查找目录下的文件，同时也可以调用其他命令执行相应的操作。

**语法**

find [-H] [-L] [-P] [-D debugopts] [-0level] [pathname] [expression]

1.注意find命令以及后面的选项和路径、操作语句，每个元素之间都至少要有一个空格。

2.注意子模块的先后顺序。

![2023-1-19_93874.png](/2023-1-19_93874.png)

**选项**

![2023-1-19_23018.png](/2023-1-19_23018.png)

**（14）xargs：将标准输入转换成命令行参数**

**说明**

xargs命令是向其他命令传递命令行参数的一个过滤器，能够将管道或者标准输入传递的数据转换成xargs命令后跟随的命令的命令行参数。

**语法**

xargs [option]

**选项**

![2023-1-19_40466.png](/2023-1-19_40466.png)

**（15）rename：重命名文件**

**说明**

rename命令通过字符串替换的方式批量修改文件名。

**语法**

rename from to file

1.from：代表需要替换或处理的字符，一般是文件名的一部分，也包括拓展名

2.to：把前面的from代表的内容替换为to代表的内容

3.file：待处理的文件，可以用“*”通配所有文件

使用范例

将所有文件的finished替换为空

rename “finished” “” *

将所有文件的.jpg替换为.png

rename .jpg .png *.jpg



**（16）basename：显示文件名或目录名**

**说明**

basename命令用于显示去除路径和文件后缀部分的文件名或目录名。

**语法**

basename [name] [suffix]

suffix是可选参数，指定要去除的文件后缀字符串。

使用范例

去除路径部分，即只显示文件名

[root@iZ8 /home]$mkdir data/dir1 -p

[root@iZ8 /home]$touch data/dir1/file.txt

[root@iZ8 /home]$basename data/dir1/file.txt

file.txt

[root@iZ8 /home]$basename data/dir1/file.txt .txt

file



**（17）dirname：显示文件或目录路径**

**说明**

dirname命令用于显示文件或目录路径。

**语法**

dirname [name]

**使用范例**

只显示文件所在路径

[root@iZ8 /home]$dirname data/dir1/file.txt

data/dir1

给dirname一个相对路径，他也会返回相对路径（当前目录.）

[root@iZ8 /home]$cd data/dir1/

[root@iZ8 /home/data/dir1]$dirname file.txt

.



**（18）chattr：改变文件的扩展属性**

**说明**

chattr命令用于改变文件的扩展属性。与chmod这个命令相比，chmod只是改变文件的读、写、执行权限，更底层的属性控制是由chattr来改变的。

**语法**

chattr [option] [mode] [files]

**选项**

![2023-1-19_1120.png](/2023-1-19_1120.png)

**使用范例**

设置只能往文件里追加内容，但不能删除文件。

chattr +a test

给文件加锁，使其只能是只读。

chattr +i file

避免恶意删除.bash_history历史记录文件或者重定向到/dev/null，又因为系统需要向这个文件中写入历史记录，因此采用追加模式，只增不减：

chattr +a .bash_history

tips: 关于chattr的功能，我们能够操作的，黑客如果知道了也能操作，因此，使用chattr的安全性是相对的。



**（19）lsattr：查看文件扩展属性**

**说明**

lsattr命令用于查看文件的扩展属性。

**语法**

lsattr [options] [files]

**选项**

![2023-1-19_3688.png](/2023-1-19_3688.png)

**使用范例**

查看文件扩展属性

[root@iZ8 /home]$lsattr file.txt

-------------------- file.txt

[root@iZ8/home]$chattr +i file.txt

[root@iZ8 /home]$lsattr file.txt

----i--------------- file.txt

查看目录扩展属性

[root@iZ8 /home]$lsattr -d dir/

-------------------- dir/

[root@iZ8 /home]$chattr +i dir

[root@iZ8 /home]$lsattr -d dir/

----i--------------- dir/



**（20）file：显示文件的类型**

**说明**

file命令用于显示文件的类型。

**语法**

file [option] [file]

多个文件之间使用空格分开，可以使用通配符来匹配多个文件。

**选项**

![2023-1-19_16921.png](/2023-1-19_16921.png)

**（21）md5sum：计算和校验文件的MD5值**

**说明**

md5sum命令用于计算和校验文件的MD5值。MD5的全名为Message-Digest Algorithm（信息-摘要算法）5，它是一种不可逆的加密算法。软件或文件一般都有自己固定的文件格式或信息，简单一点说就是“世界上没有完全相同的两片叶子”，那么对于某些网上公开下载的软件、视频，尤其是镜像文件，如果被修改了可能会导致用不了或者其他的问题。因此发布者首先要通过MD5算法得出一组数值，然后让下载的用户进行MD5的数值对比，即MD5校验。基于MD5加密不可逆算的特性，如果数值一样，那么就表示文件没有受到修改。反之，则表示修改了。

**语法**

md5sum [option] [file]

**选项**

![2023-1-19_40067.png](/2023-1-19_40067.png)

**生产案例**

利用md5sum命令来检验备份文件是否遭到损坏。

md5sum命令用于备份任务的指纹检查。每次在备份完成之后生成指纹文件，将备份和指纹文件发送到备份服务器上，在备份服务器上又会通过md5sum命令和校验文件校验备份是否正确。这样做的目的是为了在第一时间发现可能因为网络传输而造成的文件损坏。



**（22）chown：改变文件或目录的用户和用户组**

**说明**

chown命令用于改变文件或目录的用户和用户组。

**语法**

chown [option] [OWNER] [:[GROUP]] [file]

**常用格式**

chown 用户 文件或目录 # 仅仅授权用户

chown :组 文件或目录 # 仅仅授权组

chown 用户:组 文件或目录 # 表示授权用户和组

1.其中的 : 可以用 . 来代替

2.要授权的用户和组名，必须是Linux系统实际存在的。

**选项**

![2023-1-19_78970.png](/2023-1-19_78970.png)

**范例**

\# 更改文件所属用户属性

[root@iZ8 /home]$ll

total 0

-rw-r–r-- 1 root root 0 Jan 4 14:09 file.txt

drwx------ 2 testuser testuser 62 Jan 4 14:05 testuser

[root@iZ8 /home]$chown testuser file.txt

[root@iZ8 /home]$ll

total 0

-rw-r–r-- 1 testuser root 0 Jan 4 14:09 file.txt

drwx------ 2 testuser testuser 62 Jan 4 14:05 testuser

\# 更改文件所属组属性

[root@iZ8 /home]$chown .testuser file.txt

[root@iZ8 /home]$ll

total 0

-rw-r–r-- 1 testuser testuser 0 Jan 4 14:09 file.txt

drwx------ 2 testuser testuser 62 Jan 4 14:05 testuser



**（23）chmod：改变文件或目录权限**

**说明**

chmod命令是用来改变文件或目录权限的命令，但是只有文件的属主和超级用户root才能够执行这个命令。

**语法**

chmod [option] [mode] [file]

模式有两种格式：一种是采用权限字母和操作符表达式；另一种是采用数字。

**选项**

![2023-1-19_81785.png](/2023-1-19_81785.png)

![2023-1-19_41589.png](/2023-1-19_41589.png)

字母和数字权限转换图

![2023-1-19_64132.png](/2023-1-19_64132.png)

目录权限说明

![2023-1-19_97304.png](/2023-1-19_97304.png)

**（24）umask：显示或设置权限掩码**

**说明**

umask是通过八进制的数值来定义用户创建文件或目录的默认权限。

**语法**

umask [option] [mode]

**选项**

![2023-1-19_75444.png](/2023-1-19_75444.png)

文件权限计算

创建文件默认最大的权限为666（-rw-rw-rw-），默认创建的文件没有可执行权限x位。

对于文件来说，umask的设置是在假定文件拥有八进制666的权限上进行的，文件的权限就是666减umask（umask的各个位数字也不能大于6，比如077就不符合条件）的掩码数值，如果得到的3位数字其每一位都是偶数，那么这就是最终结果；如果有若干位的数字是奇数，那么这个奇数需要加1变成偶数，最后得到全是偶数的结果。

**示例**

1.umask 022 所有位为偶数

6 6 6 # 文件的起始权限值

0 2 2 - # umask 的值

\-----------

6 4 4

2.umask 045 其他用户组为奇数

6 6 6 # 文件的起始权限值

0 4 5 - # umask值

6 2 1 # 计算出来的权限，由于umask最后一位是奇数5，所以，其他用户组位+1

0 0 1 + # umask 对应的奇数位+1

6 2 2 # 真实文件权限

目录权限计算（没有奇偶之分）

创建目录默认最大权限777（-rwx-rwx-rwx），默认创建的目录属主是有x权限的，允许用户进入。

![2023-1-19_16624.png](/2023-1-19_16624.png)

文件权限计算

创建文件默认最大的权限为666（-rw-rw-rw-），默认创建的文件没有可执行权限x位。

对于文件来说，umask的设置是在假定文件拥有八进制666的权限上进行的，文件的权限就是666减umask（umask的各个位数字也不能大于6，比如077就不符合条件）的掩码数值，如果得到的3位数字其每一位都是偶数，那么这就是最终结果；如果有若干位的数字是奇数，那么这个奇数需要加1变成偶数，最后得到全是偶数的结果。

**示例**

1.umask 022 所有位为偶数

6 6 6 # 文件的起始权限值

0 2 2 - # umask 的值

\-----------

6 4 4

2.umask 045 其他用户组为奇数

6 6 6 # 文件的起始权限值

0 4 5 - # umask值

6 2 1 # 计算出来的权限，由于umask最后一位是奇数5，所以，其他用户组位+1

0 0 1 + # umask 对应的奇数位+1

6 2 2 # 真实文件权限

目录权限计算（没有奇偶之分）

创建目录默认最大权限777（-rwx-rwx-rwx），默认创建的目录属主是有x权限的，允许用户进入。

对于目录来说，umask的设置是在假定文件拥有八进制777权限上进行，目录八进制权限777减去umask的掩码数值。