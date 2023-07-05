---
title: grep详解
description: Global search REgular expression and Print out the line.文本搜索工具，根据用户指定的“模式（过滤条件)”对目标文本逐行进行匹配检查，打印匹配到的行.
published: true
date: 2023-06-02T02:18:18.020Z
tags: 
editor: markdown
dateCreated: 2023-06-02T02:18:16.632Z
---

# 一、grep基本介绍
全拼:Global search REgular expression and Print out the line.

作用:文本搜索工具，根据用户指定的“模式（过滤条件)”对目标文本逐行进行匹配检查，打印匹配到的行.

模式:由正则表达式的元字符及文本字符所编写出的过滤条件﹔
![grep1.png](/grep详解/grep1.png)
grep命令是Linux系统中最重要的命令之一，功能是从文本文件或管道数据流中筛选匹配的行和数据，如果再配合正则表达式，功能十分强大，是Linux运维人员必备的命令
grep命令里的匹配模式就是你想要找的东西，可以是普通的文字符号，也可以是正则表达式
![grep2.png](/grep详解/grep2.png)
# 二、正则表达式grep实践

![grep3.png](/grep详解/grep3.png)

### 2.1、输出以 I 开头的行(不区分大小写)
![grep4.png](/grep详解/grep4.png)
注: 这里的-i代表不区分大小写, -n代表显示匹配行和行号
### 2.2、输出以.结尾的行
![grep5.png](/grep详解/grep5.png)
注: 因为.在这里有着特殊含义, 所以要用\转义一下, 如果不加转义字符的话, grep就会把它当做正则表达式来处理(.代表的含义是匹配任意一个字符)
### 2.3、$符号
- 注意在Linux平台下, 所有文件的结尾都有一个$符
- 可以利用cat -A 查看文件
![grep6.png](/grep详解/grep6.png)
### 2.4、^$(代表空行的意思)组合符
找出文件的空行, 以及行号
![grep7.png](/grep详解/grep7.png)
### 2.5、.点符号
"."点表示任意一个字符, 有且只有一个, 不包含空行
![grep8.png](/grep详解/grep8.png)
### 2.6、*符号
"*"表示找出前一个字符0次或一次以上

找出文件中i出现0次或多次的行和行号
![grep9.png](/grep详解/grep9.png)
### 2.7、.*组合符
".*"表示所有内容, 包括空行
![grep10.png](/grep详解/grep10.png)
### 2.8、^.*t符 (含义: 以任意内容开头, 直到t结束)
![grep11.png](/grep详解/grep11.png)

### 2.9、[abc]中括号
中括号表达式,[abc]表示匹配中括号中任意一个字符, a或b或c,常见的形式如下;

- [a-z]匹配所有小写单个字母[A-Z]匹配所有单个大写字母
- [a-zA-Z]匹配所有的单个大小写字母
- [0-9]匹配所有单个数字
- [a-zA-ZO-9]匹配所有数字和字母
匹配abc字符中的任意一个,得到它的行数和行号 
![grep12.png](/grep详解/grep12.png)
### 2.10、grep的参数-o
使用"-o"选项, 可以值显示被匹配到的关键字, 而不是讲整行的内容都输出.
![grep13.png](/grep详解/grep13.png)
显示出文章中有多少行有a
![frep14.png](/grep详解/frep14.png)
"-c"只统计匹配的行数

###2.11、[^abc]中括号中去反
[^abc]或[^a-c]这样的命令, "^"符号在中括号中第一位表示排除, 就是排除字符a,b,c

注: 出现再中括号里的尖角号表示取反
![grep15.png](/grep详解/grep15.png)
# 三、扩展正则表达式grep实践
此处使用grep -E进行实践扩展正则, egrep官网已经弃用了

### 3.1、+号
+号表示匹配前一个字符1一次或多次,必须使用grep-E扩展正则
![grep16.png](/grep详解/grep16.png)
###3.2、?符
匹配前一个字符0次或1次

找出文件中包含gd或者god的行
![grep17.png](/grep详解/grep17.png)
### 3.3、|符
竖线|再正则中是或者的意思

找出opt目录中txt结尾的文件, 其名字中包含a或者e, 不区分大小写(-i)
![grep18.png](/grep详解/grep18.png)
### 3.4、()小括号
将一个或多个字符捆绑在一起, 当作一个整体进行处理
![grep19.png](/grep详解/grep19.png)
### 3.5、{n,m}匹配次数
{n,m}:匹配前一个字符至少n次, 最多m次

{n,}: 匹配前一个字符至少n次, 没有上限

{,m}: 匹配前一个字符最多m次,可以没有

重复前一个字符各种次数, 可以通过-o参数显示明确的匹配过程
![grep20.png](/grep详解/grep20.png)