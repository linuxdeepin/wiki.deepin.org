---
title: man帮助文档
description: 
published: true
date: 2023-06-12T08:15:52.388Z
tags: 
editor: markdown
dateCreated: 2023-06-12T08:15:51.023Z
---

## 前言
linux 世界是非常重视帮助文档的，但是 linux 用户可能不太重视帮助文档。尤其对于中文用户来说， linux 帮助文档有一些不友好，因为都是英文（几乎）。但是，因为 linux 开源项目很多都有详细的文档，他是我们了解项目的一个最权威，最有效的途径。预期在网络上漫无目的的瞎逛，不如静下心好好读一下文档。

本文将帮助用户了解和有效使用 linux 中的帮助文档。
## man 帮助系统
man 是一个文档帮助系统，它是 man-db 工具和 man-pages 帮助文档的统称。man-pages 有自己的格式，它其实也有多种语言的版本，比如中文是 man-pages-zh_cn 的包（在 arch linux 中）

使用方式是：man 分类 条目

如: man 2 exit，指的是查看 eixt 条目的第二册内容。

分类意义如下：

1.程序
2.系统调用
3.库调用
4.特殊文件
5.文件格式
6.游戏
7.杂项
8.系统管理命令
9.内核例程
10.要查看一个条目有那些手册，可以使用： man -f exit

新安装一个软件，它一般也会自带它自己的 man-pages 帮助文档，所以可以根据命令名称来使用 man 查看它的用法，这就是 man 帮助系统组织的方式。

### man 选项：

k：搜索条目和概要中的关键字
f：显示该条目的所有手册的简短说明
w：显示帮助文档的路径
l：针对文档文件操作
P：格式化工具，默认是 less
H：使用浏览器打开，环境变量 BROWSER 可指定浏览器
```
export BROWSER=/opt/microsoft/msedge-dev/msedge
man -H sh &  # 后台使用浏览器打开帮助，这样可以借助浏览器翻译一下

# 使用 khelpcenter（kde 的帮助中心工具）打开
khelpcenter man:sh &

# 搜索的使用
man -k man # 将在概要里面找，所以匹配的数量有可能很多
man -f man # 在条目里面找，匹配比较精准
man -k "^man$" # 这个正则表达式，等价 man -f man
# khelpcenter 里面的搜索也是 man -k ，所以可以使用正则表达式来减少匹配的数量
```
### info 帮助系统
info 是多页面的帮助系统，比 man 更复杂更新，但是文档数量没有 man 多。
```
# info 我没找到好的方法能转换成 html
# 但是可以输出纯文本
# 将 info ls 输出 ls.txt 
info --subnodes -o ls.txt ls
````
## khelpcenter 帮助系统
这是一个图形界面的帮助工具，它也可以查看系统中的 man 和 info 的文档。但是它有一个 bug，不能正确识别中文 man pages（bug）。

ps。后来我找到 kde 汉化组的人，他们告诉我一个临时的解决方法。
```
# khelpcenter 使用 kio 插件来读取 man
# kio 将中文本地化路径识别为 Chinese
# 而 man pages 使用的是 zh_CN
# 所以建立一个快捷方式，可以临时结局这个问题
sudo ln -s /usr/share/man/zh_CN /usr/share/man/Chinese

# 搜索匹配问题
# 使用正则表达式 "^命令$" 可以定位命令，避免无用信息
```

