---
title: Linux日志分析及日志管理
description: 
published: true
date: 2022-06-28T07:55:11.673Z
tags: 
editor: markdown
dateCreated: 2022-06-28T07:55:09.337Z
---

# Linux日志分析及日志管理
## 1.日志介绍
在 Linux 系统中，日志记录了系统中包括内核、服务和其它应用程序等在内的运行信息。在我们解决问题的时候，日志是非常有用的，它可以帮助我们快速的定位遇到的问题。

Linux系统内核和许多程序会产生各种错误信息、警告信息和其他的提示信息，所以把它们写到日志文件中去。完成这个过程的服务，即日志守护进程为syslog。

syslog可以根据日志的类别和优先级将日志保存到不同的文件中，位于/etc/syslog或/etc/syslogd或/etc/rsyslog.d，默认配置文件为/etc/syslog.conf或rsyslog.conf，任何希望生成日志的程序都可以向syslog发送信息。

## 2.日志分类
在下面文章中将以UOS系统下的日志为例，我们将日志分为应用日志和系统日志来分析。

## 3.应用日志
目录：/var/log/和~/.cache/package/，package为软件包名

UOS应用日志大部分默认目录~/.cache/deepin/，如影院：~/.cache/deepin/deepin_movie/deepin-movie.log

部分应用日志默认目录/var/log/，如uos应用商店：/var/log/lastore/daemon.log，向日葵：/var/log/sunlogin/sunlogin_service.log

查看日志方法：用cat/more/less/tail/head等命令，如：
```
less ~/.cache/deepin/deepin-movie/deepin-movie.log |grep Error	查看错误日志
tail -f ~/.cache/deepin/deepin-movie/deepin-movie.log		实时监控日志
```
## 4.系统日志
Linux系统日志将分为以下四个部分：系统日志类型、系统日志级别（优先级）、系统日志文件、系统日志管理。

### 4.1系统日志类型
linux常见的日志类型如下表，但并不是所有的Linux发行版都包含这些类型：
| 类型          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| auth          | 用户认证时产生的日志，如login命令、su命令                    |
| authpriv      | 与 auth 类似，但是只能被特定用户查看                         |
| console       | 针对系统控制台的消息                                         |
| cron          | 系统定期执行计划任务时产生的日志                             |
| daemon        | 某些守护进程产生的日志                                       |
| ftp           | FTP服务                                                      |
| kern          | 系统内核消息                                                 |
| local0.local7 | 由自定义程序使用                                             |
| lpr           | 与打印机活动有关                                             |
| mail          | 邮件日志                                                     |
| mark          | 产生时间戳。系统每隔一段时间向日志文件中输出当前时间，每行的格式类似于 Jun 28 09:00:00 rs2 -- MARK --，可以由此推断系统发生故障的大概时间 |
| news          | 网络新闻传输协议(nntp)产生的消息                             |
| ntp           | 网络时间协议(ntp)产生的消息                                  |
| user          | 用户进程                                                     |
### 4.2 系统日志级别
Linux常见的日志级别如下表：数值越小，优先级越高

| 数值 | 类型    | 说明                                                     |
| ---- | ------- | -------------------------------------------------------- |
| 0    | emerg   | 紧急情况，系统不可用（例如系统崩溃），一般会通知所有用户 |
| 1    | alert   | 需要立即修复，例如系统数据库损坏                         |
| 2    | crit    | 危险情况，例如硬盘错误，可能会阻碍程序的部分功能         |
| 3    | err     | 一般错误消息                                             |
| 4    | warning | 警告                                                     |
| 5    | notice  | 不是错误，但是可能需要处理                               |
| 6    | info    | 通用性消息，一般用来提供有用信息                         |
| 7    | debug   | 调试程序产生的信息                                       |
|      |         |                                                          |
如：内核日志控制台打印信息，可以通过修改/proc/sys/kernel/printk文件内容来控制，查看内核日志级别：
```
cat /proc/sys/kernel/printk
4 4 1 7
```
控制台日志级别：优先级高于该值的消息将被打印至控制台

默认的消息日志级别：将用该优先级来打印没有优先级的消息

最低的控制台日志级别：控制台日志级别可被设置的最小值(最高优先级)

默认的控制台日志级别：控制台日志级别的缺省值

### 4.3 系统日志文件
#### 4.3.1 rsyslog.conf配置文件规则
前面我们讲到，linux日志文件由syslog或rsyslog守护进程生成，它有一些规则，下面我们来看一下：

系统日志守护进程syslog的默认配置文件/etc/rsyslog.conf，根据日志类型和优先级来制定配置规则。

配置文件rsyslog.conf内容如下图，展示的是规则部分内容：
```
$ cat /etc/rsyslog.conf
```
![ll.png](/ll.png)

由图可见保存的日志文件目录/var/log以及日志文件*.log是根据日志类型和优先级来定的。下面将解析配置文件格式内容。

配置文件的基本格式：
```
服务名称[连接符号]日志等级 日志记录位置
如：user.err /var/log/user.err
```
配置文件格式中的[连接符号]的三种含义：
```
“.”表示只记录比当前等级（包含该等级）高的日志，如：user.info /var/log/user.info只记录日志等级大于等于info级别的日志。
“.=”表示只记录所需等级的日志，其他等级的日志都不记录，如：*.=err /var/log/err
“.！”表示不等于，也就是除该等级的日志外，其他等级的日志都记录。
```
注意：修改rsyslog.conf配置文件后，需要重启rsyslog服务才生效。

启动/重启/停止rsyslog服务（start/stop/restart）：
```
#sudo service rsyslog restart或
#/etc/init.d/rsyslog restart或
#systemctl restart rsyslog
```
查看rsyslog服务状态：
```
#chkconfig --list rsyslog
#systemctl status rsyslog
```
#### 4.3.2 日志文件轮替规则
日志是重要的系统文件，记录和保存了系统中所有的重要事件。但是日志文件也需要进行定期的维护，因为日志文件是不断增长的，如果完全不进行日志维护，而任由其随意递增，那么用不了多久，我们的硬盘就会被写满。

##### 4.3.2.1 默认日志文件轮替
Linux 系统是可以自动完成日志的轮替工作，logrotate 就是用来进行日志轮替（也叫日志转储）的。logrotate有一定的规则，其配置文件/etc/logrotate.conf内容如下图：
```
$ cat /etc/logrotate.conf
```
![ll1.png](/ll1.png)


日志轮替最主要的作用就是把旧的日志文件移动并改名，同时建立新的空日志文件，当旧日志文件超出保存的范围时就删除。那么，旧的日志文件改名之后，如何命名呢？主要依靠 /etc/logrotate.conf 配置文件中的“dateext”参数。

从上图可以看到参数dateext默认是被注释的，那么日志文件就需要进行改名了（自动）。如user日志文件，当第一次进行日志转存时，当前的user日志会自动改名为user.1，然后会新建user日志，用来保存新的日志；

当第二次进行日志转存时，user.1会自动改名为user.2，以此类推，到了一定规则会压缩文件为user.x.gz(x: 1、2、3、4...)。

如果配置文件中的“dateext”参数开放，那么日志将会用日期来作为日志文件的后缀，如“user-20200628”。这样日志文件名不会重叠，也就不需要对日志文件进行改名，只需要保存指定的日志个数，删除多余的日志文件即可。

##### 4.3.2.2 自定义日志文件论轮替
自定义日志文件加入轮替的两种方法：

方法一：直接修改/etc/logrotate.conf 配置文件中写入该日志的轮替策略，从而把日志加入轮替

方法二：在 /etc/logrotate.d/目录中新建立该日志的轮替文件，在该轮替文件中写入正确的轮替规则，因为该目录中的文件都会被包含到主配置文件中，所以也可以把日志加入轮替。

推荐使用这种方式，因为系统中需要轮替的日志非常多，如果全部直接写入/etc/logrotate.conf配置文件，那么这个文件的可管理性就会非常差，不利于此文件的维护。

日志文件的轮替之所以可以在指定的时间备份日志,是因为其依赖系统定时任务，在 /etc/cron.daily/目录下存在logrotate文件，系统会执行该文件，

然后logrotate会依据 /etc/logrotate.conf 配置文件的配置来判断配置文件中的日志是否符合日志轮替的条件，还可以用logrotate命令直接实现轮替：logrotate [选项] 配置文件名

例子：logrotate -f /etc/logrotate.d/logfile

##### 4.3.2.3 配置文件logrotate.conf的主要参数说明
| 参数                    | 说明                                                         |
| ----------------------- | ------------------------------------------------------------ |
| daily                   | 日志的轮替周期是毎天                                         |
| weekly                  | 日志的轮替周期是每周                                         |
| monthly                 | 日志的轮控周期是每月                                         |
| rotate数宇              | 保留的日志文件的个数。0指没有备份                            |
| compress                | 当进行日志轮替时，对旧的日志进行压缩                         |
| create mode owner group | 建立新日志，同时指定新日志的权限与所有者和所属组.如create 0600 root utmp |
| mail address            | 当进行日志轮替时.输出内存通过邮件发送到指定的邮件地址        |
| missingok               | 如果日志不存在，则忽略该日志的警告信息                       |
| nolifempty              | 如果曰志为空文件，則不进行日志轮替                           |
| minsize 大小            | 日志轮替的最小值。也就是日志一定要达到这个最小值才会进行轮持，否则就算时间达到也不进行轮替 |
| size大小                | 日志只有大于指定大小才进行日志轮替，而不是按照时间轮替，如size 100k |
| dateext                 | 使用日期作为日志轮替文件的后缀，如secure-20130605            |
| sharedscripts           | 在此关键宇之后的脚本只执行一次                               |
| prerotate/cndscript     | 在曰志轮替之前执行脚本命令。endscript标识prerotate脚本结束   |
| postrolaie/endscripl    | 在日志轮替之后执行脚本命令。endscripi标识postrotate脚本结束  |
#### 4.3.3 常用日志详细分类介绍
| 文件名                            | 说明                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| /var/log/messages                 | 包括整体系统信息，其中也包含系统启动期间的日志。此外，mail，cron，daemon，kern和auth等内容也记录在var/log/messages日志中 |
| /var/log/dmesg                    | 包含内核缓冲信息（kernel ring buffer）。在系统启动时，会在屏幕上显示许多与硬件有关的信息。可以用dmesg查看它们 |
| /var/log/auth.log                 | 包含系统授权信息，包括用户登录和使用的权限机制等             |
| /var/log/boot.log                 | 记录系统在引导过程中发生的事件，就是系统开机启动时自检过程显示的信息 |
| /var/log/daemon.log               | 包含各种系统后台守护进程日志信息                             |
| /var/log/kern.log                 | 包含内核产生的日志，在系统启动时，显示与硬件有关的信息，使用dmesg命令查看 |
| /var/log/lastlog                  | 记录所有用户最后一次成功登陆的时间、登陆IP等信息。这不是一个ASCII文件，因此需要用lastlog命令查看内容 |
| /var/log/maillog/var/log/mail.log | 包含来着系统运行电子邮件服务器的日志信息。例如，sendmail日志信息就全部送到这个文件中 |
| /var/log/user.log                 | 记录所有等级用户信息的日志                                   |
| /var/log/Xorg.x.log               | 来自X的日志信息                                              |
| /var/log/alternatives.log         | 更新替代信息都记录在这个文件中                               |
| /var/log/btmp                     | more”                                                        |
| /var/log/cups                     | 记录打印信息相关日志的目录                                   |
| /var/log/anaconda.log             | 在安装Linux时，所有安装信息都储存在这个文件中                |
| /var/log/yum.log                  | 包含使用yum安装的软件包信息（redheat系列），debain系列为apt  |
| /var/log/apt/                     | 记录apt命令相关日志的目录，目录下存在history.log和term.log，两个日志文件的区别在于 history.log主要记录了进行了哪个操作，相关的依赖有哪些，而term.log则是较为具体的一些操作，主要就是下载、打开、安装包等细节操作 |
| /var/log/dpkg.log                 | 包括安装卸载或dpkg命令清除软件包的日志                       |
| /var/log/deepin-installer.log     | 包含所有安装的软件包版本信息                                 |
| /var/log/lightdm/                 | 记录界面管理相关日志的目录                                   |
| /var/log/cron                     | 每当cron进程开始一个工作时，就会将相关信息记录在这个文件中   |
| /var/log/secure                   | 包含验证和授权方面信息。例如，sshd会将所有信息记录（其中包括失败登录）在这里 |
| /var/log/wtmp /var/log/utmp       | 永久记录每个用户登录、注销及系统的启动、停机的事件。这是二进制文件，不能直接用vi/cat等查看，要使用last命令查看 |
| /var/log/faillog                  | 包含用户登录失败信息。此外，错误登录命令也会记录在本文件中   |
除了上述Log文件以外， /var/log还基于系统的具体应用包含以下一些子目录：
```
	/var/log/httpd/或/var/log/apache2 — 包含服务器access_log和error_log信息。
	/var/log/lighttpd/ — 包含light HTTPD的access_log和error_log。
	/var/log/mail/ – 这个子目录包含邮件服务器的额外日志。
	/var/log/prelink/ — 包含.so文件被prelink修改的信息。
	/var/log/audit/ — 包含被 Linux audit daemon储存的信息。
	/var/log/samba/ – 包含由samba存储的信息。
	/var/log/sa/ — 包含每日由sysstat软件包收集的sar文件。
	/var/log/sssd/ – 用于守护进程安全服务。
```
另外，串口调试log，请参考：https://wikidev.uniontech.com/index.php?title=%E4%B8%B2%E5%8F%A3%E8%B0%83%E8%AF%95%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E
```
$ tail -f ~/.cache/deepin/deepin-movie/deepin-movie.log |grep Warning --color
```
默认只能实时查看10行数据，如果要看更多，比如说50行，可以通过如下命令：
```
$ tail -50f ~/.cache/deepin/deepin-movie/deepin-movie.log |grep Warning --color
$ tailf -100 ~/.cache/deepin/deepin-movie/deepin-movie.log |grep Warning --color
```
4.4.2 用户登录信息分析
users、who、w 命令查看已登录的用户信息,详细度不同

last、lastb 命令查看最近登录成功/失败的用户信息

lastlog 记录所有的用户什么时候登录过系统，查看后门的账号

#### 4.4.3 日志管理工具journalctl
journalctl 工具用于检索 systemd 日志，提取由 systemd-journal 服务搜集的日志，主要包括内核/系统日志、服务日志；systemd 是一个专用于 Linux 操作系统的系统与服务管理器，systemd统一管理所有Unit的启动日志。 可以只用journalctl一个命令，查看所有日志（内核日志和应用日志），日志的配置文件是/etc/systemd/journald.conf。journalctl功能强大，用法非常多，下面将介绍journalctl的常用方法。 不带任何选项与参数， 表示显示全部日志：
```
$ journalctl
```
不分页标准输出：日志默认分页输出--no-pager改为正常的标准输出
```
$ journalctl --no-pager
```
查看系统本次启动的日志：
```
$ journalctl -b
$ journalctl -b -0
```
查看上一次启动的日志: 需更改设置,如上次系统崩溃，需要查看日志时，就要看上一次的启动日志。
```
$ journalctl -b -l
```
查看指定时间的日志：
```
$ journalctl --since="2020-06-28 13:15:00"
$ journalctl --since="2020-06-27 18:00:00" --until="2020-06-28 09:00:00"
$ journalctl --since yesterday/today
```
显示尾部的最新行日志：
```
$ journalctl -n		默认只显示10行
$ journalctl -n 50	指定显示50行
```
实时滚动显示最新日志：
```
$ journalctl -f
```
查看指定服务的日志：
```
$ journalctl /usr/lib/deepin-daemon/dde-session-daemon
```
查看指定进程的日志：
```
$ ps -aux |grep dde-session-daemon
lhh       2111  0.1  0.6 2743316 56036 ?       Sl   6月17  33:19 /usr/lib/deepin-daemon/dde-session-daemon
$ journalctl _PID=2111
```
查看某个路径的脚本的日志：
```
$ sudo journalctl /usr/bin/bash
```
查看指定用户的日志：
```
$ cat /etc/passwd |grep uos
uos:x:1000:1000::/home/uos:/bin/bash

$ journalctl _UID=1000 --since today
```
查看所有日志详细参数 ：
```
$ journalctl -o verbose
```
查看指定优先级（及其以上级别）的日志：
```
$ journalctl -p err -b
```
显示所有 D-Bus 进程产生的日志：
```
$ journalctl /usr/bin/dbus-daemon
```
显示启动所产生的所有内核日志：
```
$ sudo journalctl -k
```
附journalctl命令行选项：
```
--no-full, --full, -l
如果字段内容超长 则以省略号(…)截断以适应列宽。 默认显示完整的字段内容 (超长的部分换行显示或者被分页工具截断)。
老旧的 -l/--full 选项 仅用于撤销已有的 --no-full 选项，除此之外没有其他用处。
-a, --all
完整显示所有字段内容，即使其中包含非打印字符或者字段内容超长。 默认情况下，包含非打印字符的字段将被缩写为"blob data"(二进制数据)。 注意，分页程序可能会再次转义非打印字符
-f, --follow
只显示最新的日志项， 并且不断显示新生成的日志项。 此选项隐含了 -n 选项。
-e, --pager-end
在分页工具内 立即跳转到日志的尾部。 此选项隐含了 -n1000 以确保分页工具不必缓存太多的日志行。 不过这个隐含的行数可以被明确设置的 -n 选项覆盖。 注意，此选项仅可用于 less(1) 分页器。
-n, --lines=
限制显示最新的日志行数。 --pager-end 与 --follow 隐含了此选项。 此选项的参数：若为正整数则表示最大行数； 若为 "all" 则表示不限制行数； 若不设参数则表示默认值10行。
--no-tail
显示所有日志行， 也就是用于撤销已有的 --lines= 选项(即使与 -f 连用)。
-r, --reverse
反转日志行的输出顺序， 也就是最先显示最新的日志。
-o, --output=
控制日志的 输出格式。 可以使用如下选项：
short 
这是默认值， 其输出格式与传统的 syslog 文件的格式相似， 每条日志一行。
short-full 
与 short 类似，只是将时间戳字段 按照 --since= 与 --until= 接受的格式显示。 与 short 的不同之处在于， 输出的时间戳中还包含星期、年份、时区信息，并且与系统的本地化设置无关。
short-iso 
与 short 类似，只是将时间戳字段以 ISO 8601 格式 显示。
short-iso-precise 
与 short-iso 类似，只是将时间戳字段的秒数 精确到了微秒级别(百万分之一秒)。
short-precise 
与 short 类似，只是将时间戳字段的秒数 精确到了微秒级别(百万分之一秒)。
short-monotonic 
与 short 类似，只是将时间戳字段的零值 从内核启动时开始计算。
short-unix 
与 short 类似，只是将时间戳字段显示为从"UNIX时间原点"(1970-1-1 00:00:00 UTC)以来的秒数。 精确到微秒级别。
verbose 
以结构化的格式显示 每条日志的所有字段。
export 
将日志序列化为二进制字节流 (大部分依然是文本)， 以适用于备份与网络传输(详见 Journal Export Format 文档)。亦可使用 systemd-journal-remote.service(8) 工具将二进制字节流转换为本地 journald 格式。
json 
将日志项格式化为 JSON 对象，并用换行符分隔(也就是每条日志一行，详见 Journal JSON Format 文档)。 字段值通常按照 JSON 字符串规范进行编码，但是如下三种情况例外：
大于4096字节的字段将被编码为 null 值(可以使用 --all 选项关闭此特性，但是这样做会导致生成又大又长的 JSON 对象)。
日志中允许存在同名字段，但是在 JSON 对象中不允许。 因此，同名字段的多个值在 JSON 对象中将会被 编码为一个数组。
包含非打印字符或非UTF8字符的字段， 将被编码为包含原始二进制字节的数组(其中的每个字节都视为一个无符号整数)。
注意，这种编码方式是可逆的(存在空间大小的限制)。
json-pretty 
将日志项按照JSON数据结构格式化， 但是每个字段一行， 以便于人类阅读。
json-sse 
将日志项按照JSON结构格式化，每条日志一行，但是用大括号包围， 以符合 Server-Sent Events 的要求。
json-seq 
将日志项按照JSON结构格式化， 同时为每条日志加上一个ASCII记录分隔符(0x1E)前缀以及一个ASCII换行符(0x0A)后缀，以符合 JavaScript Object Notation (JSON) Text Sequences ("application/json-seq") 的要求。
cat 
仅显示日志的实际内容， 而不显示与此日志相关的 任何元数据(包括时间戳)。
with-unit 
与 short-full 类似， 但是在日志项前缀中使用单元名称代替传统的 syslog 标识。 这对于从模板实例化而来的单元比较有意义， 因为会在单元名称中包含实例化参数。
--output-fields=
一个逗号分隔的字段名称列表，表示仅输出列表中的字段。 仅影响默认输出全部字段的输出格式(verbose, export, json, json-pretty, json-sse, json-seq)。注意， "__CURSOR", "__REALTIME_TIMESTAMP", "__MONOTONIC_TIMESTAMP", "_BOOT_ID" 字段永远输出， 不能被排除。
--utc
以世界统一时间(UTC) 表示时间
--no-hostname
不显示来源于本机的日志消息的主机名字段。 此选项仅对 short 系列输出格式(见上文)有效。
-x, --catalog
在日志的输出中 增加一些解释性的短文本， 以帮助进一步说明 日志的含义、 问题的解决方案、支持论坛、 开发文档、以及其他任何内容。 并非所有日志都有 这些额外的帮助文本， 详见 Message Catalog Developer Documentation 文档。
注意，如果要将日志输出用于bug报告， 请不要使用 此选项。
-q, --quiet
安静模式， 也就是当以普通用户身份运行时， 不显示任何警告信息与提示信息。 例如： "-- Logs begin at …", "-- Reboot --"
-m, --merge
混合显示 包括来自于其他远程主机的日志在内的所有可见日志。
-b [ID][±offset], --boot=[ID][±offset]
显示特定于某次启动的日志， 这相当于添加了一个 "_BOOT_ID=" 匹配条件。
如果未指定 ID 与 ±offset 参数， 则表示仅显示本次启动的日志。
如果省略了 ID ， 那么当 ±offset 是正数的时候， 将从日志头开始正向查找， 否则(也就是为负数或零)将从日志尾开始反响查找。 举例来说， "-b 1"表示按时间顺序排列最早的那次启动， "-b 2"则表示在时间上第二早的那次启动； "-b -0"表示最后一次启动， "-b -1"表示在时间上第二近的那次启动， 以此类推。 如果 ±offset 也省略了， 那么相当于"-b -0"， 除非本次启动不是最后一次启动(例如用 --directory 指定了 另外一台主机上的日志目录)。
如果指定了32字符的 ID ， 那么表示以此 ID 所代表的那次启动为基准 计算偏移量(±offset)， 计算方法同上。 换句话说， 省略 ID 表示以本次启动为基准 计算偏移量(±offset)。
--list-boots
列出每次启动的 序号(也就是相对于本次启动的偏移量)、32字符的ID、 第一条日志的时间戳、最后一条日志的时间戳。
-k, --dmesg
仅显示内核日志。隐含了 -b 选项以及 "_TRANSPORT=kernel" 匹配项。
-t, --identifier=SYSLOG_IDENTIFIER
仅显示 syslog 识别符为 SYSLOG_IDENTIFIER 的日志项。
可以多次使用该选项以 指定多个识别符。
-u, --unit=UNIT|PATTERN
仅显示 属于特定单元的日志。 也就是单元名称正好等于 UNIT 或者符合 PATTERN 模式的单元。 这相当于添加了一个 "_SYSTEMD_UNIT=UNIT" 匹配项(对于 UNIT 来说)， 或一组匹配项(对于 PATTERN 来说)。
可以多次使用此选项以添加多个并列的匹配条件(相当于用"OR"逻辑连接)。
--user-unit=
仅显示 属于特定用户会话单元的日志。 相当于同时添加了 "_SYSTEMD_USER_UNIT=" 与 "_UID=" 两个匹配条件。
可以多次使用此选项以添加多个并列的匹配条件(相当于用"OR"逻辑连接)。
-p, --priority=
根据 日志等级(包括等级范围) 过滤输出结果。 日志等级数字与其名称之间的 对应关系如下 (参见 syslog(3))： "emerg" (0), "alert" (1), "crit" (2), "err" (3), "warning" (4), "notice" (5), "info" (6), "debug" (7) 。 若设为一个单独的数字或日志等级名称， 则表示仅显示小于或等于此等级的日志(也就是重要程度等于或高于此等级的日志)。 若使用 FROM..TO.. 设置一个范围， 则表示仅显示指定的等级范围内(含两端)的日志。 此选项相当于添加了 "PRIORITY=" 匹配条件。
-g, --grep=
使用指定的正则表达式对 MESSAGE= 字段进行过滤，仅输出匹配的日志项。 必须使用 PERL 兼容的正则表达式，详细语法参见 pcre2pattern(3) 手册。
如果正则表达式中仅含小写字母，那么将自动进行大小写无关的匹配， 否则将使用大小写敏感的匹配。是否大小写敏感可以使用下面的 --case-sensitive 选项强制指定。
--case-sensitive[=BOOLEAN]
是否对正则表达式进行大小写敏感的匹配。
-c, --cursor=
从指定的游标(cursor)开始显示日志。 [提示]每条日志都有一个"__CURSOR"字段，类似于该条日志的指纹。
--after-cursor=
从指定的游标(cursor) 之后开始显示日志。 如果使用了 --show-cursor 选项， 则也会显示游标本身。
--show-cursor
在最后一条日志之后显示游标， 类似下面这样，以"--"开头：
-- cursor: s=0639…
游标的具体格式是私有的(也就是没有公开的规范)， 并且会变化。
-S, --since=, -U, --until=
显示晚于指定时间(--since=)的日志、显示早于指定时间(--until=)的日志。 参数的格式类似 "2012-10-30 18:17:16" 这样。 如果省略了"时:分:秒"部分，则相当于设为 "00:00:00" 。 如果仅省略了"秒"的部分则相当于设为 ":00" 。 如果省略了"年-月-日"部分，则相当于设为当前日期。 除了"年-月-日 时:分:秒"格式，参数还可以进行如下设置： (1)设为 "yesterday", "today", "tomorrow" 以表示那一天的零点(00:00:00)。 (2)设为 "now" 以表示当前时间。 (3)可以在"年-月-日 时:分:秒"前加上 "-"(前移) 或 "+"(后移) 前缀以表示相对于当前时间的偏移。 关于时间与日期的详细规范，参见 systemd.time(7) 注意， --output=short-full 将会一丝不苟的严格按照上述格式输出时间戳。
-F, --field=
显示所有日志中指定字段的所有可能值。 [译者注]类似于SQL语句："SELECT DISTINCT 指定字段 FROM 全部日志"
-N, --fields
输出所有日志字段的名称
--system, --user
仅显示 系统服务与内核的日志(--system)、 仅显示当前用户的日志(--user)。 如果两个选项都未指定，则显示当前用户的所有可见日志。
-M, --machine=
显示来自于正在运行的、特定名称的本地容器的日志。 参数必须是一个本地容器的名称。
-D DIR, --directory=DIR
仅显示 特定目录中的日志， 而不是 默认的运行时和系统日志目录中的日志。
--file=GLOB
GLOB 是一个 可以包含"?"与"*"的文件路径匹配模式。 表示仅显示来自与指定的 GLOB 模式匹配的文件中的日志， 而不是默认的运行时和系统日志目录中的日志。 可以多次使用此选项 以指定多个匹配模式(多个模式之间用"OR"逻辑连接)。
--root=ROOT
在对日志进行操作时， 将 ROOT 视为系统的根目录。 例如 --update-catalog 将会创建 ROOT/var/lib/systemd/catalog/database 文件， 并且将会显示 ROOT/run/journal 或 ROOT/var/log/journal 目录中的日志。
--header
此选项并不用于显示日志内容， 而是用于显示 日志文件内部的头信息(类似于元数据)。
--disk-usage
此选项并不用于显示日志内容， 而是用于显示 所有日志文件(归档文件与活动文件)的磁盘占用总量。
--vacuum-size=, --vacuum-time=, --vacuum-files=
这些选项并不用于显示日志内容，而是用于清理过期的日志归档文件(并不清理活动的日志文件)，以释放磁盘空间。 --vacuum-size= 可用于限制归档文件的最大磁盘使用量(可以使用 "K", "M", "G", "T" 后缀)； --vacuum-time= 可用于清除指定时间之前的归档(可以使用 "s", "m", "h", "days", "weeks", "months", "years" 后缀)； --vacuum-files= 可用于限制日志归档文件的最大数量。 注意，--vacuum-size= 对 --disk-usage 的输出仅有间接效果， 因为 --disk-usage 输出的是归档日志与活动日志的总量。 同样，--vacuum-files= 也未必一定会减少日志文件的总数， 因为它同样仅作用于归档文件而不会删除活动的日志文件。
此三个选项可以同时使用， 以同时从三个维度去限制归档文件。 若将某选项设为零， 则表示取消此选项的限制。
此三个选项还可以和 --rotate 一起使用， 表示首先滚动所有日志归档， 然后再执行清理操作。 这样可以确保首先完成所有活动日志的归档， 进而使得清理操作变得非常高效。
--list-catalog [128-bit-ID…] 
简要列出日志分类信息， 其中包括对分类信息的简要描述。
如果明确指定了分类ID(128-bit-ID)， 那么仅显示指定的分类。
--dump-catalog [128-bit-ID…] 
详细列出 日志分类信息 (格式与 .catalog 文件相同)。
如果明确指定了分类ID(128-bit-ID)， 那么仅显示指定的分类。
--update-catalog
更新 日志分类索引二进制文件。 每当安装、删除、更新了分类文件， 都需要执行一次此动作。
--setup-keys
此选项并不用于显示日志内容， 而是用于生成一个新的FSS(Forward Secure Sealing)密钥对。 此密钥对包含一个"sealing key" 与一个"verification key"。 "sealing key"保存在本地日志目录中， 而"verification key"则必须保存在其他地方。详见 journald.conf(5) 中的 Seal= 选项。
--force
与 --setup-keys 连用， 表示即使已经配置了FSS(Forward Secure Sealing)密钥对， 也要强制重新生成。
--interval=
与 --setup-keys 连用， 指定"sealing key"的变化间隔。 较短的时间间隔会导致占用更多的CPU资源， 但是能够减少未检测的日志变化时间。 默认值是 15min
--verify
检查日志文件的内在一致性。 如果日志文件在生成时开启了FSS特性， 并且使用 --verify-key= 指定了FSS的"verification key"， 那么，同时还将验证日志文件的真实性。
--verify-key=
与 --verify 选项连用， 指定FSS的"verification key"
--sync
要求日志守护进程 将所有未写入磁盘的日志数据刷写到磁盘上， 并且一直阻塞到刷写操作实际完成之后才返回。 因此该命令可以保证当它返回的时候， 所有在调用此命令的时间点之前的日志， 已经全部安全的刷写到了磁盘中。
--flush
要求日志守护进程 将 /run/log/journal 中的日志数据 刷写到 /var/log/journal 中 (如果持久存储设备当前可用的话)。 此操作会一直阻塞到操作完成之后才会返回， 因此可以确保在该命令返回时， 数据转移确实已经完成。 注意，此命令仅执行一个单独的、一次性的转移动作， 若没有数据需要转移， 则此命令什么也不做， 并且也会返回一个 表示操作已正确完成的返回值。
--rotate
要求日志守护进程滚动日志文件。此命令会一直阻塞到滚动操作完成之后才会返回。 日志滚动可以确保所有活动的日志文件都被关闭、并被重命名以完成归档， 同时，新的空白日志文件将被创建，并成为新的活动日志文件。 此选项可以与 --vacuum-size=, --vacuum-time=, --vacuum-file= 一起使用， 以提高日志清理的效率。
-h, --help
显示简短的帮助信息并退出。
--version
显示简短的版本信息并退出。
--no-pager
不将程序的输出内容管道(pipe)给分页程序。
```