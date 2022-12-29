---
title: Matrix
description: 介绍 Matrix 即时通讯平台的使用方式
published: true
date: 2022-12-29T07:03:43.144Z
tags: 
editor: markdown
dateCreated: 2022-12-29T06:37:05.800Z
---

# "Matrix 是什么?"

Matrix 是一个开源、开放、轻量级、去中心化的及时聊天通讯协议，它是包括 deepin、Mozilla、Fedora、KDE、Archlinux、debian 等开源社区均在广泛使用的及时聊天协议。

# 开始使用 Matrix

任何社区均可以搭建自己的 Matrix 实例，您可以在任何社区的 Matrix 实例中注册帐号，并登录其实例，并且您可以使用同一个帐号即可加入不同实例之中的聊天室。

## 帐号的注册

如果您还没有在任何实例注册过 Matrix 帐号，您可以考虑在下面列表中的任意一个实例中注册帐号：

- Mozilla（推荐）：https://chat.mozilla.org/#/welcome
- Element (即 matrix.org 实例)：https://app.element.io/

注册后即可登录其对应的网页版在线聊天了。

## 使用客户端登录

### Element 客户端

Element 是 Matrix 官方性质的，基于 Web 技术的 Matrix 客户端实现，除了网页环境外，也有桌面客户端可用。可以在你所使用的发行版的应用商店或包管理工具中搜索 `element-desktop` 或近似名称来检索和安装此客户端，然后即可运行并登录你的帐号。

> **注意**：
> 若您所使用发行版未做任何额外配置，则 Element 客户端默认会尝试连接 `matrix.org` 实例。若您所处的环境无法直接连接 matrix.org，则可能在打开 Element 后看到 `Your Element is misconfigured` 的提示。此时，可通过在 ~/.config/Element/config.json 创建内容如下的文件，然后尝试重新启动 Element 客户端即可：
> ```json
> {
>     "default_server_name": "mozilla.modular.im"
> }
> ```
> ~~deepin 已为软件仓库中的 Element 提供了近似的配置，故不需要进行额外操作。~~（还没有）

## 探索聊天室

社区通常通过“Space（空间）”索引自己社区所包含的聊天室列表，您可以加入对应的 Space 来检索感兴趣的聊天室，也可以直接检索并加入感兴趣的聊天室。

下面是一些推荐的空间与聊天室，可供参考：

- [deepin 空间](https://matrix.to/#/#deepin-space:matrix.org)：`#deepin-space:matrix.org`
  - [deepin 开发者频道](https://matrix.to/#/#deepin-community:matrix.org)：`#deepin-community:matrix.org`
  - [DDE 移植小组](https://matrix.to/#/#dde-port:matrix.org)：`#dde-port:matrix.org`
  - [deepin 文档小组](https://matrix.to/#/#deepin_doc_doc_go:matrix.org)：`#deepin_doc_doc_go:matrix.org`
  - [deepin Qt 维护小组](https://matrix.to/#/#_oftc_#deepin-qt:matrix.org)：`#_oftc_#deepin-qt:matrix.org`
  - 以及其它 deepin 相关的聊天室...
- [KDE 社区空间](https://matrix.to/#/#kde-community:kde.org)：`#kde-community:kde.org`