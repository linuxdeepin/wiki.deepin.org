---
title: 如何在linux文件夹中搜索包含特定内容、关键字的word文档
description: 
published: true
date: 2022-06-28T05:32:45.544Z
tags: linux word 搜索
editor: markdown
dateCreated: 2022-06-28T05:32:43.316Z
---

# 如何在linux文件夹中搜索包含特定内容、关键字的word文档

只搜索word文档（.doc/.docx）：

[grepdoc.sh（1.61 KB）](https://hu60.cn/q.php/link.url.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvc2gvZWRiMmRhNGFlMTlmYTllMmQ2ZmIwNWI1OTU0Y2E5OWExNjQ0LnNoP2F0dG5hbWU9Z3JlcGRvYy5zaA..)

同时搜索word文档（.doc/.docx）和纯文本（.txt）：

[grepdoctxt.sh（1.79 KB）](https://hu60.cn/q.php/link.url.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvc2gvMWNjY2YwNjQ2ZDI1NjA3ZDZjMmMzMDRkYWQ5ZWIyODQxODM4LnNoP2F0dG5hbWU9Z3JlcGRvY3R4dC5zaA..)

使用方法：

1. 右击脚本，选择“属性”，展开“权限管理”，勾选“允许以程序执行”。

   ![image.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cHM6Ly92a2NleXVndS5jZG4uYnNwYXBwLmNvbS9WS0NFWVVHVS1jYzhjZjA4Zi00OWY1LTRmYzUtODNjMy1lZDJhNjgzNzA0ZDQvN2JlYTljNjEtNWM2YS00ZTBhLWI5NTctZWU0MGY4ZTRiODRmLnBuZw..)

2. 双击脚本，选择“在终端运行”。

   ![image.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cHM6Ly92a2NleXVndS5jZG4uYnNwYXBwLmNvbS9WS0NFWVVHVS1jYzhjZjA4Zi00OWY1LTRmYzUtODNjMy1lZDJhNjgzNzA0ZDQvNzQwNDc5NGQtNWNhMy00YmVjLTkyYWEtYjIyYzg2YmIzMmFlLnBuZw..)

![mmexport1640349453924.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nL2M2YjFjZWFlZDRkMzI3Yjk2YTZjYmM0ODQ4MjZjYzY5NDA1MTQucG5n)

需要安装`catdoc`和`docx2txt`命令，首次启动脚本时它会尝试自动安装。如果安装失败，请自行用命令安装，如：

```undefined
sudo apt install catdoc docx2txt
```