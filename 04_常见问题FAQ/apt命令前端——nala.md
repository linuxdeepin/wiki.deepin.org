---
title: apt命令前端——nala
description: 
published: true
date: 2023-06-29T03:02:29.393Z
tags: 
editor: markdown
dateCreated: 2023-06-29T03:02:29.393Z
---

# apt命令前端——nala
## 1、并行下载

我认为并行下载是选择 Nala 而不是 APT 的最有说服力的理由。

你可能也知道，APT 一次只下载一个包，而 Nala 可能一次下载多个。这大大加快了速度，特别是如果你有很多包要更新。Nala 可以在你的 <span>sources.list</span>文件中为每个唯一镜像下载多达 16 个包。因此，理论上它的下载速度比 APT 快 16 倍。

Nala 限制每个镜像两个线程，以免对单个镜像造成过多负担。为了进一步提高下载速度，Nala 在可用镜像之间交替下载。因此，如果一个镜像因任何原因出现失败，Nala 会继续下一个，直到所有定义的镜像都用完为止。

## 2、选择最快的镜像
在大多数情况下，nala fetch命令的操作方式类似于 netselect 和 netselect-apt。但是 nala fetch会检查你的发行版是 Debian 还是 Ubuntu。然后 Nala 会从各自的主列表中获取所有镜像。完成后，它将执行一个延迟测试，并对每个镜像进行评分。最后，Nala 将选择三个最快的镜像并写入配置文件。（/etc/apt/sources.list.d/nala-sources.list）

sudo nala fetch

## 3、包管理历史
如果你知道 dnf命令，那 nala history工作方式大致相同。它使用唯一 ID编号将每个操作(安装、卸载、更新)保存到 /var/lib/nala/history.json。因此，你可以在任何时候调用 nala history命令来打印执行的每个事务的摘要。

此外，还可以使用 nala history undo ID或 nala history redo ID等命令操作包。

要查看通过 nala命令安装的包的历史事务，请运行 nala history

sudo nala history

## 如何安装 Nala
Ubuntu 和 Debian 用户可以通过输入以下命令来安装 Nala：
```
xxx@xxx:$ echo 'deb
[arch=amd64,arm64,armhf] /volian/ scar main' | sudo tee
/etc/apt/sources.list.d/volian-archive-scar-unstable.list

[sudo] xxx 的密码：

deb [arch=amd64,arm64,armhf] /volian/ scar
main

xxx@xxx:$ wget -qO -
https:///volian/scar.key | sudo tee
/etc/apt/trusted.gpg.d/volian-archive-scar-unstable.gpg > /dev/null

xxx@xxx:$ sudo apt update && sudo
apt install nala
```

## 如何使用Nala
请记住，大多数 apt命令必须以具有 sudo特权的用户身份运行。

##3 获取更新和升级包
安装 nala工具后要做的第一件事是确保更新包数据库的本地副本。如果没有这一步，系统将不知道是否有更新的软件包可用。

那么我们首先使用 nala update命令下载有关可用软件包的最新信息并更新
```
xxx@xxx:$ sudo nala update
```
更新软件包数据库后，你可以使用该 nala install命令安装任何软件包。

来自论坛用户：来自Ubuntu的某位用户的分享～