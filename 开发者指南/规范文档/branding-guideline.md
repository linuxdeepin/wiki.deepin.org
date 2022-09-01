---
title: 品牌专有名词指导方针
description: 面向开发者的，与 deepin 品牌相关的专有名词使用指导文档
published: true
date: 2022-08-31T06:04:43.850Z
tags: 
editor: markdown
dateCreated: 2022-08-31T06:04:43.850Z
---

# 在研发项目中正确使用品牌专有名词

> 注：非研发人员也应当注意相应的品牌名称用法，应当一并提供相应的文档页面。
{.is-info}

## deepin

### 总述

deepin 是指由 deepin.org 所有发行的发行版本。在指代发行版本时，应该永远使用小写的 “deepin”。

### 使用

在文档，图片中，需要使用全小写的 ‘deepin‘，即使是首字母，也应该使用小写。

```
deepin is an opensoucre os. // 正确
Deepin is an opensoucre os. // 错误，即使是段落首字母，也不应该大写
```

在代码中，需要使用全小写的 ‘deepin‘，除非代码风格规定了必须使用全大写，或首字母大小的情况。

``` c
#define DEEPIN_MACRO XXXX // 正确，以代码规范为准
const int kDeepinNumber = 1; // 正确，以代码规范为准 

// 版权信息中也需要使用小写的deepin
// * Copyright (c) 2021. deepin All rights reserved.
```

在文件名中，需要使用全小写的 ‘deepin‘。

```
/usr/lib/deepin-daemon/dde-system-daemon // 正确
/usr/share/Deepin/msc/res // 错误，应该为 /usr/share/deepin/msc/res 
```

### 例外

当 deepin 和其他名词组成专有名词时，可以使用大小写混合，例如：

``` ini
# desktop文件中，deepin-music相关的应用
[Desktop Entry]
Name=Deepin Music
```

这里 Deepin Music 是一个专有名词，在任何情况下都不可以拆开使用。 

## DDE

### 总述

DDE 是 Deepin Desktop Environment 的缩写。 

> Deepin Desktop Environment 也是专有名词，不要拆开使用，也不要写成deepin Desktop Environment，deepin desktop environment 等形式。 
{.is-info}

### 使用

在文档，图片中，需要使用全大写的 ‘DDE‘。

```
The DDE is comprised of the Desktop Environment, deepin Window Manager, ,→ Control Center, Launcher and Dock. // 正确
Use dde in other os. // 错误，文档中只有大写
Login to Dde. // 错误，文档中不允许混合大小写
```

在代码中，需要使用全大写的 ‘DDE‘，除非代码风格规定了必须使用全小写，或首字母大写的情况。

``` c
#define DDE_MACRO XXXX // 正确，以代码规范为准
const int kDdeNumber = 1; // 正确，以代码规范为准
```

在文件名中，需要使用全小写的 ‘dde‘。

```
/usr/lib/deepin-daemon/dde-system-daemon // 正确 
```