---
title: Apt-mark
description: 
published: true
date: 2022-05-08T07:58:08.011Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:29:04.106Z
---

在执行系统更新时，有时候我想阻止某些个别的包升级， Debian 系下可以使用 apt-mark 命令。
# 阻止包
```
sudo apt-mark hold <pkg>
```
比如，阻止升级 Perl，执行 apt-mark hold perl 即可。
# 取消阻止
如果不想阻止了，那么可以通过 apt-mark unhold 取消：
```
sudo apt-mark unhold <pkg>
```
# 显示已阻止的包
要查看已经被阻止的包，则可以执行：
```
sudo apt-mark showhold
```