---
title: DNS服务
description: 
published: true
date: 2022-10-25T01:12:22.747Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:31:24.875Z
---

## 简介

域名系统（英文：Domain Name System，DNS）是因特网的一项服务，它作为将域名和IP地址相互映射的一个分布式数据库，能够使人更方便的访问互联网。DNS 使用TCP和UDP端口53。当前，对于每一级域名长度的限制是63个字符，域名总长度则不能超过253个字符。

开始时，域名的字符仅限于ASCII字符的一个子集。2008年，ICANN通过一项决议，允许使用其它语言作为互联网顶级域名的字符。使用基于Punycode码的IDNA系统，可以将Unicode字符串映射为有效的DNS字符集。因此，诸如“x.中国”这样的域名可以在地址栏直接输入，而不需要安装插件。但是，由于英语的广泛使用，使用其他语言字符作为域名会产生多种问题，例如难以输入，难以在国际推广等。

## 历史

DNS最早于1983年由保罗·莫卡派乔斯（Paul Mockapetris）发明；原始的技术规范在882号因特网标准草案（RFC 882）中发布。1987年发布的第1034和1035号草案修正了DNS技术规范，并废除了之前的第882和883号草案。在此之后对因特网标准草案的修改基本上没有涉及到DNS技术规范部分的改动。 早期的域名必须以英文句号“.”结尾,当用户访问 www.wikipedia.org 的HTTP服务时必须在址栏中输入： http://www.wikipedia.org .，这样DNS才能够进行域名解析。如今DNS服务器已经可以自动补上结尾的句号。

## 记录类型

DNS系统中，常见的资源记录类型有：

- 主机记录(A记录)：RFC 1035定义，A记录是用于名称解析的重要记录，它将特定的主机名映射到对应主机的IP地址上。
- 别名记录(CNAME记录): RFC 1035定义，CNAME记录用于将某个别名指向到某个A记录上，这样就不需要再为某个新名字另外创建一条新的A记录。
- IPv6主机语录(AAAA记录): RFC 3596定义，与A记录对应，用于将特定的主机名映射到一个主机的IPv6地址。
- 服务位置记录(SRV记录): RFC 2782定义，用于定义提供特定服务的服务器的位置，如主机(hostname)，端口(port number)等。
- NAPTR记录: RFC 3403定义，它提供了正则表达式方式去映射一个域名。NAPTR记录非常著名的一个应用是用于ENUM查询。

## 技术实现

### 概述
DNS 通过允许一个名称服务器把他的一部分名称服务（众所周知的zone）“委托”给子服务器而实现了一种层次结构的名称空间。此外，DNS还提供了一些额外的信息，例如系统别名、联系信息以及哪一个主机正在充当系统组或域的邮件枢纽。

任何一个使用IP的计算机网络可以使用DNS来实现他自己的私有名称系统。尽管如此，当提到在公共的Internet DNS 系统上实现的域名时，术语“域名”是最常使用的。

这是基于13个全球范围的“根服务器”（root-servers.net），除了当中的3个以外，其他都位于美国。从这13个根服务器开始,余下的Internet DNS 命名空间被委托给其他的DNS服务器，这些服务器提供DNS名称空间中的特定部分。

## #软件

DNS系统是由各式各样的DNS软件所驱动的，包括：

- BIND (Berkeley Internet Name Domain), 使用最广的DNS软件
- DJBDNS (Dan J Bernstein's DNS implementation)
- MaraDNS
- NSD (Name Server Daemon)
- PowerDNS

### 域名字符扩展

Punycode是一个根据RFC 3492标准而制定的编码系统，主要用于把域名从地方语言所采用的Unicode编码转换成为可用于DNS系统的编码。而该编码是根据域名相异字表(由IANA制定)，Punycode可以防止所谓的IDN欺骗。

### 域名解析

举一个例子，zh.wikipedia.org作为一个域名就和IP地址208.80.154.225相对应。DNS就像是一个自动的电话号码簿，我们可以直接拨打wikipedia的名字来代替电话号码（IP地址）。DNS在我们直接调用网站的名字以后就会将像zh.wikipedia.org一样便于人类使用的名字转化成像208.80.154.225一样便于机器识别的IP地址。

DNS查询有两种方式：递归 和 迭代。DNS客户端设置使用的DNS服务器一般都是递归服务器，它负责全权处理客户端的DNS查询请求，直到返回最终结果。而DNS服务器之间一般采用迭代查询方式。以查询 zh.wikipedia.org 为例：

客户端发送查询报文"query zh.wikipedia.org"至DNS服务器，DNS服务器首先检查自身缓存，如果存在记录则直接返回结果。

如果记录老化或不存在，则DNS服务器向根域名服务器发送查询报文"query zh.wikipedia.org"，根域名服务器返回 .org 域的权威域名服务器地址。

1.DNS服务器向 .org 域的权威域名服务器发送查询报文"query zh.wikipedia.org"，得到 .wikipedia.org 域的权威域名服务器地址。

2.DNS服务器向 .wikipedia.org 域的权威域名服务器发送查询报文"query zh.wikipedia.org"，得到主机 zh 的A记录，存入自身缓存并返回给客户端。

### WHOIS（域名数据库查询）

一个域名的所有者可以通过查询WHOIS数据库[1]而被找到；对于大多数根域名服务器， 基本的WHOIS由ICANN维护，而WHOIS的细节则由控制那个域的域注册机构维护。

对于240多个国家代码顶级域名(ccTLDs)，通常由该域名权威注册机构负责维护WHOIS。例如中国互联网络信息中心(China Internet Network Information Center)负责 .CN 域名的WHOIS维护，香港互联网注册管理有限公司(Hong Kong Internet Registration Corporation Limited) 负责 .HK 域名的WHOIS维护，台湾网络信息中心 (Taiwan Network Information Center) 负责 .TW 域名的WHOIS维护。

## 争议

### 反对

当前对于DNS系统的控制方式，常常受到指责。最常被攻击的焦点是垄断企业或准垄断企业对DNS的滥用，例如VeriSign公司，以及对于顶级域名的分配。
也有些人宣称很多DNS服务器软件无法在动态IP分配上很好的工作，尽管这是某些特定实现的失败而非协议本身的问题。

### 安全性
此外，一些黑客通过伪造DNS服务器将用户引向错误网站，以达到窃取用户隐私信息的目的。大约有68000台这种DNS服务器。

## 安装与配置

DNS 服务器的作用是自动将域名地址转为IP地址。在架构邮局服务器的时候，往往需要本地调试，显然这离不开 DNS 服务器来做MX记录解析...
### DNS 记录类型

1、A 记录
```
     www	IN	A	192.168.1.101
```
2、别名(CNAME)记录
```
     www	IN	A	192.168.1.101
    mail	IN	CNAME	www
```
3、NS 记录
```
     @	IN	NS	wangyan.org.
     @	IN	A	192.168.1.101
```
### 安装 Bind9

命令安装,终端执行:
```
    sudo apt-get -y install bind9 bind9utils
```
修改DNS服务器地址
```
     echo "nameserver 192.168.1.101" >> /etc/resolv.conf
    /etc/init.d/networking restart #重启网络
```
### DNS 服务器的配置

1．创建正向Zone文件
```
    sudo gedit /etc/bind/named.conf.local
```
添加下面内容，master代表主配置文件
```
    zone "wangyan.org"{
     type master;
     file "db.wangyan.org";
    };
```
2、转发配置。转发的作用在于，如果找不到某个解析，转发到公开的DNS服务器来处理。
```
    sudo gedit  /etc/bind/named.conf.options
    forwarders {
     8.8.8.8;
    };
```
3、添加DNS记录
```
     cp /etc/bind/db.local /var/cache/bind/db.wangyan.org
    vim /var/cache/bind/db.wangyan.org
     :%s/localhost/wangyan\.org/g　#将localhost替换为你的域名
```
### DNS 测试

1、检查DNS是否设置
```
     cat /etc/resolv.conf
```
2、检查MX记录是否生效
```
     nslookup -qt=mx extmail.org (windows)
    host -t mx example.com (linux)
```
3、ping 工具
```
     ping wangyan.org
```
4、dig 工具
```
    dig wangyan.org (linux)
```
5．named-checkzone 工具
```
    named-checkzone wangyan.org /var/cache/bind/db.wangyan.org
```
##相关链接

[维基百科:域名系统](http://zh.wikipedia.org/wiki/%E5%9F%9F%E5%90%8D%E7%B3%BB%E7%BB%9F)

[Debian/Ubuntu 快速架构DNS服务器](http://wangyan.org/blog/debian-setup-dns.html)