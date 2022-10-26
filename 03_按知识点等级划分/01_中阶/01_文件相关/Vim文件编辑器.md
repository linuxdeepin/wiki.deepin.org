---
title: Vim文件编辑器
description: 
published: true
date: 2022-10-26T06:43:09.860Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:44:15.439Z
---

# 简单介绍

vim 是 vi 的一个升级版本，gvim 算是 vim 的一个 GUI 版本，或者说是 vim 的一个升级版本。
vim 兼容 vi 的所有指令，并且有更多的优势，同时，vim 还拥有众多的插件，与 emacs 一样，虽然本身只是一个编辑器，但是通过插件的拓展可以实现许多高级的应用，如不同语言的语法高亮、语法补全甚至是游戏等，当然，在这方面 vim 或许还不能如 emacs，毕竟 emacs 号称是一个操作系统。

# 简单操作

进入 vim 后（正常模式），按下 i 键进行打几个字。
然后按下 Esc 键退出编辑（插入）模式，回到正常模式。
在正常模式下，h 键为光标左移，l 键为光标右移，j 键为光标下移一行，k 键为光标上移一行。
再次按下 i 键，进入编辑模式。
更多应用及命令请自行研究。
在正常模式下，输入 " :help" 打开帮助文档。

# 简单的配置

在 /home/user/ 下新建一个 .vimrc 文件并进入编辑，这里是一个简单的配置：  

```
" 剪贴板
set clipboard=unnamed

" 显示行号
set number

" 开启语法高亮
syntax on

" 显示标尺
set ruler

" 打开折叠
set foldenable

" 打开自动缩进
set autoindent

" C 语言缩进
set cindent

" 缩进为 4 个空格
set shiftwidth=4

" tab 键相当于 4 个空格
set tabstop=4
```

# 安装语法补全插件 YouCompleteMe

除了通过 Vundle 安装语法补全 YouCompleteMe，也可以自己手动编译安装，这里有另外一种更加简单的方法。

```
# 试试这个命令，看是否已经安装 vim-addons
$ vim-addons
# 如果没有安装 vim-addons，则需安装 vim-addon-manager
$ sudo apt-get install vim-addon-manager
# 开始安装 YouCompleteMe
$ sudo apt-get install vim-youcompleteme
# 将 YCM 加入 addons 管理器中
$ vim-addons install youcompleteme
```

结束，新建一个 main.c 并进行编辑试试。  
需要说明的是，安装了 YCM 后 vim 的开启速度会慢那么 1s。

# 安装gvim

```
sudo apt-get install vim-gtk3
```

# 将gvim添加到启动器

创建 `/usr/share/applications/gvim.desktop`文件并加入以下内容：  

```
[Desktop Entry]
Name=Gvim
Exec=gvim
Icon=vim-gtk
Terminal=false
X-MultipleArgs=false
Type=Application
Encoding=UTF-8
Categories=Application;Utility;Dictionary;
StartupNotify=false
```

创建desktop文件参考wiki：[https://wiki.deepin.org/wiki/Desktop_Entry_文件](https://wiki.deepin.org/wiki/Desktop_Entry_文件)

# 安装vim中文帮助文档

从这里下载压缩包 vimcdoc-2.1.0.tar.gz  [https://sourceforge.net/projects/vimcdoc/](https://sourceforge.net/projects/vimcdoc/)
解压后

```
mkdir ~/.vim     
cd vimcdoc-2.1.0
./vimcdoc.sh -i
```

这样就安装好了，再进入vim后，`:help`出来应该就是中文的了。
