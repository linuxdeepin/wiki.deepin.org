---
title: Fcitx5
description: 以 GPL 方式发布的输入法平台，可以通过安装引擎支持多种输入法，支持简入繁出
published: true
date: 2022-11-11T01:23:20.978Z
tags: 输入法
editor: markdown
dateCreated: 2022-06-16T02:34:38.778Z
---

Fcitx (Flexible Input Method Framework) ──即小企鹅输入法，它是一个以 GPL 方式发布的[输入法](http://old.deepin.wiki/index.php?title=输入法)平台，可以通过安装引擎支持多种输入法，支持简入繁出，是在 [Linux](http://old.deepin.wiki/index.php?title=Linux) 操作系统中常用的中文输入法。它的优点是，短小精悍、跟程序的兼容性比较好。下一代版本为 [Fcitx5](http://old.deepin.wiki/index.php?title=Fcitx5)。

Fcitx5 是继 [Fcitx](http://old.deepin.wiki/index.php?title=Fcitx) 后的新一代输入法框架。自 20.2.3 后被集成到仓库中，系统预装版本仍为 fcitx4。

## 安装

讯飞输入法、搜狗输入法等是在fcitx4框架的基础上，因此此类输入法将无法在 fcitx5 下使用。
要在 deepin 上安装 fcitx5，首先应当先卸载现有的 fcitx4 输入法。

```bash
sudo apt purge *fcitx*
```

然后安装 fcitx5 框架

```bash
sudo apt install fcitx5
```

安装成功后要启用 fcitx5 还需要配置环境变量，编辑 /etc/environment

```
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
SDL_IM_MODULE=fcitx
```

写入后注销并重新登入即可。

## 输入法

### 中文输入法

安装中文输入法最简单的命令是：

```bash
sudo apt install fcitx5-chinese-addons
```

这会同时安装拼音、五笔、双拼等所有的中文输入法。如果您不希望在系统中安装所有，可以选择单独安装以下输入法。

- fcitx5-pinyin：拼音和双拼
- fcitx5-table：自然码、五笔、晚风、双拼、二笔、电报码、仓颉等

安装完成后需要在托盘右键，重新启动输入法框架。

## 个性化

## 提示与技巧

### 添加词库

在 fcitx5-pinyin 输入法中添加词库，可以将词库文件放置在 `~/.local/share/fcitx5/pinyin/dictionaries/` 中。如维基百科词库 `fcitx5-pinyin-zhwiki`：

> Download latest version of "zhwiki.dict" from: https://github.com/felixonmars/fcitx5-pinyin-zhwiki/releases
>
> Copy into ~/.local/share/fcitx5/pinyin/dictionaries/ (create the folder if it does not exist)

参见：https://github.com/felixonmars/fcitx5-pinyin-zhwiki

## 修改拼音输入法的中括号为简体中文样式

将 /usr/share/fcitx5/punctuation/punc.mb.zh_CN 的 `[ ·` 和 `] 「」` 改成 `[ [` 和 `] ]` ，重启 fcitx5 即可。

## 开启没有拼音的单行模式

勾选上 fcitx5 设置中的 在程序中显示预编辑文本 和 在预编辑中显示完整的拼音。