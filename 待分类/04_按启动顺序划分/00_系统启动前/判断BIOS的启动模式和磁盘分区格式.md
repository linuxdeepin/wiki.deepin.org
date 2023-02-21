---
title: 判断BIOS的启动模式和磁盘分区格式
description: 
published: true
date: 2022-06-23T06:04:21.619Z
tags: 启动模式, 磁盘分区, uefi, legacy
editor: markdown
dateCreated: 2022-06-23T06:04:18.749Z
---

# 判断BIOS的启动模式和磁盘分区格式
本篇分享来自道雪仙尘一剑灯的[《判断BIOS的启动模式和磁盘分区格式》](https://bbs.deepin.org/zh/post/225766)

### BIOS启动模式
我们先了解一下电脑的BIOS启动模式，见下图：
![1.png](/for_trans/bios启动模式/1.png)

BIOS启动模式分为UEFI引导和Legacy引导：

- UEFI是新式的BIOS启动引导，对应的磁盘分区格式是GPT，它可以跳过BIOS自检，启动速度更快，现在的新机型都是UEFI启动
- Legacy则是传统的BIOS启动引导，对应的磁盘分区格式是MBR，旧电脑一般都是这种引导

这里需要注意的是:
BIOS启动模式和硬盘分区格式如果不一致，安装的时候会出现报错，这就是很多用户反馈deepin系统装不上的主要原因。
所以一定要确保UEFI引导对应GPT磁盘，Legacy引导对应MBR磁盘。

![2.png](/for_trans/bios启动模式/2.png)

### 判断BIOS的启动模式和磁盘分区格式

同时按下“Win+R”组合快捷键，输入命令“msinfo32”按下回车，在系统信息表中可以查看电脑BIOS的启动模式。
![3.png](/for_trans/bios启动模式/3.png)

用同样的方法输入命令“cmd”按下回车，在弹出的窗口界面输入命令“diskpart”按下回车，继续输入“list disk”按下回车。
![4.png](/for_trans/bios启动模式/4.png)

在这里可以查看电脑的磁盘分区格式，GPT下方有星号则是GPT磁盘。

将MBR磁盘转换成GPT磁盘。

如果你是UEFI启动，但你的磁盘分区格式是MBR，继续输入以下命令：
```linux
select disk 1
convert gpt
```
![5.jpg](/for_trans/bios启动模式/5.jpg)

可以把原本的MBR磁盘转换成GPT磁盘，这里是假设我们的磁盘格式为MBR，要将编号为1的磁盘转换成GPT分区系统。

我们可以根据实际需要更改磁盘编号和磁盘格式。

