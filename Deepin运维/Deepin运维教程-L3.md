---
title: Deepin运维教程-L3
description: 
published: true
date: 2022-06-28T06:00:15.028Z
tags: 运维
editor: markdown
dateCreated: 2022-06-28T05:52:36.883Z
---

# Deepin运维教程-L3
#### vi/vim文本编辑器

- Vim是从 vi 发展出来的一个文本编辑器，vim 具有程序编辑的能力，可以主动的以字体颜色辨别语法的正确性
- vi/vim 共分为三种模式：命令模式、输入模式、底线命令模式（末行模式）
  - 命令模式：刚刚启动 vi/vim，便进入了命令模式
  - 输入模式：在命令模式下按 a/i/o 就进入了输入模式
  - ESC，退出输入模式，切换到命令模式
  - 底线命令模式：在命令模式下按下:（英文冒号）就进入了底线命令模式
- 命令格式：vim 文件名
  - 若目标文件不存在，则新创建文件并编辑
  - 若目标文件以存在，则打开文件并编辑
- 命令模式：刚刚启动 vi/vim，便进入了命令模式
  - i 切换到输入模式，在当前光标所在字符前插入
  - a 切换到输入模式，在当前光标所在字符后插入
  - o 切换到输入模式，在当前光标所在行下插入新行
  - : 切换到底线命令模式，以在最底一行输入命令
  - x 在命令模式下删除当前光标所在的单字符
  - dd 删除一整行内容，配合数字可删除指定范围内的行
  - C 删除当前光标及光标后所有内容并进入输入模式
  - u 恢复上一次修改内容，一次恢复一个操作，可多次恢复，直到恢复本次操作初始状态为止
  - $ 将光标移动至行尾
  - 0（零） 将光标移动至行首
  - gg 跳转至文件第一行
  - G 跳转至文件最后一行
  - yy 复制当前行，配合数字可以同时复制多行
  - p 粘贴当前光标所在行下
  - /关键字 搜索文件内关键字，n从上向下快速定位关键字，N从下向上快速定位关键字
- 底线命令模式可以输入单个或多个字符的命令，可用的命令非常多。
  - :w 保存
  - :q 退出
  - :wq 保存并退出
  - :q! 强制退出不保存
  - :wq! 强制保存并退出
  - :set nu 以行号形式显示文件内容
  - :set nonu 取消行号显示
  - :行号 快速跳转到指定行
  - :r 读入另一个文件的数据 , 文件内容填加到光标的下一行
  - :nohl 取消高亮显示

```shell
[root@test ~]# vim /etc/services 
```

#### 修改网卡IP地址

- 网卡配置文件地址： /etc/sysconfig/network-scripts/网卡名
- ifconfig #用于显示和设置网卡的参数
- systemctl restart network #重启网络
- ifup 网卡名 #启动该网卡设备
- ifdown 网卡名 #禁用该网卡设备

```shell
#修改IP地址
[root@test ~]# vim /etc/sysconfig/network-scripts/ifcfg-ens32 
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="none"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens32"
UUID="16085f4c-f690-4058-b29e-d55c73387026"
DEVICE="ens32"
ONBOOT="yes"
IPADDR="192.168.0.60"   #修改IP地址
PREFIX="24"
GATEWAY="192.168.0.254"
DNS1="114.114.114.114"
IPV6_PRIVACY="no"
~                    
#重启网络（IP地址发生改变，当前终端会断开）
[root@test ~]# systemctl restart network
[c:\~]$ ssh 192.168.0.60

#启动该网卡
[root@test ~]# ifup ens32

#查看所有网卡信息
[root@test ~]# ip a 
```

- 使用命令修改网卡IP地址

nmcli connection modify 网卡名 ipv4.method manual ipv4.addresses Ip地址/掩码 connection.autoconnect yes

解释：

nmcli connection modify（修改）

网卡名 ipv4.method（配置ipv4地址方法）

manual （手动配置）

ipv4.addresses（ipv4地址）

Ip地址/掩码 connection.autoconnect yes（开机自动连接）

- 激活网卡：nmcli connection up 网卡名
- 关闭网卡：nmcli connection down 网卡名
- 重启网卡：nmcli connection reload 网卡名

```shell
#使用命令修改网卡IPV地址
[root@test ~]# nmcli connection modify ens32 ipv4.method manual ipv4.addresses 192.168.0.50/24 connection.autoconnect yes

#激活网卡
[root@test ~]# nmcli connection up ens32
[c:\~]$ ssh 192.168.0.50
```

#### host命令

- host用于将一个域名解析到一个IP地址

```shell
[root@test ~]# host www.baidu.com
www.baidu.com has address 110.242.68.3
www.baidu.com has address 110.242.68.4
www.baidu.com is an alias for www.a.shifen.com.
www.baidu.com is an alias for www.a.shifen.com.
```

#### nslookup命令

- nslookup用于查询域名解析是否正常，在网络故障时用来诊断网络问题

```shell
[root@test ~]# nslookup www.baidu.com
Server:		114.114.114.114
Address:	114.114.114.114#53

Non-authoritative answer:
Name:	www.baidu.com
Address: 110.242.68.4
Name:	www.baidu.com
Address: 110.242.68.3
```

#### alias别名管理

- alias命令用于设置命令别名，用户可以使用alias自定义命令别名来简化命令的复杂度
- .bashrc 文件存放命令别名
- 命令格式：aliasi [别名]=[命令] #注意事项：等号（=）前后不能有空格
- unalias 别名 #取消别名

```shell
#定义别名
[root@test ~]# alias lsnet='ls /etc/sysconfig/network-scripts/'
[root@test ~]# lsnet
[root@test ~]# alias myls='ls -ldh'
[root@test ~]# myls /opt

#查看当前系统可用命令别名
[root@test ~]# alias
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias lsnet='ls /etc/sysconfig/network-scripts/'
alias mv='mv -i'
alias myls='ls -ldh'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'

#两条命令效果相同
[root@test ~]# ls -l hello
-rw-r--r--. 1 root root 426 3月  28 15:00 hello
[root@test ~]# ll hello
-rw-r--r--. 1 root root 426 3月  28 15:00 hello

[root@test ~]# which ls
alias ls='ls --color=auto'
	/usr/sbin/ls
[root@test ~]# /usr/sbin/ls
[root@test ~]# ls

#取消本次命令的别名功能“\”
[root@test ~]# \ls

#取消命令别名
[root@test ~]# unalias myls
[root@test ~]# myls
bash: myls: 未找到命令...

#定义别名不要跟系统命令发生冲突
[root@test ~]# alias ls=hostname
[root@test ~]# ls
test

#取消命令别名
[root@test ~]# unalias ls
[root@test ~]# alias

#重新定义别名
[root@test ~]# alias ls='ls --color=auto'
[root@test ~]# ls
```

#### history 管理历史

- history命令用于显示历史记录和执行过的命令，登录shell时会读取~./bash_history历史文件中记录下的命令，当退出或登出shell时，会自动保存到历史命令文件，该命令单独使用时，仅显示历史命令
- 历史命令默认只能存储1000条，可以通过/etc/profile文件修改
- 命令格式：history [-选项] [参数]
- 常用选项：
  - -a 追加本次新执行的命令至历史命令文件中
  - -d 删除历史命令中指定的命令
  - -c 清空历史命令列表
- 快捷操作：
  - !# 调用命令历史中第N条命令
  - !string 调用命令历史中以strind开头的命令
  - !! 重复执行上一条命令

```shell
#获取命令帮助
[root@test ~]# help history 

#查看历史命令
[root@test ~]# history

#查看记录历史命令文件
[root@test ~]# cat .bash_history 

#将历史命令同步至历史命令配置文件中
[root@test ~]# history -a
[root@test ~]# cat .bash_history 

#删除历史命令中655条命令历史
[root@test ~]# history -d 655
[root@test ~]# history -d 637

#清空缓存中所有历史命令
[root@test ~]# history -c
[root@test ~]# history
    1  history

#删除历史命令配置文件（该文件删除后系统会再次自动创建）
[root@test ~]# rm -rf .bash_history 

#快速调用历史命令中第1条
[root@test ~]# !1
[root@test ~]# !3

#调用历史命令中以cat开头的命令（只调用最近使用的cat历史命令）
[root@test ~]# !cat

#重复执行上一条命令
[root@test ~]# !!

#历史命令默认只能记录1000条，可以通过/etc/profile文件修改
[root@test ~]# vim /etc/profile
...
46 HISTSIZE=100
```

#### date日期时间管理

- date命令用于显示或设置系统日期与时间
- 命令格式：date [-选项] [+格式符] #查看系统日期时间
- date [-选项] [MMDDhhmm[[CC]YY][.ss]] #设置日期时间
- 常用选项：-s 设置日期时间
- 格式符：
  - +%Y 年份
  - +%B 月份
  - +%d 日
  - +%H 时
  - +%M 分
  - +%S 秒
  - +%F 年-月-日
  - +%X 时：分：秒

```shell
#显示系统日期与时间
[root@test ~]# date
2021年 03月 28日 星期日 17:08:34 CST

#只显示年分
[root@test ~]# date +%Y
2021

#只显示月份
[root@test ~]# date +%B
三月

#只显示几号
[root@test ~]# date +%d
28

#只显示小时
[root@test ~]# date +%H
17

#只显示分钟
[root@test ~]# date +%M
10

#只显示秒
[root@test ~]# date +%S
24

#显示年月日
[root@test ~]# date +%F
2021-03-28

#显示时分秒
[root@test ~]# date +%X
17时12分10秒

#显示年月日时分秒
[root@test ~]# date +%F%X
2021-03-2817时12分39秒

#可以自定义分隔符“-”
[root@test ~]# date +%F-%X
2021-03-28-17时13分38秒

[root@test ~]# date +%F:%X
2021-03-28:17时13分55秒

#修改系统年月日
[root@test ~]# date -s 2020-03-28
2020年 03月 28日 星期六 00:00:00 CST

#修改系统时分秒
[root@test ~]# date -s 17:16:00
2020年 03月 28日 星期六 17:16:00 CST

#修改年月日时分秒
[root@test ~]# date -s '2021-03-28 17:17:00'
2021年 03月 28日 星期日 17:17:00 CST
#解释：
''单引号：引用整体，屏蔽特殊符号的功能
""双引号：引用整体，不会屏蔽特殊符号的功能

#Linux的两种时钟
系统时钟：内核通过CPU的工作频率去计算的时间
硬件时钟：

#显示硬件时间
[root@test ~]# clock
2021年03月28日 星期日 17时23分42秒  -0.945549 秒

#显示并同步系统与硬件时钟
[root@test ~]# man hwclock
-s：把系统时间设置成与硬件时间相同
-w：把硬件时间设置成与系统时间相同
[root@test ~]# hwclock -w
[root@test ~]# date
2021年 03月 28日 星期日 17:27:18 CST

#cal显示日历
[root@test ~]# cal
      三月 2021     
日 一 二 三 四 五 六
    1  2  3  4  5  6
 7  8  9 10 11 12 13
14 15 16 17 18 19 20
21 22 23 24 25 26 27
28 29 30 31

#显示指定的全年月份
[root@test ~]# cal 2021
```

#### wc统计命令

- wc 用于统计文件的字节数、行数，并将统计的结果输出到屏幕
- 命令格式：wc [-选项] 文件名
- 常用选项：
  - -c #统计字节数
  - -l #统计行数

```shell
[root@test ~]# wc /etc/passwd
43   87 2259 /etc/passwd
行数 单词 字节  文件名

#统计文件字节数
[root@test ~]# wc -c /etc/passwd
2259 /etc/passwd

#统计文件行数
[root@test ~]# wc -l /etc/passwd
43 /etc/passwd

[root@test ~]# wc -l /etc/fstab
11 /etc/fstab
```

#### 管道符

- 管道符“|”：将命令的输出结果交给另外一条命令作为参数继续处理

```shell
[root@test ~]# head -10 /etc/passwd |tail -5

[root@test ~]# head -10 /etc/passwd |tail -5 |wc -l
5

root@test ~]# cat -n /etc/passwd |head -10|tail -5
     6	sync:x:5:0:sync:/sbin:/bin/sync
     7	shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
     8	halt:x:7:0:halt:/sbin:/sbin/halt
     9	mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
    10	operator:x:11:0:operator:/root:/sbin/nologin

[root@test ~]# ifconfig ens32 |head -2
ens32: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.50  netmask 255.255.255.0  broadcast 192.168.0.255
```

#### 重定向操作

- 重定向操作:将前面命令的输出结果，写入到其他的文本文件中
- 重定向的表示符号
  - \> #重定向输出（覆盖）
  - \>> #重定向输出（追加）
  - < #输入重定向（覆盖）
  - << **#**输入重定向（追加）
  - \> 只收集正确的输出结果
  - 2> 只收集错误的输出结果
  - &> 正确错误都收集

```shell
#将命令的输出结果以覆盖的方式重定向到文件中，（>附带创建文件功能）
[root@test ~]# ifconfig ens32 |head -2 > /opt/ens32.bak
ens32: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.50  netmask 255.255.255.0  broadcast 192.168.0.255

[root@test ~]# cat /etc/hostname > /opt/ens32.bak 
[root@test ~]# cat /opt/ens32.bak 
test

[root@test ~]# free -h > /opt/free.bak
[root@test ~]# cat /opt/free.bak 
              total        used        free      shared  buff/cache   available
Mem:           972M        414M        123M         15M        435M        336M
Swap:          2.0G          0B        2.0G

#将命令的输出结果以追加的方式重定向到文件中
[root@test ~]# cat /etc/hostname >> /opt/free.bak 
[root@test ~]# cat /opt/free.bak 

#“>”只收集正确的输出结果，不收集错误的输出结果
[root@test ~]# ls xxooooxx > /opt/xx.txt
ls: 无法访问xxooooxx: 没有那个文件或目录

#“2>”只收集错误的输出结果，不收集正确的输出结果
[root@test ~]# ls xxooooxx 2> /opt/xx.txt
[root@test ~]# cat /opt/xx.txt 
ls: 无法访问xxooooxx: 没有那个文件或目录

#“2>”以覆盖的方式将输出结果重定向到文件中
[root@test ~]# cat /etc/abc 2> /opt/ens32.bak 
[root@test ~]# cat /opt/ens32.bak 
cat: /etc/abc: 没有那个文件或目录

#“2>>”以追加的方式将输出结果重定向到文件中
[root@test ~]# ls /etc/abcd 2>> /opt/ens32.bak 
[root@test ~]# cat /opt/ens32.bak 
cat: /etc/abc: 没有那个文件或目录
ls: 无法访问/etc/abcd: 没有那个文件或目录

#“&>”以覆盖的方式将正确输出与错误输出重定向到文件中
[root@test ~]# lscat &> /opt/abc.txt
[root@test ~]# cat /opt/abc.txt 

[root@test ~]# ls /etc/passwd &> /opt/pass.bak
[root@test ~]# cat /opt/pass.bak 

[root@test ~]# free -h &> /opt/pass.bak 
[root@test ~]# cat /opt/pass.bak 

#“&>”以追加的方式将正确输出与错误输出重定向到文件中
[root@test ~]# ifconfig ens32 | head -2 &>> /opt/pass.bak 
[root@test ~]# cat /opt/pass.bak 

#以覆盖方式将正确输出与错误输出重定向到不同文件中
[root@test ~]# ll -d /root/  bcd >a.txt 2>b.txt
[root@test ~]# cat a.txt 
dr-xr-x---. 24 root root 4096 3月  28 18:07 /root/
[root@test ~]# cat b.txt 
ls: 无法访问bcd: 没有那个文件或目录
```

#### echo命令与sleep命令

- echo命令用于输出指定的字符串和变量
- 命令格式：echo [-选项] [参数]

```shell
[root@test ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin

[root@test ~]# echo xxoo
xxoo

[root@test ~]# echo abc
abc

[root@test ~]# echo 男人好难
男人好难

[root@test ~]# echo 123
123

[root@test ~]# cat /etc/hostname 
test

[root@test ~]# echo localhost > /etc/hostname
[root@test ~]# cat /etc/hostname 
localhost
```

- sleep命令可以用来将目前动作延迟一段时间
- 命令格式：sleep 时间
- 常用选项： s 秒 m 分钟 h 小时 d 日

```shell
[root@test ~]# sleep 3
```

#### 课后作业

1.查看当前系统内核名称及版本信息

uname -sr

2.请写系统存放cpu配置文件

/proc/cpuinfo

3.请写出查看cpu信息命令

cat /proc/cpuinfo

lscpu

4.请写出系统存放内存配置文件

/proc/meminfo

5.请写出查看内存命令（以人类易读方式显示）

free -h

6.请写出系统存放网卡配置文件路径

/etc/sysconfig/network-scripts/

7.请写出查看网卡配置信息命令

ifconfig（如果系统最小化安装，需要安装net-tools）

ip a （ip address）

8.请写出系统存放主机名配置文件

/etc/hostname

9.请写出查看主机名命令

cat /etc/hostname

hostname

10.将主机名修改为student（永久修改）

hostnamectl set-hostname student

vim /etc/hostname

echo student > /etc/hostname

11.请写出vim的三种模式

命令模式

输入模式

底线命令模式（末行模式）

12.将/etc/passwd文件复制到/opt目录，使用vim打开文件并显示行号

cp /etc/passwd /opt

vim /opt/passwd

:set nu

13.使用vim在/opt/passwd文件中搜索包含root关键字的行

/root

14.使用vim在/opt/passwd文件中将光标快速跳转到第10行，并将光标跳转到行尾

:10 $

15.使用vim在/opt/passwd文件中快速跳转到文件最后一行并删除，在将光标跳转到文件第一行，将刚刚删除的行复制到文件第二行

G dd p

16.使用vim将/etc/hostname文件内容读入到/opt/passwd文件最后一行下

:r /etc/hostname

17.使用vim在/opt/passwd文件中复制前5行内容并粘贴到文件最后一行下

5yy p

18.将本次vim的修改恢复至初始状态，并保存退出

u

:wq

19.将本机IP地址修改为192.168.0.100，并重启动网卡

vim /etc/sysconfig/network-scripts/ifcfg-ens32

systemctl restart network

20.如何获取一个域名所对应的IP地址

host www.baidu.com

21.如何检测本机使用的DNS是否可用

nslookup www.jd.com

22.请将hostname命令设置别名为hn（临时设置）

alias hn=hostname

23.取消hostname命令别名

unalias hn

24.如何查看本机历史命令

history

25.执行命令历史中第20条命令

!20

26.删除命令历史中第5条命令

history -d 5

27.清空所有历史命令

history -c

rm -rf .bash_history

28.查看本机当前系统日期与时间

date

29.将本机日期时间设置与你当前时间一致

date -s '2021-04-10 14:32:00'

30.统计/etc/passwd文件行数，并将命令输出结果重定向至/opt/pass.bak文件中

wc -l /etc/passwd > /opt/pass.bak

31.显示/etc/passwd文件末尾10行的前5行内容，并将输出结果追加至/opt/pass.bak文件中

tail -10 /etc/passwd | head -5 > /opt/pass.bak

cat -n /etc/passwd | tail -10 | head -5 >> /opt/pass.bak
