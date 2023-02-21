---
title: Matrix
description: 介绍 Matrix 即时通讯平台的使用方式
published: true
date: 2023-01-10T09:28:51.489Z
tags: 
editor: markdown
dateCreated: 2022-12-29T06:37:05.800Z
---

# "Matrix 是什么?"

Matrix 是一个开源、开放、轻量级、去中心化的即时聊天通讯协议，它是包括 deepin、Mozilla、Fedora、KDE、Archlinux、debian 等开源社区均在广泛使用的即时聊天协议。

# 开始使用 Matrix

任何社区均可以搭建自己的 Matrix 实例，您可以在任何社区的 Matrix 实例中注册帐号，并登录其实例，并且您可以使用同一个帐号即可加入不同实例之中的聊天室。

## 帐号的注册

如果您还没有在任何实例注册过 Matrix 帐号，您可以考虑在下面列表中的任意一个实例中注册帐号：

- Deepin (推荐)：[账号申请](https://cooperation.uniontech.com/public/form/41b23e5518074c1b958d11e5c8d2d54e) | [登录地址](https://chat.getdeepin.org)
- Mozilla（推荐）：https://chat.mozilla.org/#/welcome
- Fedora Project：https://chat.fedoraproject.org/
- OpenSuSE：https://chat.opensuse.org/
- Open Desktop：https://chat.opendesktop.org/
- Element (即 matrix.org 实例)：https://app.element.io/

注册后即可登录其对应的网页版在线聊天了。

## 使用客户端登录

您可以使用任何支持 Matrix 协议的客户端来登录并连接到 Matrix 网络。

### Element 客户端

Element 是 Matrix 官方性质的，基于 Web 技术的 Matrix 客户端实现，除了网页环境外，也有桌面客户端可用。可以在你所使用的发行版的应用商店或包管理工具中搜索 `element-desktop` 或近似名称来检索和安装此客户端，然后即可运行并登录你的帐号。

> **注意**：
> 若您所使用发行版未做任何额外配置，则 Element 客户端默认会尝试连接 `matrix.org` 实例。若您所处的环境无法直接连接 matrix.org，则可能在打开 Element 后看到 `Your Element is misconfigured` 的提示。此时，可通过在 ~/.config/Element/config.json 创建内容如下的文件，然后尝试重新启动 Element 客户端即可：
> ```json
> {
>     "default_server_name": "deepin.org"
> }
> ```
> ~~deepin 已为软件仓库中的 Element 提供了近似的配置，故不需要进行额外操作。~~（还没有）

值得注意的是，在登录前，您可能需要编辑 Home Server 到您所注册时所使用的实例所对应的位置。例如假设您使用了 Mozilla 的实例，则需要编辑修改为 `mozilla.modular.im`。

### Nheko Reborn

Nheko Reborn 是一个基于 Qt/QML 技术的 Matrix 客户端，包含了包括 E2EE 在内的广泛的 Matrix 已有特性的支持，且支持主流平台。

### NeoChat

NeoChat 是 KDE 社区提供的，基于 Qt/QML 技术的 Matrix 客户端，包含了社区常用的大部分 Matrix 特性，以及实验性的 E2EE 支持。

### 其它客户端

你可以访问 [Clients](https://matrix.org/clients/) 页面了解已知的 Matrix 客户端列表。如果你推荐使用某个客户端，也可以编辑此 Wiki 页面，将客户端的名字加到这个列表中来，来进行详细的介绍。

# 探索聊天室

## 不同社区的空间与聊天室

社区通常通过“Space（空间）”索引自己社区所包含的聊天室列表，您可以加入对应的 Space 来检索感兴趣的聊天室，也可以直接检索并加入感兴趣的聊天室。

下面是一些推荐的空间与聊天室，可供参考：

- [deepin 社区空间](https://matrix.to/#/#deepin-space:matrix.org)：`#deepin-space:matrix.org`
  - [deepin 开发者频道](https://matrix.to/#/#deepin-community:matrix.org)：`#deepin-community:matrix.org`
  - [DDE 移植小组](https://matrix.to/#/#dde-port:matrix.org)：`#dde-port:matrix.org`
  - [deepin 文档小组](https://matrix.to/#/#deepin_doc_doc_go:matrix.org)：`#deepin_doc_doc_go:matrix.org`
  - [deepin Qt 维护小组](https://matrix.to/#/#_oftc_#deepin-qt:matrix.org)：`#_oftc_#deepin-qt:matrix.org`
  - 以及其它 deepin 相关的聊天室...
- [Fedora 社区空间](https://matrix.to/#/#fedora-space:fedoraproject.org)：`#fedora-space:fedoraproject.org`
- [KDE 社区空间](https://matrix.to/#/#kde-community:kde.org)：`#kde-community:kde.org`

## 桥接聊天室

Matrix 原生支持将已有的 Matrix 聊天室桥接到 IRC/Telegram/Discord 等的群聊之中，以供 Matrix 用户可以直接和现有平台的用户进行沟通。您可以使用公开的桥接服务，也可以自己编写桥接服务来桥接已有的聊天室。

### IRC 桥

主流的 IRC 聊天平台，包括 libera.chat 与 OFTC 等，均提供了现有的桥接服务可用，可参见 [这个页面](https://matrix-org.github.io/matrix-appservice-irc/latest/bridged_networks.html) 了解详细的说明。

### Telegram 与 Discord 桥

[t2bot.io](https://t2bot.io/) 提供了将 Matrix 聊天室桥接到 Telegram 群聊与 Discord 群聊的公开服务，可前往其网站进行了解。

手动的桥接操作通常需要聊天室的管理员进行操作。