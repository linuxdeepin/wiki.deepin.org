---
title: Distrobox
description: 使用 Distrobox 在 v23 轻松安装你想要的软件包
published: true
date: 2025-01-21T03:19:19.252Z
tags: 软件
editor: markdown
dateCreated: 2023-09-25T02:04:53.272Z
---

# 使用 Distrobox 在 v23 轻松安装你想要的软件包
> 本文转载自Blumia的论坛文章[使用 Distrobox 在 v23 轻松安装你想要的软件包](https://bbs.deepin.org/post/257787)，在这做个记录方便大家检索。{.is-info}

---
deepin v23 起，deepin 发行版仓库中的所有软件包就均由 deepin 社区中维护软件包的贡献者们一同维护了，即所谓的“根发行版”或者说“根社区”。这对广大普通用户意味着两件事：

直接从 Debian 软件包仓库下载并安装软件包会有更大概率搞坏 deepin 系统
仅为 Debian 或 Ubuntu 等 deb 系发行版适配的第三方应用更大概率无法直接在 deepin 上使用了
其实即便 v23 之前，我们也是从不推荐用户混源的，同是 deb 系的不同发行版的混源仿佛是个非常常见的错误用法。但想必大家都知道，对于用户而言，应用软件生态是一个对使用体验而言至关重要的事。那么，假如我就是想使用这些为 Debian 或者 Ubuntu 适配的软件（无论开源或者闭源），有什么解决办法吗？

## Distrobox 登场
对于 Windows 用户而言，可能很大概率会听说过 WSL，这个可以使自己在 Windows 下运行一个 Linux 子系统，以及运行其中的图形界面应用。另外想必大家肯定多少有听说过 docker 等容器化方案，能让你在自己的系统中运行轻量化的容器环境，在其中运行你所需要的工具。那么能不能在 Linux 中，使用类似 docker 的技术，在 deepin v23 中运行一个发行版容器，并使得你可以使用这个发行版的环境以及其中的应用呢？

显然你没猜错，Distrobox 就是这样的工具！实际上，Distrobox 是一个 Podman/Docker 的 wrapper 脚本，它帮助你能够快速创建并启动一个 Linux 发行版环境的容器，并帮你做好这个容器与系统的集成，使你可以方便的访问主系统以及容器内的文件与网络、使用这个容器内的图形应用等。当然，这些技术细节你都不需要了解，简而言之，你可以使用 distrobox 工具来轻松在 deepin v23 下安装包括 Debian 与 Ubuntu 在内的其它发行版作为“子系统”，并在其环境内安装与使用这些发行版所提供的软件包与工具。

## 那么，如何使用 Distrobox 呢？

Distrobox 在 deepin v23 下的安装
distrobox 软件包本身已经在 deepin v23 仓库中默认提供了，你只需通过下述命令安装即可：

```Shell
sudo apt install distrobox
```
安装后，就可以使用 distrobox 命令了。

假定我们需要安装一个 Ubuntu “子系统”，那么，下面的命令就是创建一个名为 ubuntu 的“子系统”，使用 docker hub 上的 ubuntu 22.04 作为基础镜像：
```
distrobox create ubuntu --image docker.io/library/ubuntu:22.04
```
当然，创建只需要进行一次，创建过程即会自动从网络拉取相关的容器镜像，然后帮你创建好本地对应的容器环境了。创建完毕之后，你就可以通过下面的命令来进入名为`ubuntu`的容器了：
```
distrobox enter ubuntu
```
首次进入这个环境时，会需要一个初始化的过程，帮你安装一些软件包并进行一些配置，以便后续可以更方便的使用，所以会比较慢。初始化完毕后就进入对应的环境了，我们会注意到终端的提示符中，主机名称发生了变化，标志着我们已经进入了这个“子系统”环境。

若要退出这个环境，您只需`exit`即可，然后你可以再次通过`distrobox enter ubuntu`进入这个环境就可以了。

进入 Distrobox 的“子系统”内后，你会发现你仍然可以像使用宿主系统一样轻松访问网络以及自己用户目录下的文件，以及启动“子系统”内的图形应用了。这时候，（假定你在按照上述步骤使用 Ubuntu “子系统”）你可以使用 apt 与 snap 安装 Ubuntu 的软件包，以及安装专为 Ubuntu 所提供的 deb 二进制包，而完全不必担心会影响到你的宿主 deepin 系统的稳定性了。

## 我能创建什么“子系统”
正如之前所提到，Distrobox 不仅仅能运行 Ubuntu 子系统，你还可以通过它来安装包括 Debian、Arch Linux、Fedora、openSUSE 等“子系统”！这样，对于喜欢追新的用户也可以安装 Arch Linux 或 Fedora 并使用其中的软件包。希望体验其它发行版环境也可以装来试试。关于 distrobox 所支持的完整的兼容列表，可以参见官方给出的这个列表：

[https://github.com/89luca89/distrobox/blob/main/docs/compatibility.md#containers-distros](https://github.com/89luca89/distrobox/blob/main/docs/compatibility.md#containers-distros)

事实上，deepin v23 所提供的 deepin v20 兼容方案（DCM）正是基于 Distrobox 的。如果你不是 deepin 用户而希望运行一个 deepin 的“子系统”来体验的话，假定你在v23上想体验 deepin v20，则可以通过：
```
distrobox create deepin-v20 --image docker.io/linuxdeepin/apricot
```
 来创建对应的“子系统”了。
## 最后
于是这次关于 distrobox 的介绍就到此结束了。如果你对其有所了解后想要更深入的使用它，例如如何为“子系统”里的应用创建快捷方式等进阶用法，可以阅读 distrobox 的官方文档了解更多高级用法。

这篇文章希望能向大家介绍 distrobox 的用法，因而避免因 v23 源内“缺包”导致的盲目去其它发行版仓库内下包以及混源的不恰当做法。
当然，如果 distrobox 无法解决你的问题（例如你需要安装某个驱动包，无法装到容器内使用），那么请你及时在论坛、 Matrix 或 GitHub 进行反馈，以便贡献者们可以进行打包。另外，如果你希望参与 deepin 发行版软件包的维护的话，也欢迎加入我们的相关 SIG，并进行软件包的递交与维护哦~

