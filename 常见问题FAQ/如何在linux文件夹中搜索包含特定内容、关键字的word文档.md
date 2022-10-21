---
title: 如何在linux文件夹中搜索包含特定内容、关键字的word文档
description: 
published: true
date: 2022-10-21T15:19:12.418Z
tags: office
editor: markdown
dateCreated: 2022-06-28T05:32:43.316Z
---

只搜索word文档（.doc/.docx）：

```bash
#!/bin/bash
# 安装依赖
hash catdoc docx2txt iconv grep || {
    echo "缺少依赖命令，正在尝试自动安装"
    sudo apt install catdoc docx2txt libc-bin grep

    hash catdoc docx2txt iconv grep && {
        echo
        echo "依赖安装成功"
        echo
    } || {
        echo
        echo "依赖安装失败，脚本无法运行"
        echo "请点击终端上的[X]关闭该窗口"
        while true; do read; done
        exit
    }
}

# 读取参数
while true; do
    echo -n "请输入搜索文件夹："
    read dir

    # 去除开头的file://
    dir="${dir/#file:\/\//}"

    # 未输入文件夹，搜索当前文件夹
    if [ "$dir" = "" ]; then
        dir="$(realpath "$PWD")"
    fi

    if [ -e "$dir" ]; then
        break
    else
        echo "搜索文件夹不存在"
    fi
done

while true; do
    echo -n "请输入搜索关键字："
    read keyword

    if [ "$keyword" = "" ]; then
        echo "搜索关键词不能为空"
    else
        break
    fi
done

echo
echo "在 \"$dir\" 中寻找包含 \"$keyword\" 的word文件"
echo

find "$dir" -type f | while read f; do
    content=`
        cat "$f" | docx2txt 2>/dev/null | grep -ai "$keyword"
        catdoc "$f" 2>/dev/null | grep -ai "$keyword"
    `
    if [ "$content" != "" ]; then
        found="true"
        echo -------------------------------------------------------------
        echo "$f"
        echo
        echo "$content" | head
        echo
    fi
done

echo
echo "搜索完毕"
echo "可点击终端上的[X]关闭该窗口"

# 防止终端自动关闭，用户可点击[X]关闭终端
while true; do read; done
```

同时搜索word文档（.doc/.docx）和纯文本（.txt）：

```bash
#!/bin/bash
# 安装依赖
hash catdoc docx2txt iconv grep || {
    echo "缺少依赖命令，正在尝试自动安装"
    sudo apt install catdoc docx2txt libc-bin grep

    hash catdoc docx2txt iconv grep && {
        echo
        echo "依赖安装成功"
        echo
    } || {
        echo
        echo "依赖安装失败，脚本无法运行"
        echo "请点击终端上的[X]关闭该窗口"
        while true; do read; done
        exit
    }
}

# 读取参数
while true; do
    echo -n "请输入搜索文件夹："
    read dir

    # 去除开头的file://
    dir="${dir/#file:\/\//}"

    # 未输入文件夹，搜索当前文件夹
    if [ "$dir" = "" ]; then
        dir="$(realpath "$PWD")"
    fi

    if [ -e "$dir" ]; then
        break
    else
        echo "搜索文件夹不存在"
    fi
done

while true; do
    echo -n "请输入搜索关键字："
    read keyword

    if [ "$keyword" = "" ]; then
        echo "搜索关键词不能为空"
    else
        break
    fi
done

echo
echo "在 \"$dir\" 中寻找包含 \"$keyword\" 的word文件"
echo

find "$dir" -type f | while read f; do
    content=`
        cat "$f" | docx2txt 2>/dev/null | grep -Ii "$keyword"
        catdoc "$f" 2>/dev/null | grep -Ii "$keyword"
        cat "$f" | grep -Ii "$keyword"
        cat "$f" | iconv -f gb18030 -t utf-8 2>/dev/null | grep -Ii "$keyword"
        cat "$f" | iconv -f big5 -t utf-8 2>/dev/null | grep -Ii "$keyword"
    `
    if [ "$content" != "" ]; then
        found="true"
        echo -------------------------------------------------------------
        echo "$f"
        echo
        echo "$content" | head
        echo
    fi
done

echo
echo "搜索完毕"
echo "可点击终端上的[X]关闭该窗口"

# 防止终端自动关闭，用户可点击[X]关闭终端
while true; do read; done
```

使用方法：

1. 右击脚本，选择“属性”，展开“权限管理”，勾选“允许以程序执行”。

   ![image.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cHM6Ly92a2NleXVndS5jZG4uYnNwYXBwLmNvbS9WS0NFWVVHVS1jYzhjZjA4Zi00OWY1LTRmYzUtODNjMy1lZDJhNjgzNzA0ZDQvN2JlYTljNjEtNWM2YS00ZTBhLWI5NTctZWU0MGY4ZTRiODRmLnBuZw..)

2. 双击脚本，选择“在终端运行”。

   ![image.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cHM6Ly92a2NleXVndS5jZG4uYnNwYXBwLmNvbS9WS0NFWVVHVS1jYzhjZjA4Zi00OWY1LTRmYzUtODNjMy1lZDJhNjgzNzA0ZDQvNzQwNDc5NGQtNWNhMy00YmVjLTkyYWEtYjIyYzg2YmIzMmFlLnBuZw..)

![mmexport1640349453924.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nL2M2YjFjZWFlZDRkMzI3Yjk2YTZjYmM0ODQ4MjZjYzY5NDA1MTQucG5n)

需要安装`catdoc`和`docx2txt`命令，首次启动脚本时它会尝试自动安装。如果安装失败，请自行用命令安装，如：

```bash
sudo apt install catdoc docx2txt
```