---
title: 编写定期自动运行程序
description: 
published: true
date: 2022-10-18T02:53:51.249Z
tags: 
editor: markdown
dateCreated: 2022-10-17T02:15:21.137Z
---

### 定期自动运行程序

Linux有一个称为crond的守护程序，主要功能是周期性地检查 /var/spool/cron目录下的一组命令文件的内容，并在设定的时间执行这些文件中的命令。用户可以通过crontab 命令来建立、修改、删除这些命令文件。

例如，建立文件crondFile，内容为“00 9 23 Jan *HappyBirthday”，运行`crontab cronFile`后，每当元月23日上午9:00系统自动执行“HappyBirthday”的程序（“*”表示不管当天是星期几）。

### 定时自动运行程序一次

定时执行命令at 与crond 类似（但它只执行一次）：命令在给定的时间执行，但不自动重复。at命令的一般格式为：

     at [ -f file ] time 
在指定的时间执行file文件中所给出的所有命令。也可直接从键盘输入命令：

     $ at 12:00
     at> mailto Roger －s ″Have a lunch″ < plan.txt
     at> (Ctrl+D)
     Job 1 at 2000－11－09 12:00

执行的内容为:2000-11-09 12:00时候自动发一标题为“Have a lunch”，内容为plan.txt文件内容的邮件给Roger。