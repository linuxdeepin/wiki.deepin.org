---
title: Deepin运维教程-L1
description: 
published: true
date: 2022-10-18T03:28:27.203Z
tags: 运维
editor: markdown
dateCreated: 2022-06-28T05:45:04.943Z
---

# Deepin运维教程-L1
#### 命令终端字段含义介绍

- `[root@localhost ~]#`

- 解释：当前用户名为root@主机名为localhost 当前所在目录为 ~ 家目录 # 当前用户身份是超级管理员，root超级管理员家目录：/root

- 普通用户提示符为 $，普通用户的家目录：/home/用户名同名，lisi用户的家目录：/home/lisi

  `[lisi@localhost ~]$`

#### Linux系统基本概念

- Linux系统而言：
  - 多用户的系统：允许同时有很多个用户登录系统，使用系统里的资源
  - 多任务的系统：允许同时执行多个任务
  - 严格区分大小写：命令，选项，参数，文件名，目录名都严格区分大小写
  - 一切皆文件：硬件设备（内存、CPU、网卡、显示器、硬盘等等）都是以文件的形式存在的
  - 不管是文件还是目录都是以倒挂的树形结构，存在于系统的“/”根目录下，根目录是Linux系统的起点
  - 对于Linux系统而言，目录/文件没有扩展名一说，扩展名如：.sh（脚本文件) .conf（配置文件） .log（日志文件） .rpm（软件包）.tar（压缩包）是易于用户方便识别
  - 没有提示就是最好的提示（成功了）

#### 命令行编辑技巧

键盘上下键调出历史命令

<kbd>Ctrl</kbd> + <kbd>C</kbd>：取消当前执行的命令

<kbd>Ctrl</kbd> + <kbd>L</kbd>：清屏

<kbd>Tab</kbd>键自动补齐：可补齐命令、选项、参数、文件路径、软件名、服务名

<kbd>Ctrl</kbd> + <kbd>A</kbd>：将当前光标移动至行首

<kbd>Ctrl</kbd> + <kbd>E</kbd>：将当前光标移动至行尾

<kbd>Ctrl</kbd> + <kbd>U</kbd> 清空至行首

<kbd>Ctrl</kbd> + <kbd>W</kbd> 删除一个单词

`exit`：退出系统

#### 命令行一般命令格式

- `命令字 [-选项...] [参数...]`
  - 命令字：命令本身（功能）
  - 选项：调整命令功能的
    - 短选项：`-l` `-a` `-d` `-h`（单个字符），短选项可以合并使用：`-lad` `-lh`
    - 长选项：`--help`（单词），长选项通常是不能合并使用的
  - 参数：命令的执行对象，文件/目录/程序等
  - \[ \]：可选的
  - ...：可以同时有多个选项或参数

#### 学习方法

- 遇到问题：前期不要求你们有排错的能力
- 思考自己能不能解决：百度、Google、最后再问老师
- 主动学习：不要被动学习
- 不要死磕一个技术点：低头学习的时候不要忘了抬头看路

#### Linux系统辨别目录与文件的方法

蓝色表示目录（windows系统里的文件夹）

白色表示文件

浅蓝色表示链接文件（类似于windows系统的快捷方式）

绿色表示可执行文件（如脚本，命令程序文件）

红色表示压缩文件

黄色表示设备文件（硬盘、键盘、鼠标、网卡、CPU硬件设备都是以文件的形式存在的）

红色闪动文件——>表示链接文件不可用

#### ls 查看目录/文件命令

- ls命令（英文全拼：list）：用于查看目录下内容及目录和文件详细属性信息
- 命令格式：`ls [-选项...] [参数...]`
- 常用选项：
  - `-a` 显示目录下所有内容，包含隐藏的内容
  - `-l` 以长格式显示目录下的内容及详细属性
  - `-h` 人性化显示目录下内容大小（kB、MB、GB）
  - `-d` 仅显示目录本身而不显示目录下的内容
  - `-i` 查看inode号（系统任何的文件或目录都有一个唯一的编号）
  - `-R` 递归查看目录下所有内容（从头到尾）

#### Linux 系统文件类型

\- 文件：

d 目录：

l 链接文件

b 跨设备文件

c 字符设备文件

p 管道设备文件

s 套接字

#### Linux 系统下的归属关系

- 在Linux系统下，文件给用户分成了三类
  - u 所有者：文件或目录的拥有者，拥有者的权限通常是最大的
  - g 所属组：文件或目录属于哪一个组，所属组的权限略微比所有者小
  - o 其他人：既不是文件或目录的所有者，也不属于文件或目录组内的成员，其他人的权限通常最小的权限
- ls命令示例：

```
#显示当前所在目录下的所有内容
[root@localhost ~]# ls		

#查看根目录下所有内容
[root@localhost ~]# ls   /
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

#查看/etc目录下所有内容
[root@localhost ~]# ls /etc

#查看/bin目录下所有内容
[root@localhost ~]# ls /bin

#查看/dev目录下所有内容
[root@localhost ~]# ls /dev

#查看目录下所有目录和文件，包括隐藏的内容
[root@localhost ~]# ls -a

#以长格式显示目录下所有内容，包括详细的属性信息
[root@localhost ~]# ls -l
-rw-r--r--. 1 root root 1831 3月  13 17:45 initial-setup-ks.cfg
drwxr-xr-x. 2 root root    6 3月  13 17:47 公共
#解释
-：文件类型
1：代表文件的引用次数，只针对与做了硬连接的文件才有效
root：文件的所有者
root：文件的所属组
1831：文件的大小，默认以字节为单位显示大小
3月  13 17:45：文件最近一次的修改时间
initial-setup-ks.cfg：文件名

#以长格式显示目录所有内容，以人性化的方式显示详细的属性信息
[root@localhost ~]# ls -l -h

#短选项合并使用
[root@localhost ~]# ls -lh

#以长格式显示目录所有内容，以人性化的方式显示详细的属性信息，包括隐藏的内容
[root@localhost ~]# ls -lha


#以长格式显示根目录下所有内容，包括详细的属性信息
[root@localhost ~]# ls -l /
lrwxrwxrwx.   1 root root    7 3月  13 17:15 bin -> usr/bin

#创建hello.txt文件
[root@localhost ~]# touch hello.txt

#查看文件的元数据信息
[root@localhost ~]# stat hello.txt
  文件："hello.txt"
  大小：0         	块：0          IO 块：4096   普通空文件
设备：fd00h/64768d	Inode：33575020    硬链接：1
权限：(0644/-rw-r--r--)  Uid：(    0/    root)   Gid：(    0/    root)
环境：unconfined_u:object_r:admin_home_t:s0
最近访问：2021-03-14 16:38:14.349861770 +0800
最近更改：2021-03-14 16:38:14.349861770 +0800
最近改动：2021-03-14 16:38:14.349861770 +0800
创建时间：-
```

#### Linux 基本权限的类别

- r 读取 w 写入 x 执行 - 没有权限
- 权限顺序：rwxrwxrwx

```
[root@localhost ~]# ls -l
-rw-r--r--. 1 root root 1831 3月  13 17:45 initial-setup-ks.cfg
#解释
-：文件类型
rw- r-- r--：所有者u、所属组g、其他人o的权限
u   g   o

1：代表文件的引用次数，只针对与做了硬连接的文件才有效
root：文件的所有者
root：文件的所属组
1831：文件的大小，默认以字节为单位显示大小
3月  13 17:45：文件最近一次的修改时间
initial-setup-ks.cfg：文件名

#查看/root目录本身详细属性信息
[root@localhost ~]# ls -ld /root
dr-xr-x---. 14 root root 4096 3月  14 16:38 /root

#查看当前目录下所有内容的inode号
[root@localhost ~]# ls -i
33574979 anaconda-ks.cfg  33574984 initial-setup-ks.cfg  33575035 模板  33575036 图片  17470701 下载            17470702 音乐
33575020 hello.txt        51909391 公共                  51909392 视频   3204374 文档  33575017 新建文件夹.zip   3204373 桌面

#查看hello.txt文件的inode号
[root@localhost ~]# ls -i hello.txt
33575020 hello.txt

#查看/etc/目录本身的inode号
[root@localhost ~]# ls -id /etc
16777281 /etc
```

#### mkdir 创建目录命令

- mkdir（英文全拼：make directory）用于创建新目录
- 命令格式：`mkdir [-选项] 目录名`
- 常用选项：
  - `-p` 递归创建多个目录
- 注意事项：
  - 目录还是文件的名字，除了以“/”以外的任意名称，“/”根目录，路径分隔符
  - 文件或目录的名字长度不能超过255个字符
- mkdir命令示例

```
#在当前所在目录创建test目录
[root@localhost ~]# mkdir test
[root@localhost ~]# ls

#在当前所在目录同时创建多个目录
[root@localhost ~]# mkdir test1 test2 test3
[root@localhost ~]# ls

#指定在/tmp目录下创建abc目录
[root@localhost ~]# mkdir /tmp/abc
[root@localhost ~]# ls /tmp
abc

#在指定目录下同时创建多个目录
[root@localhost ~]# mkdir /tmp/abc1 /tmp/abc2 /tmp/abc3
[root@localhost ~]# ls /tmp

#在/opt目录下创建student，在当前目录创建student1..3
[root@localhost ~]# mkdir /opt/student student1  student2 student3
[root@localhost ~]# ls /opt
rh  student

#mkdir默认无法在一个不存在的目录下创建目录，需要通过-p选项
[root@localhost ~]# mkdir /opt/xx/oo
mkdir: 无法创建目录"/opt/xx/oo": 没有那个文件或目录

[root@localhost ~]# mkdir /opt/a/b/c/d
mkdir: 无法创建目录"/opt/a/b/c/d": 没有那个文件或目录

#在/opt目录下递归创建目录
[root@localhost ~]# mkdir -p /opt/xx/oo
[root@localhost ~]# ls /opt
rh  student  xx

[root@localhost ~]# mkdir -p /opt/a/b/c/d
[root@localhost ~]# ls /opt
a  rh  student  xx

#ls -R选项可以递归目录下所有内容
[root@localhost ~]# ls -R /opt/a
/opt/a:
b

/opt/a/b:
c

/opt/a/b/c:
d
```

#### cd 切换工作目录命令

- cd（英文全拼：change directory）切换目录

命令格式：`cd [-选项] [目录名]`

- 提示：目录名称可以是绝对路径或相对路径，如果不指定目录名称，则切换到当前用户的家目录~
- 常用快捷操作：
  - ~ 表示为家目录-
  - . 表示为当前目录
  - .. 表示上一级目录
  - -可在两路径之间来回切换

#### pwd 打印当前所在目录命令

- pwd（英文全拼：print work directory）打印当前所在的工作目录，执行pwd命令后，可显示当前所在的工作目录的绝对路径名称
- 命令格式：`pwd [-选项]`

```
[root@localhost ~]# cd /opt/a/b/c/d

#打印当前所在目录绝对路径
[root@localhost d]# pwd
/opt/a/b/c/d

#切换到用户家目录
[root@localhost d]# cd ~
[root@localhost ~]# pwd
/root
[root@localhost ~]# cd /opt/a/b/c/d
[root@localhost d]# pwd
/opt/a/b/c/d
[root@localhost d]# cd
[root@localhost ~]# pwd
/root

[root@localhost ~]# cd /bin
[root@localhost bin]# pwd
/bin

[root@localhost bin]# cd /boot
[root@localhost boot]# pwd
/boot
[root@localhost boot]# ls

[root@localhost boot]# cd /dev
[root@localhost dev]# pwd
/dev
[root@localhost dev]# ls

[root@localhost dev]# cd /etc
[root@localhost etc]# pwd
/etc
[root@localhost etc]# ls

[root@localhost etc]# ls /
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

#“.”表示当前所在目录，对于cd命令而言作用不大
[root@localhost etc]# cd .

[root@localhost etc]# cd /opt/a/b/c/d
[root@localhost d]# pwd
/opt/a/b/c/d

#“..”切换到当前目录的上一级目录
[root@localhost d]# cd ..
[root@localhost c]# pwd
/opt/a/b/c

[root@localhost c]# cd ..
[root@localhost b]# pwd
/opt/a/b

[root@localhost b]# cd ..
[root@localhost a]# cd ..
[root@localhost opt]# pwd
/opt

[root@localhost opt]# cd ..
[root@localhost /]# cd ..
[root@localhost /]# cd
[root@localhost ~]# ls

[root@localhost ~]# cd /opt/a/b/c/d
[root@localhost d]# pwd
/opt/a/b/c/d

#"-"可在两个路径之间来回切换
[root@localhost d]# cd /etc/yum
[root@localhost yum]# cd -
/opt/a/b/c/d

[root@localhost d]# pwd
/opt/a/b/c/d

[root@localhost d]# cd -
/etc/yum

[root@localhost yum]# cd -
/opt/a/b/c/d

[root@localhost d]# cd -
/etc/yum
```

#### 绝对路径与相对路径

- 绝对路径：以/（根）为起点，到达你想去的目标目录称为绝对路径
- 相对路径：以当前路径为起点，到达你想去的目标目录

```
#绝对路径以“/”作为起点，到达目标路径
[root@localhost ~]# cd /opt/a/b/c/d
[root@localhost d]# pwd
/opt/a/b/c/d

#切换到上一级目录
[root@localhost c]# cd ..
[root@localhost b]# pwd
/opt/a/b
[root@localhost b]# ls
c

#相对路径以当前路径作为起点到达目标路径
[root@localhost b]# cd c/
[root@localhost c]# pwd
/opt/a/b/c
[root@localhost c]# cd ..
[root@localhost b]# cd ..
[root@localhost a]# cd ..
[root@localhost opt]# pwd
/opt
```

#### rmdir 删除空目录命令

- rmdir（英文全拼：remove directory）删除空目录
- 命令格式：`rmdir [-选项] 目录名`

```
#rmdir只能删除空目录，如果目录下存在数据无法删除
[root@localhost ~]# rmdir /opt/a
rmdir: 删除 "/opt/a" 失败: 目录非空
[root@localhost ~]# ls -R /opt/a
/opt/a:
b

/opt/a/b:
c

/opt/a/b/c:
d

/opt/a/b/c/d:

[root@localhost ~]# rmdir /opt/a/b/c/d
[root@localhost ~]# ls -R /opt/a
/opt/a:
b

/opt/a/b:
c

/opt/a/b/c:

[root@localhost ~]# rmdir /opt/a/b/c
[root@localhost ~]# ls -R /opt/a/b
/opt/a/b:

[root@localhost ~]# rmdir /opt/a/b
[root@localhost ~]# ls -R /opt/a
/opt/a:

[root@localhost ~]# rmdir /opt/a
[root@localhost ~]# ls /opt
rh  student  xx

[root@localhost ~]# rmdir /opt/
rmdir: 删除 "/opt/" 失败: 目录非空
```

#### touch 创建文件命令

- touch 命令用于创建新的空白文件
- 命令格式：`touch [-选项] 文件名`

```
#在当前路径创建空文件
[root@localhost ~]# touch hello
[root@localhost ~]# ls

#在当前路径同时创建多个文件
[root@localhost ~]# touch t1 t2 t3 t4
[root@localhost ~]# ls

#在指定路径同时创建多个文件
[root@localhost ~]# touch /opt/test1 /opt/test2 /opt/test3
[root@localhost ~]# ls /opt
rh  student  test1  test2  test3  xx

#如果存在同名目录时，无法创建
[root@localhost ~]# mkdir test
mkdir: 无法创建目录"test": 文件已存在

#如果存在同名文件时，touch命令没有提示，但原有文件不会被覆盖
[root@localhost ~]# touch t1

#对于目录而言，只有单个目录的时候，“/”可有可无
[root@localhost ~]# ls /opt/
rh  student  test1  test2  test3  xx
[root@localhost ~]# ls /opt
rh  student  test1  test2  test3  xx

#对于目录而言，查看目录下的内容时，必须要有“/”
[root@localhost ~]# ls /opt/xx
oo

#对于文件而言，后边绝对不能有“/”
[root@localhost ~]# ls /opt/test1
/opt/test1
[root@localhost ~]# ls /opt/test1/
ls: 无法访问/opt/test1/: 不是目录
```

#### cp 复制命令

- cp（英文全拼：copy file）用于复制文件或目录，cp命令在复制时也可修改目录或文件名字
- 命令格式：`cp [-选项] 源文件或目录 目标目录`
- 常用选项：
  - `-p` 保留源文件属性不变（如：修改时间、归属关系、权限）
  - `-r` 复制目录（包含该目录下所有的子目录和文件）

```
#复制当前目录文件到/opt目录（相对路径方式复制）
[root@localhost ~]# cp t1 /opt/
[root@localhost ~]# ls /opt
rh  student  t1  test1  test2  test3  xx

#复制文件到/opt目录（绝对路径方式复制）
[root@localhost ~]# cp /root/t2 /opt
[root@localhost ~]# ls /opt
rh  student  t1  t2  test1  test2  test3  xx

#同时复制多个文件
[root@localhost ~]# cp t3 t4 /opt/
[root@localhost ~]# ls /opt

#创建目录
[root@localhost ~]# mkdir abc

#使用-r对目录执行复制
[root@localhost ~]# cp -r abc /opt
[root@localhost ~]# ls /opt

#同时复制多个目录
[root@localhost ~]# mkdir abc1 abc2 abc3
[root@localhost ~]# cp -r abc1 abc2 abc3 /opt
[root@localhost ~]# ls /opt

#复制hello文件到/opt并改名为hello.txt
[root@localhost ~]# cp hello /opt/hello.txt
[root@localhost ~]# ls /opt

#复制xxxx目录到/opt并改名xxoo
[root@localhost ~]# mkdir xxxx
[root@localhost ~]# cp -r xxxx /opt/xxoo
[root@localhost ~]# ls /opt

#使用“.”配合cp命令执行复制
[root@localhost ~]# cd /etc/sysconfig/network-scripts/
[root@localhost network-scripts]# pwd
/etc/sysconfig/network-scripts

[root@localhost network-scripts]# cp /root/t1 .
[root@localhost network-scripts]# ls

#操持属性不变复制文件
[root@localhost ~]# cp -p anaconda-ks.cfg /opt
cp：是否覆盖"/opt/anaconda-ks.cfg"？ y                         
[root@localhost ~]# ls -l /opt/anaconda-ks.cfg 
-rw-------. 1 root root 1800 3月  13 17:34 /opt/anaconda-ks.cfg

#对比以上两个文件的详细属性信息（最后一次修改时间）
[root@localhost ~]# ls -l anaconda-ks.cfg 
-rw-------. 1 root root 1800 3月  13 17:34 anaconda-ks.cfg

#这两个操作代表什么意思？
[root@localhost ~]# cp -r xxxx /mnt/oooo  #拷贝并改名
[root@localhost ~]# cp -r xxxx /mnt/oooo  #拷贝
```

#### mv 移动命令

- mv（英文全拼：move file）用于移动文件或目录到其他位置，也可用于修改目录或文件名
- 命令格式：`mv [-选项] 源文件... 目标路径`

```
#移动当前路径hello文件到/mnt目录
[root@localhost ~]# mv hello /mnt
[root@localhost ~]# ls /mnt
hello  home  oooo  test

#同时移动多个文件
[root@localhost ~]# mv t1 t2 t3 t4 /mnt
[root@localhost ~]# ls /mnt
hello  home  oooo  student1  t1  t2  t3  t4  test

#移动/opt目录下文件到/mnt
root@localhost ~]# mv /opt/test1 /opt/test2 /opt/test3 /mnt/
[root@localhost ~]# ls /mnt
hello  home  oooo  student1  t1  t2  t3  t4  test  test1  test2  test3

#移动目录
[root@localhost ~]# mv student1 /mnt
[root@localhost ~]# ls /mnt
hello  home  oooo  student1  test

#移动文件并改名
[root@localhost ~]# mv hello.txt /media/hello
[root@localhost ~]# ls /media/
hello

#移动目录并改名
[root@localhost ~]# mv test /media/testxx
[root@localhost ~]# ls /media/
hello  testxx
```

#### cat 查看文件内容命令

- cat （英文全拼：concatenate）命令用于查看文本文件内容
- 命令格式：`cat [选项] 文件名`
- 常用选项
  - `-n` 查看文件时以行号的形式显示文件内容

```
#查看文件内容
[root@localhost ~]# cat anaconda-ks.cfg 
[root@localhost ~]# cat initial-setup-ks.cfg 
[root@localhost ~]# cat /etc/hosts

#查看网卡文件内容
[root@localhost ~]# cat /etc/sysconfig/network-scripts/ifcfg-ens32 
...
NAME="ens32"   //网卡名
UUID="16085f4c-f690-4058-b29e-d55c73387026"
DEVICE="ens32"
ONBOOT="yes"
IPADDR="192.168.0.50"     //网卡IP地址
PREFIX="24"			      //子网掩码
GATEWAY="192.168.0.254"   //网管
DNS1="114.114.114.114"    //DNS

#查看当前系统用户基本信息文件内容
[root@localhost ~]# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin

#查看当前系统主机名配置文件内容
[root@localhost ~]# cat /etc/hostname
localhost.localdomain

#查看当前系统版本信息文件内容
[root@localhost ~]# cat /etc/redhat-release 
CentOS Linux release 7.6.1810 (Core) 

#查看当前系统开机自动挂载配置文件内容
[root@localhost ~]# cat /etc/fstab

#查看系统组基本信息文件内容
[root@localhost ~]# cat /etc/group

#使用“-n”以行号形式显示文件内容
[root@localhost ~]# cat -n /etc/passwd
[root@localhost ~]# cat -n /etc/hostname
[root@localhost ~]# cat -n /etc/fstab
[root@localhost ~]# cat -n /etc/group
[root@localhost ~]# cat -n /etc/services 
```

#### less命令

- less工具是对文件的输出进行分页显示的工具，常用于查看内容量较大的文件
- 命令格式：`less [-选项] 文件`
- 常用选项：
  - `-N` #以行号形式显示文件内容
- 使用技巧：
  - 键盘上下键逐行查看
  - <kbd>PgDn</kbd> ：向上翻一页（<kbd>Fn</kbd> + 上键）
  - <kbd>PgUp</kbd> ：向下翻一页（<kbd>Fn</kbd> + 下键）
  - /字符串 ：搜索指定字符串（n从上向下搜索，N从下向上搜索）
  - G：直接跳转到文件最后一行
  - gg：直接跳转到文件行首
  - q ：退出

```
[root@localhost ~]# less -N /etc/services
```

#### head与tail命令

- head命令：用来显示文件开头部分内容，默认显示文件开头10行内容
- 命令格式：`head [选项] 参数`
- 常用选项：
  - `-n<行数>` 指定显示的行数
  - `-f` 动态显示

```
[root@localhost ~]# head /etc/passwd
[root@localhost ~]# head /etc/fstab
[root@localhost ~]# head /etc/group
[root@localhost ~]# head /etc/hostname
[root@localhost ~]# head /etc/hosts
[root@localhost ~]# head /etc/sysconfig/network-scripts/ifcfg-ens32 

#查看存放DNS配置文件信息
[root@localhost ~]# head /etc/resolv.conf 

#使用-n指定显示文件前多少行内容
[root@localhost ~]# head -n 5 /etc/passwd
[root@localhost ~]# head -n 6 /etc/passwd
[root@localhost ~]# head -n 15 /etc/passwd
[root@localhost ~]# head -n 20 /etc/passwd
```

- tail命令：用来显示文件末尾部分内容，默认显示文件末尾10行内容
- 命令格式：`tail [选项] 参数`
- 常用选项：`-n<行数>` 指定显示的行数 `-f` 动态显示

```
[root@localhost ~]# tail /etc/passwd

#使用“-n”指定显示文件末尾多少行内容
[root@localhost ~]# tail -n 5 /etc/passwd
[root@localhost ~]# tail -n 5 /etc/sysconfig/network-scripts/ifcfg-ens32 
IPADDR="192.168.0.50"
PREFIX="24"
GATEWAY="192.168.0.254"
DNS1="114.114.114.114"
IPV6_PRIVACY="no"

#动态查看文件内容
[root@localhost ~]# touch t1
root@localhost ~]# tail -f t1

#另开一个终端向文件写入内容
[root@localhost ~]# echo 123 > t1
```

#### rm删除命令

- rm（英文全拼：remove）命令用于删除文件或者目录。
- 命令格式：`rm [-选项…] 目录或文件…`
- 常用选项
  - `-f` 强制删除
  - `-r` 删除目录
  - “*”特殊字符：系统常用符号，用来代表任意字符

```
[root@localhost ~]# ls /opt
abc  abc1  abc2  abc3  anaconda-ks.cfg  hello.txt  home  rh  student  t1  t2  t3  t4  xx  xxoo

[root@localhost ~]# ls /mnt
hello  home  oooo  student1  t1  t2  t3  t4  test  test1  test2  test3

#删除指定目录下文件
[root@localhost ~]# rm /opt/anaconda-ks.cfg 
rm：是否删除普通文件 "/opt/anaconda-ks.cfg"？y  #默认需要确认（y|n）

#查看文件是否被成功删除
[root@localhost ~]# ls /opt
abc  abc1  abc2  abc3  hello.txt  home  rh  student  t1  t2  t3  t4  xx  xxoo

[root@localhost ~]# rm /opt/hello.txt 
rm：是否删除普通空文件 "/opt/hello.txt"？y

#同时删除目录下指定文件
[root@localhost ~]# rm /opt/t1 /opt/t2 /opt/t3 /opt/t4
rm：是否删除普通空文件 "/opt/t1"？y
rm：是否删除普通空文件 "/opt/t2"？y
rm：是否删除普通空文件 "/opt/t3"？y
rm：是否删除普通空文件 "/opt/t4"？y

#查看文件是否被成功删除
[root@localhost ~]# ls /opt
abc  abc1  abc2  abc3  home  rh  student  xx  xxoo

#使用“-f”强制删除文件（无需确认，直接删除）
[root@localhost ~]# rm -f /mnt/hello
[root@localhost ~]# ls /mnt
home  oooo  student1  t1  t2  t3  t4  test  test1  test2  test3

#同时强制删除多个文件
[root@localhost ~]# rm -f /mnt/t1 /mnt/t2 /mnt/t3 /mnt/t4
[root@localhost ~]# ls /mnt

#删除目录
[root@localhost ~]# rm  -r /opt/abc
rm：是否删除目录 "/opt/abc"？y

[root@localhost ~]# ls /opt
abc1  abc2  abc3  home  rh  student  xx  xxoo

#同时删除多个目录
[root@localhost ~]# rm -r /opt/abc1 /opt/abc2 /opt/abc3
rm：是否删除目录 "/opt/abc1"？y
rm：是否删除目录 "/opt/abc2"？y
rm：是否删除目录 "/opt/abc3"？y

[root@localhost ~]# ls /opt
home  rh  student  xx  xxoo

#同时强制删除多个目录
[root@localhost ~]# rm -rf /opt/home /opt/student /opt/xx /opt/xxoo
[root@localhost ~]# ls /opt
rh

#创建目录与文件
[root@localhost ~]# touch /opt/t1
[root@localhost ~]# mkdir /opt/test
[root@localhost ~]# ls /opt
rh  t1  test

#rm命令在删除目录时，包含改目录及目录下所有数据全部删除
[root@localhost ~]# rm -rf /opt/
[root@localhost ~]# ls /

[root@localhost ~]# ls /mnt
home  oooo  student1  test  test1  test2  test3

#使用“*”通配任意所有字符，删除/mnt目录下所有数据
[root@localhost ~]# rm -rf /mnt/*
[root@localhost ~]# ls /mnt
```

#### 软连接与硬连接

- Linux中的链接文件类似于windows中的快捷方式
- 软连接特点：软连接可以跨分区，可以对目录进行链接，源文件删除后，链接文件不可用
- 软连接命令格式：`ln -s 源文件路径 目标路径`
- 注意：创建链接时一定要写目录或文件的绝对路径，哪怕是在当前路径下，也要写绝对路径·

```
[root@localhost ~]# touch hello.soft
[root@localhost ~]# ls

#创建软连接（必须要绝对路径创建）
[root@localhost ~]# ln -s /root/hello.soft /opt
[root@localhost ~]# ls /opt

#查看连接文件详细属性
[root@localhost ~]# ls -l /opt/hello.soft 
lrwxrwxrwx. 1 root root 16 3月  21 14:28 /opt/hello.soft -> /root/hello.soft
#提示：链接文件的权限最终取决于源文件的权限

#普通用户验证
[lisi@localhost ~]$ ls /opt
hello.soft
[lisi@localhost ~]$ ls -l /opt/hello.soft 
lrwxrwxrwx. 1 root root 16 3月  21 14:28 /opt/hello.soft -> /root/hello.soft
[lisi@localhost ~]$ cat /opt/hello.soft 
cat: /opt/hello.soft: 权限不够
#提示：由于源文件存放于/root目录下，而普通用户对/root目录没有任何权限，所以普通用户无法查看

#删除源文件
[root@localhost ~]# rm -f /root/hello.soft 
[root@localhost ~]# ls

#山粗源文件后，软链接文件不可用
[root@localhost ~]# ls -l /opt/hello.soft 
lrwxrwxrwx. 1 root root 16 3月  21 14:28 /opt/hello.soft -> /root/hello.soft

#创建文件并创建软连接
[root@localhost ~]# touch hello.soft
[root@localhost ~]# ln -s /root/hello.soft /opt

[root@localhost ~]# ls -l /opt/hello.soft 
lrwxrwxrwx. 1 root root 16 3月  21 14:39 /opt/hello.soft -> /root/hello.soft

#删除链接文件后，源文件仍然可用
[root@localhost ~]# rm -f /opt/hello.soft 
[root@localhost ~]# ls
[root@localhost ~]# cat hello.soft 

#对目录创建软连接
[root@localhost ~]# ln -s /root/test1 /opt/

[root@localhost ~]# ls -ld /opt/test1
lrwxrwxrwx. 1 root root 11 3月  21 14:44 /opt/test1 -> /root/test1

3创建链接时一定要写目录或文件的绝对路径，哪怕是在当前路径下，也要写绝对路径
[root@localhost ~]# ln -s hello.soft /opt
[root@localhost ~]# ls /opt
hello.soft  test1
[root@localhost ~]# ls -l /opt/hello.soft 
lrwxrwxrwx. 1 root root 10 3月  21 14:47 /opt/hello.soft -> hello.soft
```

- 硬链接特点：硬连接不可以跨分区，不可以对目录进行链接，源文件删除后，链接文件仍然可用
- 硬连接命令格式：`ln 源文件路径 目标路径`

```
#创建文件，并创建硬连接
[root@localhost ~]# touch hello.hard
[root@localhost ~]# ln /root/hello.hard /opt/
[root@localhost ~]# ls /opt
hello.hard  hello.soft  test1

#向硬连接的源文件写入内容
root@localhost ~]# echo 123 > /root/hello.hard 

#查看源文件内容
[root@localhost ~]# cat /root/hello.hard 
123

#查看链接文件内容，以同步更新
[root@localhost ~]# cat /opt/hello.hard 
123

#向链接文件写入内容，查看源文件以同步更新
[root@localhost ~]# echo xx >> /opt/hello.hard 

#擦看源文件，以同步更新
[root@localhost ~]# cat /root/hello.hard 
123
xx

#硬连接文件的特点可以保持文件属性不发生改变
[root@localhost ~]# ls -l /root/hello.hard 
-rw-r--r--. 2 root root 7 3月  21 14:55 /root/hello.hard
[root@localhost ~]# ls -l /opt/hello.hard 
-rw-r--r--. 2 root root 7 3月  21 14:55 /opt/hello.hard

#并且硬连接文件的i节点号相同
[root@localhost ~]# ls -i /root/hello.hard 
33711090 /root/hello.hard
[root@localhost ~]# ls -i /opt/hello.hard 
33711090 /opt/hello.hard

#硬连接不允许对目录进行连接
root@localhost ~]# ln /root/test1 /opt
ln: "/root/test1": 不允许将硬链接指向目录

#硬连接源文件删除后，链接文件仍然可用
[root@localhost ~]# rm -f /root/hello.hard 
[root@localhost ~]# cat /opt/hello.hard 
123
xx

#向硬连接文件写入内容
[root@localhost ~]# echo  abc >> /opt/hello.hard 
[root@localhost ~]# cat /opt/hello.hard 
123
xx
abc

#硬连接不允许跨分区
[root@localhost ~]# lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0   20G  0 disk 
├─sda1            8:1    0    1G  0 part /boot
└─sda2            8:2    0   19G  0 part 
  ├─centos-root 253:0    0   17G  0 lvm  /
  └─centos-swap 253:1    0    2G  0 lvm  [SWAP]
sr0              11:0    1  4.3G  0 rom  
[root@localhost ~]# ln /root/hello.soft /boot
ln: 无法创建硬链接"/boot/hello.soft" => "/root/hello.soft": 无效的跨设备连接
```