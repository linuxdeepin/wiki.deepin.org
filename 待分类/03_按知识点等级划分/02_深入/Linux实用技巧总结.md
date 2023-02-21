---
title: Linux实用技巧总结
description: 
published: true
date: 2022-10-17T09:11:41.761Z
tags: 
editor: markdown
dateCreated: 2022-06-27T09:04:42.651Z
---

# Linux实用技巧
本篇文章给大家分享一些非常实用小技巧，熟悉 Linux 系统的同学都知道，它高效主要体现在命令行。通过命令行，可以将很多简单的命令，通过自由的组合，得到非常强大的功能。希望对大家有帮助。
## 1. 快速清空文件的方法
```
$ > access.log

true > access.log

cat /dev/null > access.log

echo -n "" > access.log

echo > access.log

truncate -s 0 access.log
```
简单解释下， : 在 shell 中是一个内置命令，表示 no-op，大概就是空语句的意思，所以 : 的那个用法，就是执行命令后，什么都没有输出，将空内容覆盖到文件
## 2. 快速生成大文件
有时候，在 Linux 上，我们需要一个大文件，用于测试上传或下载的速度，通过 dd 命令可以快速生成一个大文件
```
$ dd if=/dev/zero of=file.img bs=1M count=1024
```
上述命令，生成一个文件名为 file.img 大小为 1G 的文件。

## 3. 安全擦除硬盘数据
介绍一种擦除硬盘数据的方法，高效，安全。可以通过 dd 命令，轻松实现：
```
$ dd if=/dev/urandom of=/dev/sda
```
使用 /dev/urandom 生成随机数据，将生成的数据写入 sda 硬盘中，相当于安全的擦除了硬盘数据。

当年陈老师，如果学会了这条命令，可能也不会有艳兆门事件了。
## 4. 快速制作系统盘
在 Linux 下制作系统盘，直接一条命令搞定：
```
$ dd if=ubuntu-server-amd64.iso of=/dev/sdb
```
sdb 可以 U 盘，也可以是普通硬盘
## 5. 查看某个进程的运行时间
可能，大部分同学只会使用 ps aux，其实可以通过 -o 参数，指定只显示具体的某个字段，会得到更清晰的结果。
```
$ ps -p 10167 -o etimes,etime
ELAPSED     ELAPSED
1712055 19-19:34:15
```
通过 etime 获取该进程的运行时间，可以很直观地看到，进程运行了 19 天
同样，可以通过 -o 指定 rss 可以只获取该进程的内存信息
```
$ ps -p 10167 -o rss
RSS
2180
```
## 6.动态实时查看日志
通过 tail 命令 -f 选项，可以动态地监控日志文件的变化，非常实用
```
$ tail -f test.log
```
如果想在日志中出现 Failed 等信息时立刻停止 tail 监控，可以通过如下命令来实现：
```
$ tail -f test.log | sed '/Failed/ q'
```
## 7.时间戳的快速转换
时间操作，对程序员来说就是家常便饭。有时候希望能够将时间戳，转换为日期时间，在 Linux 命令行上，也可以快速的进行转换：
```
$ date -d@1234567890 +"%Y-%m-%d %H:%M:%S"
2009-02-14 07:31:30
```
当然，也可以在命令行上，查看当前的时间戳
```
$ date +%s
1617514141
```
## 8.优雅的计算程序运行时间
在 Linux 下，可以通过 time 命令，很容易获取程序的运行时间：
```
$ time ./test
real    0m1.003s
user    0m0.000s
sys     0m0.000s
```
可以看到，程序的运行时间为: 1.003s。细心的同学，会看到 real 貌似不等于 user + sys，而且还远远大于，这是怎么回事呢？

先来解释下这三个参数的含义：

- real：表示的钟表时间，也就是从程序执行到结束花费的时间；

- user：表示运行期间，cpu 在用户空间所消耗的时间；

- sys：表示运行期间，cpu 在内核空间所消耗的时间；

由于 user 和 sys 只统计 cpu 消耗的时间，程序运行期间会调用 sleep 发生阻塞，也可能会等待网络或磁盘 IO，都会消耗大量时间。因此对于类似情况，real 的值就会大于其它两项之和。

另外，也会遇到 real 远远小于 user + sys 的场景，这是什么鬼情况？

这个更好理解，如果程序在多个 cpu 上并行，那么 user 和 sys 统计时间是多个 cpu 时间，实际消耗时间 real 很可能就比其它两个之和要小了
## 9. 命令行查看ascii码
我们在开发过程中，通常需要查看 ascii 码，通过 Linux 命令行就可以轻松查看，而不用去 Google 或 Baidu
```
man ascii
```
## 10. 优雅的删除乱码的文件
在 Linux 系统中，会经常碰到名称乱码的文件。想要删除它，却无法通过键盘输入名字，有时候复制粘贴乱码名称，终端可能识别不了，该怎么办？

不用担心，下边来展示下 find 是如何优雅的解决问题的。
```
$ ls  -i
138957 a.txt  138959 T.txt  132395.txt
$ find . -inum 132395 -exec rm {} \;
```
命令中，-inum 指定的是文件的 inode 号，它是系统中每个文件对应的唯一编号，find 通过编号找到后，执行删除操作。
### 11. Linux上获取你的公网IP地址
在办公或家庭环境，我们的虚拟机或服务器上配置的通常是内网 IP 地址，我们如何知道，在与外网通信时，我们的公网出口 IP 是神马呢？

这个在 Linux 上非常简单，一条命令搞定
```
$ curl ip.sb
$ curl ifconfig.me
```
上述两条命令都可以
## 12. 如何批量下载网页资源
有时，同事会通过网页的形式分享文件下载链接，在 Linux 系统，通过 wget 命令可以轻松下载，而不用写脚本或爬虫
```
$ wget -r -nd -np --accept=pdf http://fast.dpdk.org/doc/pdf-guides/
# --accept：选项指定资源类型格式 pdf
```
## 13. 历史命令使用技巧
分享几个历史命令的使用技巧，能够提高你的工作效率。

- !!：重复执行上条命令；

- !N：重复执行 history 历史中第 N 条命令，N 可以通过 history 查看；

- !pw：重复执行最近一次，以pw开头的历史命令，这个非常有用，小编使用非常高频；

- !$：表示最近一次命令的最后一个参数；

猜测大部分同学没用过 !$，这里简单举个例子，让你感受一下它的高效用法
```
$ vim /root/sniffer/src/main.c
$ mv !$ !$.bak
# 相当于
$ mv /root/sniffer/src/main.c /root/sniffer/src/main.c.bak
```
当前工作目录是 root，想把 main.c 改为 main.c.bak。正常情况你可能需要敲 2 遍包含 main.c 的长参数，当然你也可能会选择直接复制粘贴。

而我通过使用 !$ 变量，可以很轻松优雅的实现改名，是不是很 hacker 呢？
## 14. 快速搜索历史命令
在 Linux 下经常会敲很多的命令，我们要怎么快速查找并执行历史命令呢？

通过上下键来翻看历史命令，No No No，可以通过执行 Ctrl + r，然后键入要所搜索的命令关键词，进行搜索，回车就可以执行，非常高效。
## 15. 真正的黑客不能忽略技巧
最后，再分享一个真正的黑客不能忽略技巧。我们在所要执行的命令前，加一个空格，那这条命令就不会被 history 保存到历史记录

有时候，执行的命令中包含敏感信息，这个小技巧就显得非常实用了，你也不会再因为忘记执行 history -c 而烦恼了。

## 16、查找当前目录下所有以.tar结尾的文件然后移动到指定目录：
```
 find . -name “*.tar” -exec mv {}./backup/ ;
```
**注解**：find –name 主要用于查找某个文件名字，-exec 、xargs可以用来承接前面的结果，然后将要执行的动作，一般跟find在一起用的很多，find使用我们可以延伸-mtime查找修改时间、-type是指定对象类型（常见包括f代表文件、d代表目录），-size 指定大小，例如经常用到的：查找当前目录30天以前大于100M的LOG文件并删除。
```
find  . -name "*.log" –mtime +30 –typef –size +100M |xargs rm –rf {};
```
## 17、批量解压当前目录下以.zip结尾的所有文件到指定目录：
```
for i  in  `find . –name “*.zip”–type f `
do
unzip –d $i /data/www/img/
done
```
**注解**：forI in （command）;do … done为for循环的一个常用格式，其中I为变量，可以自己指定。
## 18、sed常用命收集：test.txt做测试
如何去掉行首的.字符: 
```
sed-i ‘s/^.//g’ test.txt
```
在行首添加一个a字符: 
```
sed’s/^/a/g’    test.txt
```
在行尾添加一个a字符: 
``` 
sed’s/$/a/‘     tets.txt
```
在特定行后添加一个c字符:
``` 
sed ‘/wuguangke/ac’ test.txt
```
在行前加入一个c字符: 
```
sed’/wuguangke/ic’ test.txt
```
## 19、如何判断某个目录是否存在，不存在则新建，存在则打印信息。
```
 
if
 
[! –d /data/backup/];then
 
Mkdir–p /data/backup/
 
else
 
echo  "The Directory alreadyexists,please exit"
 
fi
```
**注解**：if…;then …else ..fi：为if条件语句,!叹号表示反义“不存在“，-d代表目录。
## 20、监控linux磁盘根分区，如果根分区空间大于等于90%，发送邮件给Linux SA
### (1)、打印根分区大小
```
df -h |sed -n '//$/p'|awk '{print $5}'|awk –F ”%” '{print $1}'
```
**注解**：awk ‘{print $5}’意思是打印第5个域，-F的意思为分隔，例如以%分隔，简单意思就是去掉百分号，awk –F. ‘{print $1}’分隔点.号
### (2)、if条件判断该大小是否大于90，如果大于90则发送邮件报警
```
while sleep 5m
 
do
 
for i in `df -h |sed -n '//$/p' |awk '{print $5}' |sed 's/%//g'`
 
do
 
echo $i
 
if [ $i -ge 90 ];then
 
echo “More than 90% Linux of disk space ,Please LinuxSA Check Linux Disk !” |mail -s “Warn Linux / Parts is $i%” 
 
XXX@XXX.XX
 
fi
 
done
 
done
```
## 21、统计 Nginx 访问日志，访问量排在前20 的 ip地址：
```
cat access.log |awk '{print $1}'|sort|uniq -c |sort -nr |head -20
```
**注解**：sort排序、uniq（检查及删除文本文件中重复出现的行列 ）
## 22、sed另外一个用法找到当前行，然后在修改该行后面的参数：
```
sed -i '/SELINUX/s/enforcing/disabled/' /etc/selinux/config
```
Sed冒号方式 sed -i ‘s:/tmp:/tmp/abc/:g’test.txt意思是将/tmp改成/tmp/abc/。
## 23、打印出一个文件里面最大和最小值：
``` 
cat a.txt |sort -nr|awk ‘{}END{print} NR==1′
 
cat a.txt |sort -nr |awk ‘END{print} NR==1′
```
这个才是真正的打印最大最小值：sed ‘s/ / /g’ a.txt |sort -nr|sed -n ’1p;$p’
## 24、使用snmpd抓取版本为v2的cacti数据方式：
```
snmpwalk -v2c -c public 192.168.0.241
```
## 25、修改文本中以jk结尾的替换成yz：
```
sed -e ‘s/jk$/yz/g’ b.txt
```
## 26、网络抓包：tcpdump
```
tcpdump -nn host 192.168.56.7 and port 80 抓取56.7通过80请求的数据包。
 
tcpdump -nn host 192.168.56.7 or ! host 192.168.0.22 and port 80 排除0.22 80端口！
 
tcp/ip 7层协议物理层–数据链路层-网络层-传输层-会话层-表示层-应用层。
```
## 27、显示最常用的20条命令：
```
 cat .bash_history |grep -v ^# |awk ‘{print $1}’ |sort |uniq -c |sort -nr |head-20
```
## 28、写一个脚本查找最后创建时间是3天前，后缀是*.log的文件并删除。
```
find . -mtime +3  -name "*.log" |xargs rm -rf {} ;
```
## 29、写一个脚本将某目录下大于100k的文件移动至/tmp下。
``` 
find . -size +100k -exec mv {} /tmp ;
```
## 30、写一个防火墙配置脚本，只允许远程主机访问本机的80端口。
``` 
iptables -F
 
iptables -X
 
iptables -A INPUT -p tcp --dport 80 -j accept
 
iptables -A INPUT -p tcp -j REJECT
```
或者
```
iptables -A INPUT -m state --state NEW-m tcp -p tcp --dport 80 -j ACCEPT
```
## 31、写一个脚本进行nginx日志统计，得到访问ip最多的前10个(nginx日志路径：
```
/home/logs/nginx/default/access.log)。
 
cd /home/logs.nginx/default
 
sort -m -k 4 -o access.logok access.1 access.2 access.3 .....
```
## 32.替换文件中的目录
```
sed 's:/user/local:/tmp:g' test.txt
```
或者
```
sed -i 's//usr/local//tmp/g' test.txt
```
