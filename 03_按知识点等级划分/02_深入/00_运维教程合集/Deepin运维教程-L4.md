---
title: Deepin运维教程-L4
description: 
published: true
date: 2022-06-28T05:54:52.286Z
tags: 运维
editor: markdown
dateCreated: 2022-06-28T05:53:48.775Z
---

# Deepin运维教程-L4
#### 用户账号管理

- 用户账号的作用：用户账号可用来登录系统，可以实现访问控制
- 用户模板目录：/etc/skel/

```shell
[root@localhost ~]# ls -a /etc/skel/
.  ..  .bash_logout  .bash_profile  .bashrc  .mozilla

[root@localhost ~]# cd /etc/skel/
[root@localhost skel]# vim prompt
```

#### useradd创建用户

- useradd 命令用于创建新的用户
- 命令格式：useradd [-选项] 用户名
- 常用选项：
  - -u 指定用户UID
  - -d 指定用户家目录
  - -c 用户描述信息
  - -g 指定用户基本组
  - -G 指定用户附加组
  - -s 指定用户的shell

```shell
[root@localhost ~]# useradd user1

#创建用户并指定用户的UID
[root@localhost ~]# useradd -u 1100 user2

#创建用户并指定用户的家目录
root@localhost ~]# useradd -d /opt/user3 user3

#创建用户并指定UID与用户描述信息
[root@localhost ~]# useradd -u 1400  -c yunwei user4

#创建test组
[root@localhost ~]# groupadd test

#创建用户指定用户UID、描述信息、基本组
[root@localhost ~]# useradd -u 1500 -c xxoo@163.com -g test user5
[root@localhost ~]# id user5

#创建用户指定用户UID、描述信息、附加组
[root@localhost ~]# useradd -u 1600 -c yunwei -G test xiaozhang
[root@localhost ~]# id xiaozhang
uid=1600(xiaozhang) gid=1600(xiaozhang) 组=1600(xiaozhang),1401(test)

#/sbin/nologin ：禁止用户登录系统
[root@localhost ~]# useradd -u 1800 -c test -s /sbin/nologin user8
user8:x:1800:1800:test:/home/user8:/sbin/nologin
```

#### id命令

- id 命令用于查看系统用户和用户所在组的信息
- 命令格式：id [-选项] [用户名]

```shell
[root@localhost ~]# id user1
uid=1001(user1) gid=1001(user1) 组=1001(user1)
```

#### /etc/passwd用户信息文件

用户的基本信息存放在/etc/passwd文件

```shell
[root@localhost ~]# vim /etc/passwd
root:x:0:0:root:/root:/bin/bash
#每个字段含义解释：用户名:密码占位符:UID:基本组GID:用户描述信息:家目录:解释器程序
UID：0 超级用户
UID：1-499 系统伪用户，不能登录系统并且没有家目录
UID：500-65535 普通用户
```

组：

基本组（初始组）：一个用户只允许有一个基本组

附加组（在基本组之外组）：一个用户可以允许有多个附加组

用户--->shell程序--->内核--->硬件

#### /etc/default/useradd文件

/etc/default/useradd 存放用户默认值信息

```shell
[root@localhost ~]# vim /etc/default/useradd
# useradd defaults file
GROUP=100 #用户默认组
HOME=/home #用户家目录
INACTIVE=-1 #密码过期宽限天数（/etc/shadow文件第7个字段）
EXPIRE= #密码失效时间（/etc/shadow文件第8个字段）
SHELL=/bin/bash #默认使用的shell
SKEL=/etc/skel #模板目录
CREATE_MAIL_SPOOL=yes #是否建立邮箱
```

#### /var/spool/mail/用户邮件目录

```shell
[root@localhost ~]# ls /var/spool/mail/
laowang  lisi  rpc  user1  user2  user3  user4  user5  user8  xiaozhang
```

#### passwd设置用户密码

- passwd命令用于设置用户密码
- 命令格式：passwd [-选项] [用户名]
- 密码规范：长度不能少于8个字符，复杂度（数字、字母区分大小写，特殊字符），普通用户
- 密码规范：本次修改的密码不能和上次修改的密码太相近 123xxoo...A
- 常用选项
  - -S 查看密码信息
  - -l 锁定用户密码
  - -u 解锁用户密码
  - -d 删除密码
  - --stdin 通过管道方式设置用户密码
- 非交互设置用户密码
- 命令格式：echo "密码" | passwd --stdin 用户名

```shell
#设置用户密码
[root@localhost ~]# passwd user1
更改用户 user1 的密码 。
新的 密码：1
无效的密码： 密码是一个回文
重新输入新的 密码：1
passwd：所有的身份验证令牌已经成功更新。

#使用user1用户登录系统
[user1@localhost ~]$ ls
prompt
[user1@localhost ~]$ cat prompt 
不允许随便修改系统xx文件！
有问题可联系管理员邮箱：xxoo@163.com

#查看用户密码信息
[root@localhost ~]# passwd -S user1

#锁定用户当密码
[root@localhost ~]# passwd -l user2
锁定用户 user2 的密码 。
passwd: 操作成功

[root@localhost ~]# passwd -S user2
user2 LK 2021-04-10 0 99999 7 -1 (密码已被锁定。)

#解锁用户密码
[root@localhost ~]# passwd -u user2
解锁用户 user2 的密码。
passwd: 操作成功

#删除用户密码
[root@localhost ~]# passwd -d user2
清除用户的密码 user2。
passwd: 操作成功

#非交互设置用户密码
[root@localhost ~]# echo 1 | passwd --stdin laowang
更改用户 laowang 的密码 。
passwd：所有的身份验证令牌已经成功更新。
```

#### /etc/shadow用户密码文件

- 用户的密码信息存放在/etc/shadow文件中，该文件默认任何人都没有任何权限（不包括root）

```shell
[root@localhost ~]# vim /etc/shadow
root:$6$1ji5e8yglrZWAcI6$FONKr3qebZufQ.u0Mf/MbipzGw/MVvxS.vgXcy/duc4b/GU0U7tfe37wPQ4XJEXstqBuwvaJqq2/kY/g/783u/::0:99999:7:::
#每个字段含义解释：
第一字段：用户名
第二字段：密码加密字符串，加密算法为SHA512散列加密算法，如果密码位是“*”或者“!!”表示密码已过期
第三个字段：密码最后一次修改日期，日期从1970年1月1日起，每过一天时间戳加1
第四个字段：密码修改的期限，如果该字段为0表示随时可以修改密码，例如：该字段为10，代表10天之内不可以修改密
第五个字段：密码有效期
第六个字段：密码到期前警告时间（和第五个字段相比）
第七个字段：密码过期后的宽限天数（和第五个字段相比）
第八个字段：账号失效时间，日期从1970年1月1日起
第九个字段：保留

#chage命硬用于修改/etc/shadow文件信息，修改文件内容第三个字段（密码最后一次修改时间）
[root@localhost ~]# chage -d 0 user8
```

#### su命令

- su命令用于切换当前用户身份到其他用户身份
- 命令格式：su [-选项] [用户名]

```shell
#只切换用户身份，环境没有改变
[root@localhost ~]# su user1
[user1@localhost root]$ ls
ls: 无法打开目录.: 权限不够
[user1@localhost root]$ cd
[user1@localhost ~]$ exit
exit

#切换用户身份，连同环境一起切换
[root@localhost ~]# su - user1
上一次登录：六 4月 10 16:54:40 CST 2021pts/1 上
[user1@localhost ~]$ pwd
/home/user1

#普通用户切换为root（需要输入root用户的密码）
[user1@localhost ~]$ su - root
密码：
上一次登录：六 4月 10 16:05:17 CST 2021从 192.168.0.1pts/2 上
```

#### usermod修改用户属性

- usermod 命令用于修改已存在用户的基本信息
- 命令格式：usermod [-选项] 用户名
- 常用选项：
  - -u 修改用户UID
  - -d 修改用户家目录
  - -g 修改用户基本组
- -c 修改用户描述信息
  - -G 添加用户附加组
  - -s 修改用户shell

```shell
#修改用户UID（用户如果以登录系统，不允许修改）
[root@localhost ~]# usermod -u 1111 user1
[root@localhost ~]# id user1
uid=1111(user1) gid=1001(user1) 组=1001(user1)

#修改用户描述信息
[root@localhost ~]# usermod -c xxoo@163.com user8

#修改用户的附加组
[root@localhost ~]# usermod -G test user8
[root@localhost ~]# id user8
uid=1800(user8) gid=1800(user8) 组=1800(user8),1401(test)

#修改用户的解释器
[root@localhost ~]# usermod -s /bin/bash user8
```

#### userdel删除用户

- userdel 用于删除给定的用户以及与用户相关的文件，该命令若不加选项仅删除用户账号，不删除用户相关文件
- 命令格式：userdel [-选项] 用户名
- 常用选项：
  - -r 删除用户同时，删除与用户相关的所有文件

```shell
#删除用户，仅删除账号，不删除家目录
[root@localhost ~]# userdel user8
[root@localhost ~]# ls /home
laowang  lisi  user1  user2  user4  user5  user8  xiaozhang
[root@localhost ~]# id user8
id: user8: no such user

#删除用户，连同用户家目录一并删掉
[root@localhost ~]# userdel -r user4
[root@localhost ~]# ls /home
laowang  lisi  user1  user2  user5  user8  xiaozhang
[root@localhost ~]# id user4
id: user4: no such user
```

#### groupadd添加新组

- groupadd 用于创建一个新的工作组，新组的信息将被添加到/etc/group文件中
- 命令格式：groupadd [-选项] 组名
- 常用选项：
  - -g GID #指定组的GID

```shell
#创建组
[root@localhost ~]# groupadd -g 1555 student
[root@localhost ~]# cat /etc/group
```

#### /etc/group组信息文件

- 组信息存放在/etc/group文件中

```shell
[root@localhost ~]# vim /etc/group
root:x:0:
#每个字段含义解释：组名:组密码占位符:GID:组中附加用户
```

#### /etc/gshadow组密码文件

- 组密码信息存放在/etc/gshadow文件中

```shell
[root@localhost ~]# vim /etc/gshadow
root:::
#每个字段含义解释：组名:组密码:组内管理员:组中附加用户
```

#### groupmod修改组属性

- groupmod 用于修改指定工作组属性
- 命令格式：groupmod [-选项] 组名
- 常用选项：
  - -g GID #修改组的GID
  - -n 新组名 #修改组名

```shell
#修改组名
[root@localhost ~]# groupmod -n stugrp student

#修改组GID
root@localhost ~]# groupmod -g 1666 stugrp
```

#### gpasswd组管理命令

- gpasswd 是Linux工作组文件/etc/group和/etc/gshadow管理工具，用于将用户添加到组或从组中删除
- 命令格式：gpasswd [-选项] 用户名 组名
- 常用选项：
  - -a #将用户添加到工作组
  - -d #将用户从工作组中删除

```shell
#创建用户
[root@localhost ~]# useradd hary
[root@localhost ~]# useradd tom
[root@localhost ~]# useradd natasha
[root@localhost ~]# useradd kenji
[root@localhost ~]# useradd jack

#讲用户加入到组
[root@localhost ~]# gpasswd -a hary stugrp
正在将用户“hary”加入到“stugrp”组中
[root@localhost ~]# gpasswd -a tom stugrp
正在将用户“tom”加入到“stugrp”组中
[root@localhost ~]# gpasswd -a kenji stugrp
正在将用户“kenji”加入到“stugrp”组中
[root@localhost ~]# gpasswd -a natasha stugrp
正在将用户“natasha”加入到“stugrp”组中
[root@localhost ~]# gpasswd -a jack stugrp
正在将用户“jack”加入到“stugrp”组中
[root@localhost ~]# 

#查看组文件信息
[root@localhost ~]# cat /etc/group
stugrp:x:1666:hary,tom,kenji,natasha,jack

#将用户从组中删除
root@localhost ~]# gpasswd -d tom stugrp
[root@localhost ~]# gpasswd -d hary stugrp
正在将用户“hary”从“stugrp”组中删除
[root@localhost ~]# gpasswd -d jack stugrp
正在将用户“jack”从“stugrp”组中删除
[root@localhost ~]# gpasswd -d kenji stugrp
正在将用户“kenji”从“stugrp”组中删除
[root@localhost ~]# cat /etc/group
```

#### groupdel删除组

- groupdel 用于删除指定工作组
- 命令格式：groupdel 组名

```shell
[root@localhost ~]# groupdel stugrp
```

#### chmod权限管理

- chmod（英文全拼：change mode）设置用户对文件的权限
- 命令格式：chmod [-选项] 归属关系+-=权限类别 文件...
- root用户可以修改任何文件和目录的权限
- 文件所有者
- 常用选项：
  - -R 递归修改，包含目录下所有的子文件与子目录
- 归属关系：u 所有者 g 所属组 o 其他人
- 权限类别： r 读取 w 写入 x 执行 - 没有权限
- 操作：+ 添加权限 - 去除权限 = 重新定义权限
- 权限数字表示：r ---- 4 w ---- 2 x ---- 1 0 没有权限

```shell
#查看文件详细属性
[root@localhost ~]# ll hello
-rw-r--r--. 1 root root 426 3月  28 15:00 hello

#为文件所有者添加执行权限
[root@localhost ~]# chmod u+x hello
[root@localhost ~]# ll hello
-rwxr--r--. 1 root root 426 3月  28 15:00 hello

#为文件所属组添加写权限
[root@localhost ~]# chmod g+w hello
[root@localhost ~]# ll hello
-rwxrw-r--. 1 root root 426 3月  28 15:00 hello

#为文件其他人添加写权限
[root@localhost ~]# chmod o+w hello
[root@localhost ~]# ll hello
-rwxrw-rw-. 1 root root 426 3月  28 15:00 hello

#使用（逗号）可以同时为多个用户授权
[root@localhost ~]# chmod g+x,o+x hello
[root@localhost ~]# ll hello
-rwxrwxrwx. 1 root root 426 3月  28 15:00 hello

#去除所有者执行权限
[root@localhost ~]# chmod u-x hello
[root@localhost ~]# ll hello
-rw-rwxrwx. 1 root root 426 3月  28 15:00 hello

#去除所属组执行权限
[root@localhost ~]# chmod g-x hello
[root@localhost ~]# ll hello
-rw-rw-rwx. 1 root root 426 3月  28 15:00 hello

#去除其他人执行权限
[root@localhost ~]# chmod o-x hello
[root@localhost ~]# ll hello
-rw-rw-rw-. 1 root root 426 3月  28 15:00 hello

#同时去除ugo写权限
[root@localhost ~]# chmod u-w,g-w,o-w hello
[root@localhost ~]# ll hello
-r--r--r--. 1 root root 426 3月  28 15:00 hello

#重新定义所有者权限
[root@localhost ~]# chmod u=rwx hello
[root@localhost ~]# ll hello
-rwxr--r--. 1 root root 426 3月  28 15:00 hello

#重新定义所属组权限
[root@localhost ~]# chmod g=rwx hello
[root@localhost ~]# ll hello
-rwxrwxr--. 1 root root 426 3月  28 15:00 hello

#重新定义其他人权限
[root@localhost ~]# chmod o=rwx hello
[root@localhost ~]# ll hello
-rwxrwxrwx. 1 root root 426 3月  28 15:00 hello

#创建目录并设置目录权限
[root@localhost ~]# mkdir /test
[root@localhost ~]# ll -d /test
drwxr-xr-x. 2 root root 6 4月  11 14:30 /test

#为目录所属组添加写权限
[root@localhost ~]# chmod g+w /test
[root@localhost ~]# ll -d /test
drwxrwxr-x. 2 root root 6 4月  11 14:30 /test

#为目录其他人添加写权限
[root@localhost ~]# chmod o+w /test
[root@localhost ~]# ll -d /test
drwxrwxrwx. 2 root root 6 4月  11 14:30 /test
[root@localhost ~]# 

#重新定义所有用户权限
[root@localhost ~]# chmod u=rwx,g=rx,o=rx /test
[root@localhost ~]# ll -d /test
drwxr-xr-x. 2 root root 6 4月  11 14:30 /test

#同时为所有用户定义相同权限
[root@localhost ~]# chmod ugo=rwx /test
[root@localhost ~]# ll -d /test
drwxrwxrwx. 2 root root 21 4月  11 14:37 /test

#权限数字定义方式
[root@localhost ~]# ll hello
-rwxrwxrwx. 1 root root 426 3月  28 15:00 hello
所有者：rwx   4+2+1=7
所属组：r     4
其他人：r     4
[root@localhost ~]# chmod 744 hello
[root@localhost ~]# ll hello
-rwxr--r--. 1 root root 426 3月  28 15:00 hello

所有者：rw 4+2=6
所属组：rw 4+2=6
其他人：--- 0
[root@localhost ~]# chmod 660 hello
[root@localhost ~]# ll hello
-rw-rw----. 1 root root 426 3月  28 15:00 hello

所有者：rwx 4+2+1=7
所属组：wx  2+1=3
其他人：--- 0
[root@localhost ~]# touch /hello.txt
[root@localhost ~]# ll /hello.txt 
-rw-r--r--. 1 root root 0 4月  11 14:45 /hello.txt
[root@localhost ~]# chmod 730 /hello.txt 
[root@localhost ~]# ll /hello.txt 
-rwx-wx---. 1 root root 0 4月  11 14:45 /hello.txt

#去除所有用户权限
[root@localhost ~]# chmod 000 /hello.txt 
[root@localhost ~]# ll /hello.txt 
----------. 1 root student 0 4月  11 14:45 /hello.txt

#递归修改目录下所有子文件与子目录权限
[root@localhost ~]# ll -d /test
drwxrwxrwx. 2 root root 21 4月  11 14:37 /test

[root@localhost ~]# mkdir /test/xxoo
[root@localhost ~]# ll -d /test/xxoo/
drwxr-xr-x. 2 root root 6 4月  11 14:54 /test/xxoo/

[root@localhost ~]# ll /test/abc.txt 
-rw-r--r--. 1 root root 0 4月  11 14:37 /test/abc.txt
#默认用户在该目录下创建文件权限与父目录不一致

#递归修改目录下所有子文件与子目录权限
[root@localhost ~]# chmod -R 777 /test
[root@localhost ~]# ll /test/abc.txt 
-rwxrwxrwx. 1 root root 0 4月  11 14:37 /test/abc.txt
[root@localhost ~]# ll -d /test/xxoo
drwxrwxrwx. 2 root root 6 4月  11 14:54 /test/xxoo

#深入理解权限，
[root@localhost ~]# mkdir /test1
[root@localhost ~]# chmod 777 /test1
[root@localhost ~]# ll -d /test1
drwxrwxrwx. 2 root root 6 4月  11 14:57 /test1

#在该目录下创建文件与目录
[root@localhost ~]# touch /test1/root.txt
[root@localhost ~]# mkdir /test1/rootbak
[root@localhost ~]# chmod o=rx /test1
[root@localhost ~]# ll -d /test1
drwxrwxr-x. 2 root root 6 4月  11 14:59 /test1
[root@localhost ~]# touch /test1/root.txt

#普通用户对该目录如果拥有rwx权限是可以删除该目录下任何用户创建的文件（包括root）
[user1@localhost ~]$ cd /test1
[user1@localhost test1]$ ls
root.txt
[user1@localhost test1]$ ll root.txt 
-rw-r--r--. 1 root root 0 4月  11 14:57 root.txt
[user1@localhost test1]$ rm -rf root.txt 
[user1@localhost test1]$ ls
rootbak
[user1@localhost test1]$ rm -rf rootbak/
[user1@localhost test1]$ ls
[user1@localhost test1]$ ll -d /test1
drwxrwxrwx. 2 root root 6 4月  11 14:59 /test1

总结：
1.用户对文件拥有写权限可以增加/修改/删除文件里内容，并不能删除文件，删除文件取决于对文件的父目录有没有rwx权限
2.用户对目录拥有rwx权限可以查看/创建/修改/删除目录下的文件
```

#### umask预设权限

- umask用于显示或设置创建文件的权限掩码
- 命令格式：umask [-p] [-S] [mode]

```shell
root@localhost ~]# mkdir /test2
[root@localhost ~]# ll -d /test2
drwxr-xr-x. 2 root root 6 4月  11 15:05 /test2
[root@localhost ~]# umask --help
umask: 用法:umask [-p] [-S] [模式]

#查看目录默认权限掩码，以数字形式显示
[root@localhost ~]# umask -p
umask 0022

#查看目录默认权限掩码，以字母形式显示
[root@localhost ~]# umask -S
u=rwx,g=rx,o=rx

#设置目录默认权限掩码，为所属组添加写权限
[root@localhost ~]# umask g+w 
[root@localhost ~]# mkdir /test3
[root@localhost ~]# ll -d /test3
drwxrwxr-x. 2 root root 6 4月  11 15:09 /test3

#去除目录默认权限掩码
[root@localhost ~]# umask g-w 
[root@localhost ~]# mkdir /test4
[root@localhost ~]# ll -d /test4
drwxr-xr-x. 2 root root 6 4月  11 15:10 /test4
```

#### chown归属关系管理

- chown（英文全拼：change owner）用于设置文件的所有者和所属组关系
- 命令格式：
  - chown [-选项] 所有者:所属组 文档 #同时修改所有者和所属组身份
  - chown [-选项] 所有者 文档 #只修改所有者身份
  - chown [-选项] :所属组 文档 #只修改所属组身份
- 常用选项：
  - -R 递归修改

```shell
#创建文件
[root@localhost ~]# chmod 744 /hello.txt 
[root@localhost ~]# ll /hello.txt 
-rwxr--r--. 1 root student 0 4月  11 14:45 /hello.txt

#修改文件所有者为user1用户
[root@localhost ~]# chown user1 /hello.txt 
[root@localhost ~]# ll /hello.txt 
-rwxr--r--. 1 user1 student 0 4月  11 14:45 /hello.txt

#修改文件所有者与所属组为lisi
[root@localhost ~]# chown lisi:lisi /hello.txt 
[root@localhost ~]# ll /hello.txt 
-rwxr--r--. 1 lisi lisi 4 4月  11 15:26 /hello.txt

#创建目录
[root@localhost ~]# mkdir /test5
[root@localhost ~]# ll -d /test5
drwxr-xr-x. 2 root root 6 4月  11 15:30 /test5

#修改目录所有者与所属组为lisi
[root@localhost ~]# chown lisi:lisi /test5
[root@localhost ~]# ll -d /test5
drwxr-xr-x. 2 lisi lisi 6 4月  11 15:30 /test5

[root@localhost ~]# touch /test5/root.txt
[root@localhost ~]# ll /test5/root.txt 
-rw-r--r--. 1 root root 0 4月  11 15:31 /test5/root.txt

#递归修目录下所有子文件与子目录归属关系
[root@localhost ~]# chown -R lisi:lisi /test5
[root@localhost ~]# ll /test5/root.txt 
-rw-r--r--. 1 lisi lisi 0 4月  11 15:31 /test5/root.txt
```

#### SetUID特殊权限

- SetUID（SUID）：对于一个可执行的文件用了SUID权限后，普通用户在执行该文件后，临时拥有文件所有者的身份，该权限只在程序执行过程中有效，程序执行完毕后用户恢复原有身份
- SetUID权限会附加在所有者的 x 权限位上，所有者的 x 权限标识会变成 s
- 设置SetUID命令格式：chmod u+s 文件名

```shell
#搜索命令绝对路径
[root@localhost ~]# which passwd
/usr/bin/passwd
[root@localhost ~]# ll /usr/bin/passwd 
-rwsr-xr-x. 1 root root 27832 6月  10 2014 /usr/bin/passwd

[root@localhost ~]# which cat
/usr/bin/cat
[root@localhost ~]# ll /usr/bin/cat
-rwxr-xr-x. 1 root root 54160 10月 31 2018 /usr/bin/cat

#普通用户使用cat命令是默认无法查看/etc/shadow文件内容
[lisi@localhost ~]$ cat /etc/shadow
cat: /etc/shadow: 权限不够

#设置SUID权限
[root@localhost ~]# chmod u+s /usr/bin/cat
[root@localhost ~]# ll /usr/bin/cat
-rwsr-xr-x. 1 root root 54160 10月 31 2018 /usr/bin/cat

#普通用户再次使用cat命令时临时获取文件所有者身份
[lisi@localhost ~]$ cat /etc/shadow

#去除SUID权限
[root@localhost ~]# chmod u-s /usr/bin/cat
[root@localhost ~]# ll /usr/bin/cat
-rwxr-xr-x. 1 root root 54160 10月 31 2018 /usr/bin/cat

[root@localhost ~]# which vim
/usr/bin/vim

[root@localhost ~]# ll /usr/bin/vim
-rwxr-xr-x. 1 root root 2294208 10月 31 2018 /usr/bin/vim

#为vim设置SUID权限
[root@localhost ~]# chmod u+s /usr/bin/vim
[root@localhost ~]# ll /usr/bin/vim
-rwsr-xr-x. 1 root root 2294208 10月 31 2018 /usr/bin/vim

[root@localhost ~]# ll /etc/passwd
-rw-r--r--. 1 root root 2737 4月  10 17:26 /etc/passwd

[root@localhost ~]# chmod u-s /usr/bin/vim
[root@localhost ~]# vim /etc/passwd
```

#### SetGID特殊权限

- SetGID（SGID）：当对一个可执行的二进制文件设置了SGID后，普通用户在执行该文件时临时拥有其所属组的权限，该权限只在程序执行过程中有效，程序执行完毕后用户恢复原有组身份
- 当对一个目录作设置了SGID权限后，普通用户在该目录下创建的文件的所属组，均与该目录的所属组相同
- SetGID权限会附加在所属组的 x 权限位上，所属组的 x 权限标识会变成 s
- 设置SetGID命令格式：chmod g+s 文件名

```shell
[root@localhost ~]# mkdir /test6
[root@localhost ~]# chmod 777 /test6
[root@localhost ~]# ll -d /test6
drwxrwxrwx. 2 root root 6 4月  11 15:59 /test6

#为目录设置SGID权限
[root@localhost ~]# chmod g+s /test6
[root@localhost ~]# ll -d /test6
drwxrwsrwx. 2 root root 6 4月  11 15:59 /test6
#SGID权限会附加在所属组执行权限位，所属组执行权限变为s

[root@localhost ~]# touch /test6/1.txt
[root@localhost ~]# ll /test6/1.txt 
-rw-r--r--. 1 root root 0 4月  11 16:00 /test6/1.txt

#修改目录所属组为lisi组
[root@localhost ~]# chown :lisi /test6
[root@localhost ~]# ll -d /test6
drwxrwsrwx. 2 root lisi 19 4月  11 16:00 /test6

#SGID对目录设置后，在该目录下创建的任何文件都会继承父目录的所属组
[root@localhost ~]# touch /test6/2.txt
[root@localhost ~]# ll /test6/2.txt 
-rw-r--r--. 1 root lisi 0 4月  11 16:01 /test6/2.txt
```

#### Sticky BIT特殊权限

- Sticky BIT（SBIT）：该权限只针对于目录有效，当普通用户对一个目录拥有w和x权限时，普通用户可以在此目录下拥有增删改的权限，应为普通用户对目录拥有w权限时，是可以删除此目录下的所有文件
- 如果对一个目录设置了SBIT权限，除了root可以删除所有文件以外，普通用户就算对该目录拥有w权限，也只能删除自己建立的文件，不能删除其他用户建立的文件
- SBIT权限会附加在其他人的 x 权限位上，其他人的 x 权限标识会变成 t
- 设置SBIT命令格式：chmod o+t 目录名

```shell
#为目录设置SBIT
[root@localhost ~]# chmod o+t /test
[root@localhost ~]# ll -d /test
drwxrwxrwt. 2 root root 6 4月  11 16:07 /test

[lisi@localhost test]$ ls
kenji.txt  laowang.txt  lisi.txt

[lisi@localhost test]$ rm -rf *
rm: 无法删除"kenji.txt": 不允许的操作
rm: 无法删除"laowang.txt": 不允许的操作
```

#### FACL访问控制列表

- FACL（Filesystemctl Access Control List）文件系统访问控制列表：利用文件扩展属性保存额外的访问控制权限，单独为每一个用户量身定制一个权限
- 命令格式：setfacl 选项 归属关系:权限 文档
- 常用选项：
  - -m 设置权限
  - -x 删除指定用户权限
  - -b 删除所有用户权限

```shell
#为natasha用户设置ACL权限
[root@localhost ~]# setfacl -m u:natasha:rx /yunwei/
[root@localhost ~]# ll -d /yunwei/
drwxrwx---+ 2 root yunwei 54 4月  11 16:43 /yunwei/
[root@localhost ~]# ll -d /test
drwxrwxrwt. 2 root root 42 4月  11 16:11 /test

#查看目录ACL权限
[root@localhost ~]# getfacl /yunwei
getfacl: Removing leading '/' from absolute path names
# file: yunwei
# owner: root
# group: yunwei
user::rwx
user:natasha:r-x
group::rwx
mask::rwx
other::---

#用户测试权限
[natasha@localhost ~]$ ls /yunwei/
hell.sh  kenji.txt  lisi.txt
[natasha@localhost yunwei]$ rm -rf kenji.txt 
rm: 无法删除"kenji.txt": 权限不够
[natasha@localhost yunwei]$ touch natasha.txt
touch: 无法创建"natasha.txt": 权限不够
[natasha@localhost yunwei]$ vim kenji.txt 


[root@localhost ~]# setfacl -m u:tom:rx /yunwei
[root@localhost ~]# setfacl -m u:jack:rx /yunwei
[root@localhost ~]# setfacl -m u:hary:rx /yunwei
[root@localhost ~]# getfacl /yunwei
getfacl: Removing leading '/' from absolute path names
# file: yunwei
# owner: root
# group: yunwei
user::rwx
user:hary:r-x
user:tom:r-x
user:natasha:r-x
user:jack:r-x
group::rwx
mask::rwx
other::---

#删除指定用户ACL权限
[root@localhost ~]# setfacl -x u:tom /yunwei
[root@localhost ~]# getfacl /yunwei
getfacl: Removing leading '/' from absolute path names
# file: yunwei
# owner: root
# group: yunwei
user::rwx
user:hary:r-x
user:natasha:r-x
user:jack:r-x
group::rwx
mask::rwx
other::---

#删除所有用户ACL权限
[root@localhost ~]# setfacl -b /yunwei
[root@localhost ~]# getfacl /yunwei
getfacl: Removing leading '/' from absolute path names
# file: yunwei
# owner: root
# group: yunwei
user::rwx
group::rwx
other::---
```

#### 课后作业

1.创建test1用户，并指定用户UID为6666，指定用户描述信息为test1@163.com，指定用户解释器为/sbin/nologin

test1:x:6666:6666:test1@163.com:/home/test1:/sbin/nologin

2.创建名为stugrp组，将test1用户加入到stugrp组

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# groupadd stugrp

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# gpasswd -a test1 stugrp

3.请写出/etc/passwd文件中每个字段含义

用户名 密码占位符 UID GID 描述信息 家目录 解释器

4.创建test2用户，并设置密码为123456

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# useradd test2
[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# passwd test2

5.修改root用户密码为123456

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# passwd

6.请写出Linux系统下存放用户密码信息文件

/etc/shadow

7.设置test2用户首次登录系统需要修改密码

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# chage -d 0 test2

8.使用root切换为test1用户身份

su - 用户名

9.将test2用户添加至stugrp组，并锁定用户密码

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# gpasswd -a test2 stugrp

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# passwd -l test2

10.删除test1用户，连同用户家目录一并删除

root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# userdel -r test1

11.请写出Linux系统存放组信息文件，与组密码信息文件

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# ls /etc/group

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# ls /etc/gshadow

12.将test2用户从stugrp组中删除

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# gpasswd -d test2 stugrp

13.在根下创建upload目录，并修改目录所有者为test2用户，所属组为stugrp组，并将lisi用户加入到stugrp组，修改所有者权限rwx，修改所属组权限为rwx，设置其他人没有任何权限

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# mkdir /upload

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# chown test2:stugrp /upload/

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# gpasswd -a lisi stugrp

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# chmod 770 /upload/

14.创建test3用户，非交互式设置用户密码为123456，并设置test3用户可以对upload目录拥有rx权限

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# useradd test3
[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# echo 123456 | passwd --stdin test3

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# setfacl -m u:test3:rx /upload/
[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# getfacl /upload/

15.在根下创建shared目录，并同时设置所有人都有完全权限（至少两种方法设置），要求所有普通用户在该目录下只能修改自己创建的文件

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# mkdir /shared
[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# chmod ugo=rwx /shared/
[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# chmod 777 /shared/
[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# ll -d /shared/

[root[@](https://hu60.cn/q.php/bbs.topic.100995.html#)[localhost](https://hu60.cn/q.php/user.info.0.html) ~]# chmod o+t /shared/

#### 常用特殊符号的使用

Linux系统下通配符起到了很大的作用，对于不确定的文档名称可以使用以下特殊字符表示

*常用的特殊符号，在文件名上，用来代表任意多个任意字符

? 常用的特殊符号，在文件名上，用来代表任意单个任意字符

[0-9] #在文件名上，用来代表多个字符或连续范围中的一个，若无则忽略

{a,b,cd,abcd} #在文件名上，用来代表多组不同的字符串，全匹配

- 范例

```shell
#查找以tab结尾的文件
[root@localhost ~]# ls /etc/*tab
[root@localhost ~]# ls /etc/*wd
[root@localhost ~]# ls /etc/*.conf
[root@localhost ~]# ls /etc/redhat*
[root@localhost ~]# ls /etc/*ss*

#查找以tty开头的文件，结尾以一个任意字符结尾
[root@localhost ~]# ls /dev/tty?
[root@localhost ~]# ls /etc/host?
[root@localhost ~]# ls /etc/pass??

#查找tty开头结尾以1-5连续字符结尾
[root@localhost ~]# ls /dev/tty[1-5]
[root@localhost ~]# ls /dev/tty[4-9]
[root@localhost ~]# ls /dev/tty[1,3,5,7,9,15,20,30]

#查找tty开头结尾为不连续字符结尾
[root@localhost ~]# ls /dev/tty{1,3,5,7,9,15,20,30}
[root@localhost ~]# ls /dev/tty{1..9}
[root@localhost ~]# ls /dev/tty{1..10}
[root@localhost ~]# ls /dev/tty[1-10]
```

