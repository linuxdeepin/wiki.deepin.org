---
title: Wxpython
description: 
published: true
date: 2022-05-07T07:48:53.903Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:45:03.996Z
---

wxPython是一个Python包装wxWidgets（这是用 C++ 编写），一个流行的跨平台GUI工具包。由Robin Dunn以及Harri Pasanen开发，wxPython是作为一个Python扩展模块。
就像wxWidgets，wxPython也是一个免费的软件。它可以从官方网站下载： <http://wxpython.org>. 在本网站上可下载 wxPython 对应操作系统平台二进制和源代码。 在wxPython API主要模块包括一个核心模块。它由 wxObject 类，这是基础 API 的所有类。控制模块包含了所有 GUI 应用程序开发中使用的部件。 例如，wx.Button，wx.StaticText（类似于一个标签），wx.TextCtrl（可编辑的文本控制）等。 wxPython 的API有GDI（图形设备接口）模块。这是一组用于在部件中的绘图类。 如字体，颜色，画笔等类就是其中的一部分。所有的容器窗口类是由 Windows 模块定义。 wxPython 官方网站也主持 Phoenix 工程计划 – 为Python3.* 新实现的wxPython。 它着重于提高速度，可维护性和可扩展性。该项目始于2012年开始，现仍处于测试阶段。

经测试，deepin15.8中python3用pip方式安装失败

pip安装命令：
sudo apt-get install python3-pip

wxpython安装命令：
安装命令pip3 install -U wxPython

Python 3.6.5 (default, May 11 2018, 13:30:17)
[GCC 7.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import wx
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'wx'
>>>
此种wxpython安装方法deepin和win10系统安装均报错

下面是win10报错代码

>>> import wx
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Users\chenl\AppData\Local\Programs\Python\Python37\lib\site-packages\wx\__init__.py", line 17, in <module>
    from wx.core import *
  File "C:\Users\chenl\AppData\Local\Programs\Python\Python37\lib\site-packages\wx\core.py", line 12, in <module>
    from ._core import *
ImportError: DLL load failed: 找不到指定的模块。

python3.7.1配wxPython-4.0.3-cp37-cp37m-win_amd64文件安装的

### 求高手编辑教程帖
