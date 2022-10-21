---
title: python入门
description: 
published: true
date: 2022-10-21T05:08:08.804Z
tags: 
editor: markdown
dateCreated: 2022-10-21T05:07:22.359Z
---

# 黑客000的Python简易教程

---

## 第一讲 初识Python

Python（英国发音：/ˈpaɪθən/ 美国发音：/ˈpaɪθɑːn/）, 是一种面向对象的解释型计算机程序设计语言，由荷兰人Guido van Rossum于1989年发明，第一个公开发行版发行于1991年。
本章节将向大家介绍 Python 最新版开发环境的安装步骤及如何进行环境配置，刚入门的新手学员可以跟着本章内容进行学习。
本章节需要用到的Python 最新源码，二进制文档，新闻资讯等可以在[Python官网](https://www.python.org/)查看：
Unix & Linux 平台安装 Python:
以下为在 Unix & Linux 平台上安装 Python 的简单步骤：
打开 WEB 浏览器访问[Python下载地址](https://www.python.org/downloads/source/)
选择适用于 Unix/Linux 的源码压缩包。
下载及解压压缩包 Python-3.x.x.tgz，3.x.x 为你下载的对应版本号。
如果你需要自定义一些选项修改 Modules/Setup

```-
root@localhost :/root# tar -zxvf Python-3.8.5.tgz
root@localhost :/root# cd Python-3.8.5
root@localhost :/root# ./configure
root@localhost :/root# make && make install
```

# 黑客000的Python简易教程

---

## 第一讲 初识Python

Python（英国发音：/ˈpaɪθən/ 美国发音：/ˈpaɪθɑːn/）, 是一种面向对象的解释型计算机程序设计语言，由荷兰人Guido van Rossum于1989年发明，第一个公开发行版发行于1991年。
本章节将向大家介绍 Python 最新版开发环境的安装步骤及如何进行环境配置，刚入门的新手学员可以跟着本章内容进行学习。
本章节需要用到的Python 最新源码，二进制文档，新闻资讯等可以在[Python官网](https://www.python.org/)查看：
Unix & Linux 平台安装 Python:
以下为在 Unix & Linux 平台上安装 Python 的简单步骤：
打开 WEB 浏览器访问[Python下载地址](https://www.python.org/downloads/source/)
选择适用于 Unix/Linux 的源码压缩包。
下载及解压压缩包 Python-3.x.x.tgz，3.x.x 为你下载的对应版本号。
如果你需要自定义一些选项修改 Modules/Setup

```-
root@localhost :/root# tar -zxvf Python-3.8.5.tgz
root@localhost :/root# cd Python-3.8.5
root@localhost :/root# ./configure
root@localhost :/root# make && make install
```



Windows 平台安装 Python:
以下为在 Windows 平台上安装 Python 的简单步骤：
打开 WEB 浏览器访问[Windows版Python下载地址](https://www.python.org/downloads/windows/)
在下载列表中选择 Windows 平台安装包，包格式为：python-3.8.5-amd64.exe 文件 ， -3.8.5 为你要安装的版本号，-amd64为你系统的版本。
下载后，双击下载包，进入 Python 安装向导，安装非常简单，你只需要使用默认的设置一直点击"下一步"直到安装完成即可。
Windows x86-64 为 64 位版本
Windows x86 为 32 位版本
自python3.9起，官方不再支持在win7及以下版本安装和运行python，但我们仍可通过编译源代码的形式来给win7安装更高版本的python。

---

## 第二讲 第一个程序

### 2.0 开发第一个程序

首先右键桌面，新建一个文件命名为1.py
然后在里面写上以下内容:

![2.0-0.png](https://storage.deepin.org/thread/202209031428372336_2.0-0.png)

我们的第一个程序写好了

---

### 2.1 运行第一个程序

接下来如何运行它呢?
首先，右键桌面的空白处,点击终端打开

![2.1-0.png](https://storage.deepin.org/thread/202209031429054327_2.1-0.png)

然后在终端里面输入chmod a+x ./1.py&&./1.py

![2.1-1.png](https://storage.deepin.org/thread/202209031429179666_2.1-1.png)

看到了吧，程序输出了Goodbye World，程序要和世界拜拜了

---

### 2.2 调试第一个程序

阿偶，好像落了点什么，应该这样写

![2.2-0.png](https://storage.deepin.org/thread/202209031429286857_2.2-0.png)

这下好了，再运行一下它，程序就和世界拜拜了。

[视频](https://hacker-000000.github.io/demo001/2.2-1.mp4.html)

---

## 第三讲 Python基础语法

### 3.0 Python 保留字

保留字即关键字，我们不能把它们用作任何标识符名称。Python 的标准库提供了一个关键词模块，我们可以使用它来查看当前版本的所有保留字：

```python
import keyword
print(keyword.kwlist)
```

我们可以看到，关键词有这些: \['False', 'None', 'True', '\_\_peg\_parser\_\_', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield'\]
这些关键词不能作为变量，列表，字典，类与对象的名称。

---

### 3.1 代码注释

注释是给人看的，机器会忽略这些内容。写代码时写注释是很好的习惯。要不然代码长了，没有注释，调试起来肯定很麻烦。
Python 中单行注释以 # 开头，多行注释采用三对单引号（'''）或者三对双引号（"""）将注释括起来。
例:

```python
#!/usr/bin/python3
# 这是单行注释
import os#这也是单行注释
print("Goodbye World")
"""
这是多行注释
这也是多行注释
这也是多行注释
"""
```

---

### 3.2 缩进

Python 最具特色的就是使用缩进来表示代码块。缩进的空格数是可变的，但是同一个代码块的语句必须包含相同的缩进空格数。
小贴士:缩进不一定需要使用空格，可以使用Tab按键(键盘上Q左面，Caps上面的按键)。

---

### 3.3 数据类型

Python 中有7个标准的数据类型：

| **名称** | **类型** | 样例                          |
| -------------- | -------------- | ----------------------------- |
| int            | 整数           | 0                             |
| float          | 小数           | 0.1                           |
| str            | 字符串         | "你好"                        |
| list           | 列表           | [0,1,2]                       |
| tuple          | 元组           | (0,1,2)                       |
| set            | 集合           | set(0,3,2,2,1)                |
| dict           | 字典           | {"name":"警察","phone":"110"} |

Python3 的7个标准数据类型中：
不可变数据（4个）：int,float,str,tuple
可变数据（3 个）：list,set,dict
可变数据和不可变数据是相对于引用地址来说的，不可变数据类型不允许变量的值发生变化，如果改变了的变量的值，相当于新建了一个对象，而对于相同的值的对象，内部会有一个引用计数来记录有多少个变量引用了这个对象。可变数据类型允许变量的值发生变化。对变量进行修改操作只会改变变量的值，不会新建对象，变量引用的地址也不会发生变化，不过对于相同的值的不同对象，在内存中则会存在不同的对象，即每个对象都有自己的地址，相当于内存中对于同值的对象保存了多份，这里不存在引用计数，是实实在在的对象。
简单地讲，可变数据和不可变数据的“变”是相对于引用地址来说的，不是不能改变其数据，而是改变数据的时候会不会改变变量的引用地址。

---

### 3.4 类型判断

python可以用type函数来检查一个变量的类型，使用方法如下：

![3.4-0.png](https://storage.deepin.org/thread/202209031429532754_3.4-0.png)

在脚本代码中的使用：

```python
#!/usr/bin/python3
str0="字符串"
print(type(str0))
int0=1234
print(type(int0))
```

---

### 3.5 字符串

#### 3.5.0 字符串基础

Python 中单引号和双引号使用完全相同，但单引号和双引号不能匹配。
例:

![3.5.0-0.png](https://storage.deepin.org/thread/202209031430106929_3.5.0-0.png)

使用三对引号('''或""")可以囊括一个多行字符串。
例:

[视频](https://hacker-000000.github.io/demo001/3.5.0-1.mp4.html)

其实在Python中，单独一行字符串是可以作为注释的。
与其他编程语言相似，python也使用 '\\'作为转义字符
换行符为\\n，反斜杠(\\)为\\\\(防止转义)
字符串可以用 + 运算符连接在一起，用 \* 运算符重复。
Python 没有单独的字符类型，一个字符就是长度为 1 的字符串。

---

#### 3.5.1 字符串切片

字符串切片:str0\[n\]:   str0的第n+1个字符，例如str0\[0\]为str0的第一个字符
字符串切片:str0\[start:stop\]: str0从start+1个字符开始,到stop截止
字符串切片:str0\[start:stop:step\]: str0从start+1开始,到stop截止,间隔step-1

```python
#!/usr/bin/python3
str0="qwertyuiop"
print(str0)                 # 输出字符串 qwertyuiop
print(str0[0:-1:1])         # 输出第一个到倒数第二个的所有字符qwertyuio
print(str0[0])              # 输出字符串第一个字符q
print(str0[2:5])            # 输出从第三个开始到第五个的字符ert
print(str0[2:])             # 输出从第三个开始后的所有字符ertyuiop
print(str0[1:5:2])          # 输出从第二个开始到第五个且每隔两个的字符wr
print(str0[::-1])           # 倒序输出字符串poiuytrewq
print(str0 * 2)             # 输出字符串两次qwertyuiopqwertyuiop
```

---

#### 3.5.2 字符串格式化

#### .format方法

format() 方法格式化指定的值，并将其插入字符串的占位符内。
format() 方法返回格式化的字符串。
可以使用命名索引 {price}、编号索引{0}、甚至空的占位符 {} 来标识占位符。

例:

```python
  >>> "字{}串{}式{}".format("符","格","化")
'字符串格式化'
  >>> "字符{chuan}{ge}式化".format(ge="格",chuan="串")
'字符串格式化'
  >>> "{2}{1}{0}".format("式化","串格","字符")
'字符串格式化'
```

#### f-string

#### 介绍

f-string是python3.6开始引入的新语法，相比于之前的%和format方法，f-string方法能更快速直观的格式化字符串。
f-string形式为：f"{content:format}"，其中，f为标识符，表示字符串为f-string;
content是替换并填入字符串的内容，可以是变量，表达式或者函数;
.format是格式描述符，默认为空。
下面对比了三种格式化字符串的方法：

```python
var='good'
  >>> # %方法
  >>>'python is %s' % var
'python is good'
  >>>
  >>> # format方法
  >>> 'python is {}'.format(var)
'python is good'
  >>>
  >>> # f-string方法
  >>> f'python is {var}'
'python is good'
```

#### 转义

在f-string中，{}是作为占位符替换变量用的，具有特殊含义，如果要在f-string中显示{}本身，则需要对应使用{}进行转义。
例:

```python
  >>> f"我是{{"
'我是{'
  >>> f"我是}}"
'我是}'
  >>> f"我们是{{}}"
'我们是{}'
  >>> f'我们是}}{{'
'我们是}{'
  >>> f"我是{"
  File "<stdin>", line 1
    f"我是{"
          ^
SyntaxError: f-string: expecting '}'
```

#### 表达式

f-string中的大括号不仅可以填入变量，也可以填入表达式或者调用函数。
表达式可以在{}中定义表达式，python会自动计算出结果。

```python
  >>> # 定义变量
  >>> var=10
  >>> # f-string表达式
  >>> f'{var+1}'
'11'
```

#### 使用函数

可以在{}中调用函数或方法。
也可以自定义一个函数，然后在{}中进行调用。

```python
  >>> var='jack'
  >>> f'{var.upper()}'
'JACK'
  >>> def plus(x):
...     return x+' python'
  >>> var='hello'
  >>> f'{plus(var)}'
'hello python'
```

## END