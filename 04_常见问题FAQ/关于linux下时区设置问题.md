---
title: 关于linux下时区设置问题
description: 
published: true
date: 2023-06-29T02:55:04.355Z
tags: 
editor: markdown
dateCreated: 2023-06-29T02:53:19.880Z
---

## linux时间8个小时差,Linux下Chrome浏览器时间不对，时区不对

通常规定时区的都是比较权威的机构。比如说我们用的中国标准时间（通常叫「北京时间」）就是中国官方确定的。而在互联网上，时区是由IANA管理的（这个机构还管IP地址和域名的分配）。IANA曾经是美国政府管辖的，因为互联网最早就是从美国诞生，后来为了国际合作的需要创建了ICANN，并把IANA让渡给了这个国际组织。

IANA创建的时区规范是tz database（通常简称tzdata），在GNU/Linux系统里，这个数据库存放在 /usr/share/zoneinfo/目录中。每个时区是一个用二进制格式存放的文件，同时还有一些辅助的文本文件，比如说 /usr/share/zoneinfo/zone.tab这个文件存放了一些时区的国家或地区代码、坐标、时区名称和注释。我们通常可以选择的时区就是在这个文件中定义的，称为主要（Canonical）时区。还有一些时区虽然在这个文档中没有定义，但是存在相应的时区文件，叫做链接（Link）时区。这些时区一般是曾经有用但是后来被废弃或者改名的，作为向后兼容使用。

tzdata的时区名称通常是用一个城市来命名。在 /usr/share/zoneinfo/Asia/目录下，可以找到5个中国大陆的城市，分别对应民国时代的五个时区。

- 昆仑时区：Asia/Kashgar 喀什
- 新藏时区：Asia/Urumqi 乌鲁木齐
- 陇蜀时区：Asia/Chongqing和Asia/Chungking 重庆
- 中原标准时区：Asia/Shanghai 上海
- 长白时区：Asia/Harbin 哈尔滨

在这些时区中，Asia/Kashgar、Asia/Chongqing、Asia/Chungking、Asia/Harbin已经被弃用，除了Asia/Kashgar被Asia/Urumqi替换外，其他的都替换成了Asia/Shanghai。在zone.tab文件中可以看到Asia/Shanghai后面注释了Beijing Time（即北京时间，中国标准时间），Asia/Urumqi后面注释了Xinjiang Time（新疆民间有时候还在用这个时区。有的版本后面还写了Vostok，是一个南极科考站）

虽然时区的定义是比较灵活的，但是一旦使用互联网发送信息都需要遵循IANA的标准，因此BSD系列和GNU系列都是直接选择了tzdata作为系统内置的时区标准。而Windows等虽然似乎系统时间并不是采用tzdata，但是互联网应用程序仍然需要使用tzdata的。在浏览器控制台可以执行这个代码查看本机的时区：Intl.DateTimeFormat().resolvedOptions().timeZone;

感谢论坛用户：enforcee的分享～