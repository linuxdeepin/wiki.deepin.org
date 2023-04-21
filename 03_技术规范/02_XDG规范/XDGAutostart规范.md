---
title: XDGAutostart规范
description: 
published: true
date: 2023-04-20T15:16:42.699Z
tags: freedesktop, dbus
editor: markdown
dateCreated: 2023-04-20T15:16:42.699Z
---

# XDGAutostart规范

原文链接[Desktop Application Autostart Specification](https://specifications.freedesktop.org/autostart-spec/autostart-spec-0.5.html)

## 介绍

这个DRAFT文档定义了在桌面环境启动期间和挂载可移动介质之后自动启动应用程序的方法。

本规范中的一些文件位置是根据“[桌面基本目录规范](https://specifications.freedesktop.org/basedir-spec/)”指定的。

文档中的关键词“MUST”，“MUST NOT”，“REQUIRED”，“SHALL”，“SHALL NOT”，“SHOULD”，“SHOULD NOT”，“RECOMMENDED”，“MAY”和“OPTIONAL”的解释与RFC 2119中描述的一致。

## 应用程序在启动期间自动启动

通过将应用程序的。desktop文件放在自动启动目录中，应用程序将在用户登录后的桌面环境启动期间自动启动。

## 自动启动目录

自动启动目录是`$XDG_CONFIG_DIRS/autostart`，根据“桌面基础目录规范”中的“引用此规范”一节定义。

如果同一个文件名位于多个自动启动目录下，则应该只使用最重要目录下的文件。

例如:如果没有设置`$XDG_CONFIG_HOME`，则用户主目录下的自动启动目录为`~/.config/autostart/`。

例如:如果没有设置`$XDG_CONFIG_DIRS`，则系统范围内的自动启动目录为`/etc/xdg/autostart/`。

例如:如果没有设置`$XDG_CONFIG_HOME`和`$XDG_CONFIG_DIRS`，并且存在两个文件`/etc/xdg/autostart/foo.desktop`和`~/.config/autostart/foo.desktop`，那么只会使用文件`~/.config/autostart/foo.desktop`。`~/.config/autostart/`比`/etc/xdg/autostart/`更重要。

## 应用程序的桌面文件

应用程序的。Desktop文件必须具有“桌面条目规范”中定义的格式。考虑到自动启动目录中的。desktop文件没有显示在菜单中，所有的键都应按照以下例外情况的定义进行解释。

### 隐藏键

当desktop文件的Hidden键设置为true时，必须忽略。desktop文件。如果在多个目录中存在多个同名的。desktop文件，那么只需要考虑最重要的。desktop文件中的隐藏键:如果将其设置为true，那么其他目录中所有同名的。desktop文件也必须被忽略。

### OnlyShowIn和NotShowIn键

OnlyShowIn实体可以包含一个字符串列表，标识必须自动启动此应用程序的桌面环境，所有其他桌面环境都不能自动启动此应用程序。

NotShowIn的实体可以包含一个字符串列表，标识不能自动启动此应用程序的桌面环境，所有其他桌面环境都必须自动启动此应用程序。

在一个.desktop文件中只能出现其中一个键，OnlyShowIn或NotShowIn。

### TryExec键

如果TryExec键的值与已安装的可执行程序不匹配，具有非空TryExec字段的.desktop文件绝对不能被自动启动。TryExec字段的值可以是绝对路径，也可以是没有任何路径组件的可执行文件的名称。如果指定了可执行程序的名称而没有任何路径组件，则搜索`$PATH`环境以查找匹配的可执行程序。

### 实现注意事项

如果应用程序通过在系统范围的自动启动目录中安装`.desktop`文件来自动启动，则个人用户可以通过在其个人自动启动目录中放置同名的`.desktop`文件来禁用该应用程序的自动启动，该目录包含密钥Hidden=true。

## 自动启动应用程序后挂载

当桌面环境挂载新媒介时，该媒介可能包含可以建议启动应用程序的自动启动文件或可以建议打开位于该媒介上的特定文件的自动打开文件。

### 自动启动文件

在挂载新介质时，应该按照优先顺序检查该介质的根目录中是否有以下Autostart文件:.autorun、autorun、autorun.sh，只考虑出现的第一个文件。

桌面环境可能会根据用户、系统管理员或供应商设置的策略完全忽略自动启动文件。

桌面环境必须在自动启动应用程序之前提示用户进行确认。

当检测到自动启动文件并且用户确认其执行时，必须执行自动启动文件，并将当前工作目录(CWD)设置为介质的根目录。

### Autoopen文件

当挂载新媒介时，
a)介质不包含自动启动文件或
b)忽略自动启动文件的策略生效，则应按优先顺序检查介质的根目录中是否有以下自动打开文件:. Autoopen, Autoopen。应该只考虑出现的第一个文件。

桌面环境可能会根据用户、系统管理员或供应商设置的策略完全忽略自动打开文件。

自动打开文件必须包含一个相对路径，该路径指向介质上包含的非可执行文件。如果文件包含换行符或回车符，则必须忽略换行符或回车符本身以及后面的所有字符。

相对路径绝对不能包含指向父目录(../)的路径组件。

相对路径绝对不能指向可执行文件。

桌面环境必须验证相对路径指向的文件实际上位于介质上，考虑到任何符号或其他链接，必须忽略指向介质本身之外的文件位置的任何相对路径。

如果相对路径指向一个可执行文件，那么桌面环境绝对不能执行该文件。

桌面环境必须在打开文件之前提示用户进行确认。

当一个自动打开文件被检测到，并且用户已经确认应该打开自动打开文件中显示的文件，那么自动打开文件中显示的文件必须在用户通常首选的应用程序中打开，除非用户另有指示。