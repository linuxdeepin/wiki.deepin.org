---
title: man帮助命令
description: 
published: true
date: 2022-10-26T07:32:03.369Z
tags: 
editor: markdown
dateCreated: 2022-10-26T07:29:49.286Z
---

# man帮助命令
linux中有很多很多很多命令, 包括但不限于 
```
ls
cd
mv
cp
vim
...
```
作为使用人员不可能每个命令都熟练记住里面的参数功能和用法, 这时候就需要
```
man 要寻求帮助的命令名称
```
来显示对应命令的帮助文档了
比如, 如果我想查看 vim 命令的帮助文档只需执行如下命令即可
```
man vim
```
帮助文档可能会新开一个显示页面, 按 Q 键退出即可回到 Shell 界面