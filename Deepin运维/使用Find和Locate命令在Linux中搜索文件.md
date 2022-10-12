---
title: 使用Find和Locate命令在Linux中搜索文件
description: 
published: true
date: 2022-10-12T05:52:41.081Z
tags: find locate
editor: markdown
dateCreated: 2022-10-12T05:52:41.081Z
---

# 使用Find和Locate命令在Linux中搜索文件
## 按名称搜索
搜索文件最直接的方法就是按名称搜索，要使用 find 命令按名称查找文件，可以用以下语法：

`find -name "query"`

这种方式搜索会区分大小写。如果要按名称查找文件但忽略大小写，请使用 -iname 选项：

`find -iname "query"`

如果要查找不匹配某个关键字的所有文件，可以使用 -not或者! 反转搜索：
```
find -not -name "query_to_avoid"
find ! -name "query_to_avoid"
```

## 按类型搜索

也可以使用 -type 选项指定要查找的文件类型。如下操作，搜索/dev目录录下的 b(块设备)：

`find /dev -type b