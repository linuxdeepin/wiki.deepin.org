---
title: XDG桌面菜单项
description: 
published: true
date: 2022-11-07T07:49:06.536Z
tags: freedesktop
editor: markdown
dateCreated: 2022-11-07T07:49:06.536Z
---

原文链接：https://portland.freedesktop.org/doc/xdg-desktop-menu.html
该功能对于dde-desktop不可用，dde-desktop使用自己的菜单项，但是其可以用于其他用途，比如关联自定义文件类型的默认程序
名称

`xdg-desktop-menu` - 用于安装（卸载）桌面菜单项的命令行工具
概要
```
xdg-desktop-menu install[ --noupdate] [ --novendor] [ ] --mode modedirectory-file(s) desktop-file(s)
xdg-desktop-menu uninstall [--noupdate] [--mode mode] directory-file(s) desktop-file(s)
xdg-desktop-menu forceupdate [--mode mode]
xdg-desktop-menu { --help | --manual | --version }
```
描述
`xdg-desktop-menu`程序可用于将新菜单项安装到桌面的应用程序菜单中。
    该应用程序菜单根据XDG桌面菜单规范工作，该规范位于：http://www.freedesktop.org/wiki/Specifications/menu-spec

指令

`install`
在桌面菜单系统的子菜单中安装一个或多个应用程序。
    desktop-file：桌面文件代表菜单中的单个菜单项。桌面文件由freedesktop.org桌面条目规范定义。* .desktop文件的最重要方面总结如下:
        菜单项可以通过两种不同的方式添加到菜单系统中。它们可以基于一个或多个类别关键字添加到菜单系统中的预定义子菜单，也可以添加到新的子菜单。
        要将菜单项添加到预定义的子菜单中，表示菜单项的桌面文件必须具有列出一个或多个关键字的Categories=项。菜单项将基于所包含的关键字包含在适当的子菜单中。
        要将菜单项添加到新的子菜单，必须在桌面文件之前加上描述该子菜单的目录文件。如果指定了多个桌面文件，则所有条目都将添加到同一菜单中。如果将条目安装到使用xdg-desktop-menu的上一次调用创建的菜单中，则除了已存在的条目外，还将安装新条目。
        directory-file：表示的 .directory文件 directory-file代表一个子菜单。目录文件提供了子菜单的名称和图标。目录文件的名称用于标识子菜单。
        如果提供了多个目录文件，则每个文件将代表该菜单之前的菜单内的一个子菜单，从而创建嵌套菜单层次结构（子子菜单）。菜单项本身将添加到最后一个子菜单。
        目录文件遵循freedesktop.org桌面条目规范定义的语法。

`uninstall`
从xdg-desktop-menu install先前安装的桌面菜单系统中删除应用程序或子菜单。仅当子菜单不再包含任何菜单条目时，才删除子菜单和关联的目录文件。

`forceupdate`
强制更新菜单系统。仅当最后一次调用xdg-desktop-menu包含该--noupdate选项时，此命令才有用。

参数：

`--noupdate`
推迟更新菜单系统。如果按顺序对菜单系统进行了多次更新，则此标志可用于指示将进行其他更改，并且不必立即更新菜单系统。

`--novendor`
通常，xdg-desktop-menu检查以确保要安装的任何 .directory和 .desktop文件都具有供应商前缀。此选项可用于禁用该检查。
供应商前缀由字母字符（[a-zA-Z]）组成，并以短划线（“-”）终止。鼓励公司和组织使用带有商标的单词或短语（最好是组织名称）作为供应商前缀。供应商前缀的目的是防止名称冲突。

`--mode mode`
mode可以是user或system。在用户模式下，仅为当前用户安装（卸载）文件。在系统模式下，将为系统上的所有用户安装（卸载）文件。通常，只有root用户才能在系统模式下安装。
默认是由root调用时使用系统模式，而由非root用户调用时使用用户模式。

`--help`
显示命令简介。

`--manual`
显示此手册页。

`--version`
显示xdg-utils版本信息。

桌面文件：
应用程序菜单中的应用程序项由 .desktop文件表示。 .desktop文件由 [Desktop Entry]标头和后面的几行Key=Value组成。

*.desktop文件可以使用几种不同的语言提供应用程序的名称和描述。这是通过在后面的方括号中添加LC_MESSAGES使用的语言代码来完成的 Key。这样一来，您可以Key根据当前选择的语言为同一名称指定不同的值。

经常使用以下键：
`Type=Application`
这是一个必填字段，指示 .desktop文件描述了应用程序启动器。
`Name=Application Name`
应用程序的名称。例如Mozilla
`GenericName =Generic Name`
应用程序的一般描述。例如网页浏览器
`Comment=Comment`
可选字段，用于为应用程序指定工具提示。例如，访问Internet上的网站
`Icon=Icon File`
用于该应用程序的图标。这可以是图像文件的绝对路径，也可以是图标名称。如果提供了图标名称，则会在用户的当前图标主题中按名称查找图像。在xdg-icon-resource命令可用于图像文件安装到图标主题。使用图标名称代替绝对路径的优点是，使用图标名称，可以以几种不同的大小以及几种不同的主题样式提供应用程序图标。
`Exec=Command Line`
用于启动应用程序的命令行。如果应用程序可以打开文件，则应指定％f占位符。将文件放在应用程序启动器上时，％f被替换为所删除文件的文件路径。如果可以在命令行上指定多个文件，则应使用％F占位符代替％f。如果应用程序除了本地文件之外还能够打开URL，则可以使用％u或％U代替％f或％F。
`Categories=Categories`
用分号分隔的类别列表。类别是描述和分类应用程序的关键字。默认情况下，应用程序是根据类别在应用程序菜单中组织的。将菜单条目明确分配给新的子菜单后，无需列出任何类别。
使用类别时，建议包括以下类别之一：AudioVideo, Development, Education, Game, Graphics, Network, Office, Settings, System, Utility。
有关其他类别的信息，请参见XDG桌面菜单规范的附录A：http://standards.freedesktop.org/menu-spec/menu-spec-1.0.html#category-registry
`MimeType =Mimetypes`
由分号分隔的模仿类型列表。该字段用于指示应用程序能够打开哪些文件类型。
有关 .desktop文件格式的完整概述，请访问http://www.freedesktop.org/wiki/Specifications/desktop-entry-spec

目录文件
应用程序菜单中子菜单的外观由 .directory文件提供。特别是它提供了子菜单的标题和可能的图标。 .directory文件包含 [Desktop Entry]标头，后跟几行Key=Value。

* .directory文件可以使用几种不同的语言为子菜单提供标题（名称）。这是通过在后面的方括号中添加LC_MESSAGES使用的语言代码来完成的Key。这样一来，您可以Key根据当前选择的语言为同一名称指定不同的值。

以下键与子菜单有关：
`Type=Directory`
这是一个必填字段，指示* .directory文件描述了一个子菜单。
`Name=Menu Name`
子菜单的标题。例如Mozilla
`Comment=Comment`
可选字段，用于为子菜单指定工具提示。
`Icon=Icon File`
子菜单使用的图标。这可以是图像文件的绝对路径，也可以是图标名称。如果提供了图标名称，则会在用户的当前图标主题中按名称查找图像。在XDG-图标资源 命令可用于图像文件安装到图标主题。使用图标名称而不是绝对路径的优点是，使用图标名称，子菜单图标可以提供几种不同的大小以及几种不同的主题样式。

环境变量

xdg-desktop-menu遵循以下环境变量：
XDG_UTILS_DEBUG_LEVEL
将此环境变量设置为非零数值将使xdg-desktop-menu在stderr上执行更详细的报告。设置较高的值会增加详细程度。
XDG_UTILS_INSTALL_MODE
用户或管理员可以使用此环境变量来覆盖安装模式。有效值为用户和系统。
退出码

退出代码0表示成功，而非零退出代码表示失败。可以返回以下故障代码：
1
命令行语法错误。
2
在命令行上传递的某个文件不存在。
3
找不到所需的工具。
4
动作失败。
5
没有权限读取在命令行上传递的某个文件。
例子

ShinyThings Inc.公司已经开发了一个名为“ WebMirror”的应用程序，并希望将其添加到应用程序菜单中。该公司将使用“ shinythings”作为其供应商ID。为了将应用程序添加到菜单中，需要有一个带有适当类别类别的.desktop文件：

shinethings-webmirror.desktop：
```
  [Desktop Entry]
  Encoding=UTF-8
  Type=Application

  Exec=webmirror
  Icon=webmirror

  Name=WebMirror
  Name[nl]=WebSpiegel

  Categories=NetworkWebDevelopment；
```
现在，可以使用xdg-desktop-menu工具将Shinythings-webmirror.desktop文件添加到桌面应用程序菜单中：

xdg-desktop-menu安装./shinythings-webmirror.desktop
请注意，在本示例中，菜单项有两种语言，英语和荷兰语。荷兰语的语言代码是nl。

在下一个示例中，公司ShinyThings Inc.将向桌面应用程序菜单添加其自己的子菜单，该子菜单由“ WebMirror”菜单项和“ WebMirror Admin Tool”菜单项组成。

首先，公司需要创建两个描述菜单项的.desktop文件。由于要将项目添加到新的子菜单中，因此不必包括Categories=行：

shinethings-webmirror.desktop：
```
  [Desktop Entry]
  Encoding=UTF-8
  Type=Application

  Exec=webmirror
  Icon=shinythings-webmirror

  Name=WebMirror
  Name[nl]=WebSpiegel
```
shinethings-webmirror-admin.desktop：
```
  [Desktop Entry]
  Encoding=UTF -8
  Type=Application

  Exec=webmirror-admintool
  Icon=shinythings-webmirror-admintool

  Name=webmirror-admintool
  Name[nl]=shinythings-webmirror-admintool
```
另外，需要创建一个.directory文件来为子菜单本身提供标题和图标：

shinethings-webmirror.directory：
```
  [Desktop Entry]
  Encoding=UTF-8

  Icon=shinythings-webmirror-menu

  Name=WebMirror
  Name[nl]=WebSpiegel
```
这些文件现在可以通过以下方式安装：
```
xdg-desktop-menu install ./shinythings-webmirror.directory \
      ./shinythings-webmirror.desktop ./shinythings-webmirror-admin.desktop
```
菜单项也可以一一安装：
```
xdg-desktop-menu install --noupdate ./shinythings-webmirror.directory \
      ./shinythings-webmirror.desktop
xdg-desktop-menu install --noupdate ./shinythings-webmirror.directory \
      ./shinythings-webmirror-admin.desktop
xdg-desktop-menu forceupdate
```
尽管结果相同，但是同时安装所有文件的效率略高。

* .desktop和* .directory文件引用的图标名称分别为webmirror，webmirror-admin和webmirror-menu。在此示例中，图标以两种不同的尺寸安装，一次的尺寸为22x22像素，一次的尺寸为64x64像素：
```
xdg-icon-resource install --size 22 ./wmicon-22.png shinythings-webmirror
xdg-icon-resource install --size 22 ./wmicon-menu-22.png shinythings-webmirror-menu
xdg-icon-resource install --size 22 ./wmicon-admin-22.png shinythings-webmirror-admin
xdg-icon-resource install --size 64 ./wmicon-64.png shinythings-webmirror
xdg-icon-resource install --size 64 ./wmicon-menu-64.png shinythings-webmirror-menu
xdg-icon-resource install --size 64 ./wmicon-admin-64.png shinythings-webmirror-admin