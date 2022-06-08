---
title: Qt_Creator
description: 
published: true
date: 2022-05-07T07:49:45.869Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:40:43.398Z
---

## 简介

Qt Creator是跨平台的轻量级集成开发环境，它包括项目生成向导、C++代码编辑器、浏览文件，集成了Qt Designer、Qt Assistant、QtLinguist、图形化的GDB 调试前端，集成qmake 构建工具等功能。

## 安装

`sudo apt-get install qtcreator`

## 卸载

`sudo apt-get remove qtcreator`

## 仓库地址

[http://packages.deepin.com/deepin/pool/main/q/qtcreator/](http://packages.deepin.com/deepin/pool/main/q/qtcreator/)

## 常见问题

### 脚本，安装完QT，添加gcc g++ gdb cmake ，修正libGL.so，以及fcitx中文输入支持。
    #!/bin/bash

    sudo apt-get install gcc g++ gdb cmake
    sudo ln -s /usr/lib/i386-linux-gnu/libGL.so.1 /usr/lib/i386-linux-gnu/libGL.so
    sudo ln -s /usr/lib/x86_64-linux-gnu/libGL.so.1 /usr/lib/x86_64-linux-gnu/libGL.so

    #输入中文
    userdir=`env | grep ^HOME= | cut -c 6-`
    qt5Creator=/Qt5.8.0/Tools/QtCreator/lib/Qt/plugins/platforminputcontexts
    qt5gcc_64=/Qt5.8.0/5.8/gcc_64/plugins/platforminputcontexts

    sudo apt-get install fcitx-libs-qt5

    sudo cp  /usr/lib/x86_64-linux-gnu/qt5/plugins/platforminputcontexts/libfcitxplatforminputcontextplugin.so $userdir$qt5Creator
    sudo chmod +x $userdir$qt5Creator/libfcitxplatforminputcontextplugin.so

    sudo cp  /usr/lib/x86_64-linux-gnu/qt5/plugins/platforminputcontexts/libfcitxplatforminputcontextplugin.so $userdir$qt5gcc_64
    sudo chmod +x $userdir$qt5gcc_64/libfcitxplatforminputcontextplugin.so

### 从应用商店安装 Qt Creator，提示错误：qmake: could not exec '/usr/lib/x

终端执行：sudo apt-get install qt5-default

## 相关链接

维基百科：
