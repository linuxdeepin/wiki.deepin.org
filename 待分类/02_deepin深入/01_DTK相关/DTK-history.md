---
title: DTK 的历史起源、发展，和简单入门。
description: DTK 的历史起源、发展，和简单入门。
published: true
date: 2023-02-15T17:29:19.216Z
tags: DTK
editor: markdown
dateCreated: 2023-02-15T17:29:19.216Z
---

# DTK的历史起源、发展，和简单入门


**简 述：**　 **DTK**（`deepin tool kit` 后改为 `Development ToolKit`）全称是 **深度工具套件**；是基于 `Qt5` 开发一整套界面美观且实用的 `UI` 图形库。

本篇主要尽可能多的讲解一下 `dtk` 的起源于历史，以及项目壮大后的拆分、现在 `dtk` 项目的组成和基本含义；以及 `dtk` 的文档从无到有，再到现在的极大丰富历史；最后就是 `dtk` 的如何快速上手部分。


网上关于它的教程比较少，关于历史的更是基本就没有，在此补一笔。

> 本文初发于 “[**偕臧的小站**](https://ifmet.cn/)“，同步转载于此，有部分修改。

### [](#一点碎碎念-： "一点碎碎念~：")一点碎碎念~：

> 工欲善其事，必先利其器。再知道一点它的演变历史~


本来只是想单写一篇 dtk 的其实起源的，但随着本文知识的丰富、确保时间线的基本准确，请教很多的前辈和大佬们，感谢他们。
请教了 dtk、deepin、develop team 的历史和原源过程中，体会到事物往往不是单一的，且与彼此之间有着千丝万缕联系而又互相独立，各有特色。**本篇的侧重点是 dtk 项目的历史起源、发展过程与现状、现在的理解和如何快速上手；最后附上部分 dtk 相关的资料文档。** **而关于 deepin && dde && team 的介绍，则单独成立一篇新的[文章](https://xmuli.blog.csdn.net/article/details/106195055)。**

### [](#背景交代： "背景交代：")背景交代：

说来惭愧，初接触 deepin 是由 v15.11 才开始的，一开始埋头着开发 dtk 有许久的一些时间，但对其历史出身来历一直比较模糊，于是乎收集和整理一下现存的所有 dtk 的项目、手册资料，与库的维护者和最初的库的开发者，以及公司有着较深资历前辈们交流，外加上个人和小伙伴们，也都有参与过该库的维护一段时间后，才得以落笔，梳理成此篇文章（欢迎交流与指正）。

### [](#dtk-名称的理解（易混淆）： "dtk 名称的理解（易混淆）：")dtk 名称的理解（易混淆）：

- 其中有几个容易混淆的名词，`dtk`， `dtkwidget`， `dtk` 项目。

**下面仅为个人理解，按照当前 2020 上半年的阶段的项目实际情况理解：**

**dtk 含义理解：** 是一个泛指，需要灵活的在上下文的语义中理解其含义：

1.  **dtk 项目(库) 的集合。** （是一堆仓库的集合 = dtkwidget + dtkgui + dtkcore + qt5integration…）
2.  偶也指 dtkwidget 这个仓库项目（这个仓库的名称就叫 [dtkwidget](https://github.com/linuxdeepin/dtkwidget)）
3.  deepin tool kit 是一个深度工具构建集合。
4.  是能在 deepin(uos) 系统开发应用软件，使用非 Qt 原生的样式控件的集合的开发环境
5.  dtk 也作为构建 deepin 全家桶的基石

DTK 目前分为三个模块，dtkcore、dtkgui，dtkwidget，主要功能如下：

| 项目      | 功能描述                                                                               |
| --------- | -------------------------------------------------------------------------------------- |
| dtkcore   | 提供应用程序开发中的工具类，如程序日志、文件系统监控、格式转换等工具类                 |
| dtkgui    | 包含了开发图形用户界面应用程序所需的功能。主要是控制窗口主题这种外观性，调色板等信息。 |
| dtkwidget | 提供各种 dtk 基础控件，方便开发统一风格的应用。                                        |

### [](#DTK-的历史起源： "DTK 的历史起源：")DTK 的历史起源：

#### [](#dtk-项目的历史演变（含名称）： "dtk 项目的历史演变（含名称）：")dtk 项目的历史演变（含名称）：

![](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/20200518_111136.png)

- 在 2015 年，开发 deepin v15 版本的时候，因为开发控制中心（基于 Qt Widgets），开发的过程中抽象出了 dui 控件库（从控制中心开发起三个月），只有一些通用性的控件，仓库名称为 `dui` 。
- 16 年初，随后开发其他项目，dui 中加的东西越来越多，也不仅限于 ui 方面的控件，之后就改名为了 `deepin-tool-kit` 项目。
- 17 年底，随后成为了 DDE 桌面环境的底层开发库，封装了桌面组件和上层应用的通用型窗口、控件、工具类，随着项目越来越大，编译也越来越慢，于是拆分为了 `dtkcore`、`dtkwidget`、`dtkwm`（仅三个仓库），旧的 deepin-tool-kit 仓库地址处于废弃状态。
- 19 年新增加了 dtkgui 模块，废弃了 dtkwm（不可跨平台，强依赖 X11，因此废弃），此后分为 dtkcore、dtkgui、dtkwidget，角色分别对应 Qt core、gui、widgets 模块。
- 20 年之后，按照如今的理解为，`dtk` = `dtkwidget` + `dtkgui` + `qt5integration` + `dtkcore` + … 等多个项目的总称呼

**由于 `qt5integratioin` 项目一直是独立的，本不算 `dtk` 项目里的一员，不过实际上是和 `dtk` 配套的，故本文算到 `dtk` 框架中。**

#### [](#样式变化： "样式变化：")样式变化：

其中随着时间的变化，界面的 UI 样式也发生了比较大的变化，当时 v15 版本的控制中心，还在屏幕右侧，不透明偏向黑色，到后来的成为白色偏透明色，在到现在的一个单独的控制中心（下图依次为 deepin v15 早期版本，和 uos v20 的控制中心）。

而 dtk 是属于绘画自定义皮肤控件的基础核心，然后在它的基础上面做了一次封装， 封装了一系列 Dxxxx 开头的控件（如 DPushButton 等）， 然后再由应用开发的同学们，开发出一款款应用，全部使用 dtk 的控件。那样开发出来的界面，就和系统的风格保持了一致性；而对于 win 系列的软件，也可以使用 wine（deepin-win） 来在 此 linux 上面运行；而对于第三方的应用软件，使用 chameleon style 启动，来保证和系统的风格一致。

下面放图、使用 dtk 库 开发出来的控件或者应用,enen… dtk 的样式就这么好看，这么的棒，这里要多感谢设计师们。**ps：** 设计师们简直就是程序开发之友，尤其是使用绘画控件的时候，可以少修改 50%+ 的代码或者 bug，且他们也非常好沟通~

- deepin v15 早期不透明黑色效果：

![](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/20200515161731.png)

- uos/deepin v20 早期透明磨砂效果：

![](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/20200515_175353.png)

#### [](#0-gt-1-的诞生-dui： "0 -> 1 的诞生 dui：")0 -> 1 的诞生 dui：

约 2015 年，**起初是创建 dui 项目的，后来才改名为，[deepin-tool-kit](https://github.com/linuxdeepin/deepin-tool-kit) ；再后来拆分为为多个库 dtkwidget + qt5integration + 其他；**

早先，且所有的 dtk + deepin 的项目，都是在 github 作为唯一的大本营;由于 0->1 时代，主要是开发构建 dtk 的体系架构，基本没有什么文档的书写 ✍️（一个伏笔）

后来有听到前辈们说，那个时候是的氛围轻松而又欢乐，更多的是一种平等沟通的合作，大家都是开源爱好的志愿者，一起做着自己喜欢的事情。羡煞我也。 向往与憧憬那个年代，以及更早的几年

#### [](#deepin-tool-kit-是石器时代： "deepin-tool-kit 是石器时代：")deepin-tool-kit 是石器时代：

deepin-tool-kit 最开始，是既没有文档，且 api 接口不稳定，容易变化，且设计图的 UI 样式也变化较快，导致入门、开发、维护该 dtk 库都很是困难~。**基本是内部开发者靠小伙伴讲解，外部开发靠自行阅读源码** 

#### [](#壮大后拆分-dtk-库： "壮大后拆分 dtk 库：")壮大后拆分 dtk 库：

前辈们的开发者一起编写最初的 `dtk` 库，随着近好几年的变化和发展，后期一定规模了，深思熟虑后，为更好发展，就决定将对 `deepin-tool-kit` 仓库进行拆分（2017 年左右）： 详见 [issues #8](https://github.com/linuxdeepin/deepin-tool-kit/issues/8)

- **dtkcore：** 由 dtkbase/dtkutil/dtksettings 合并进入
- **dtkwidget：** 由 dtkwidget/dtksettings 构成
- **dtkwm：** 处理窗口识别库/xcb 功能， 供截图、录屏、系统监视器使用
- **qtwebkit：** 封装 chroium，提高 CEF/WebEngine 兼容接口

昂~ ，为了代码的健壮性发展，与方便后期的可维护性

#### [](#dtk-的口口相传时代： "dtk 的口口相传时代：")dtk 的口口相传时代：

随着上一个时代已经过去了，dtk 的主体框架和 api 都趋向于稳定；且有了部分中文文档，（是中文文档！！！ 鸡冻 ٩(๑>◡<๑)۶！！！）；此时也基本对内部和外部的研发都已经友好了很多。

此阶段应该是 deepin 15.11 （恩，至今都是一个经典的版本，后来发现该发版本打磨和维护了近 5 年，很棒的一个版本）已经发布了出来了，开始研发 v20 阶段了。

不过此阶段，新的库的维护者学习 dtk 的方法，依旧是口口相传，手把手的教你一行一行的写代码（像一个大哥哥一样）；毕竟使用的最新的接口，还没来得及更新到文档之中，且容易有变化，旧的接口就稳定得多。**此阶段 dtk 的研发就是内部靠口口相传，外部研发靠部分文档+@戳你一下，加双方都看源码，来完成彼此的合作** （有事觉得有和其他开发者一起互动的感觉也很棒）

**这里说一个小的插曲：**

当我初次开始接触 dtk 库的维护时候，学习方式也是口口相传。

记得刚来，就来负责 dtk 的开发与维护，初接触的一周，都不知到该如何敲下手，一行都憋不出来；担心自己凉了，然后网上搜了搜 dtk 关键字，我的妈耶，连一个文档都没得，心里下意识的觉着凉透了，想自学都没得门路，此刻只想点一碗伤心凉粉安慰一下自己；

后那一周，是我的负责人，搬着一个小板凳，坐在我旁边，一行一行的敲着我看的，我才得以快速上手的。后面就想着，一定要尽快的摸清楚 dtk 的绘画自定义皮肤的控件的架构和原理。后面写几篇 dtk 分析，帮助后面小白入门，尽我所能帮助，为它的发展添一块瓦（所幸，在延期了几个月后，将其写出来且发表了 🤣🤣）

至今回忆起来，都感觉有点不好意思，但是那会却又是无可奈何。至今都十分奇怪，为何他的耐心如此只好，大概君子无愠色，就是这种赞誉吧~ （尤其是每天都有很多人找他的情况下，巨忙）；那个时候，也不只是我，还有很多新人也是被手把手的教会 着如何开发 dtk。那会看到，大佬很忙碌，但是又不得不请教他的时候，就觉得很残忍~（尤其是偶尔还打扰周末休息）


#### [](#dtk-的文档时代： "dtk 的文档时代：")dtk 的文档时代：

约 2019 年末，2020 年初~，deepin 和 team 成长的很多，收获了也很多，和外部一起合力成立一个更大的公司（统信软件），来更好的整合资源与预计前景规划 ~ ；

随着 uos v20 sp1 和 deepin v20 beta 的先后在今年发布，以及 deepin 也收获了金钱和其他资源的支持，反手就是一个投入更多的财力人力和其他资源来研发新的 v20 版本。以获得更好的回报~，然后良性循环。

咳咳，好像有点跑题了哦~ 不过落实到和 dtk 相关的就是，有一段时间，一群研发们，疯狂熬夜加班，给补充 dtk 文档（验证了伏笔 :dog:），使用 Doxygen 来生成文档（大部分都是中文的哦~）。

有关 dtk 的 api 使用说明，可以参考：[https://linuxdeepin.github.io/dtk](https://linuxdeepin.github.io/dtk/)

但不知道为什么会显示上次更新的时间还是 20219-07-10，实际后面有补充很多的~。此阶段外部开发，可以很大一个程度来依靠此文档。且 dtk 的每一个 api 的解释都是写在函数的上面的注释地方。直接使用 IDE 的代码提示 + 源码 api 的注释，可以更加高效的开发。

**现阶段就是内部靠： 视频培训 + 手把手带 + Doxygen 文档 + 企业 wx 群 + 翻看源码 + 组员帮助**

**外部靠： 回看视频培训 + Doxygen 文档 + 企业 wx 群@dtk 研发来讲解 + 翻看源码 + 组员帮助**

不过说讲真，dtk 学习，最快最有效的，还是自行翻阅 Qt & dtk 的源码；和自己尝试独立写出一个自绘 dtk 控件皮肤样式来的快。且学习 qt/ c++ 的人，一般自学能力都真的很强，其中 dtk 的项目都已经在 github 上面同步开源了：[linuxdeepin](https://github.com/linuxdeepin)， 其中社区版的分支是和内部的 gitlab 是保持同步的。


### [](#dtk-库的快速入门： "dtk 库的快速入门：")dtk 库的快速入门：

#### [](#如今-dtk-项目的构成（2020-05）： "如今 dtk 项目的构成（2020-05）：")如今 dtk 项目的构成（2020-05）：

**目前，dtk 项目的各个仓库之间的关系如下：** 现在基本是这个格局了（图片基本准确），说一下个人的 dtk 和它们之间的理解：

![](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/20200514160359.png)

实际开发过程中，你是不需要掌握这么多的库的，也不会需要你一个人同时维护这么多的库，且同时有一些库，是很少被改动的。并不是经常需要提交。

**这里再次说一下 dtk 的含义：**

> **dtk = dtk 项目 = 多个仓库集合 = dtkwidget + dtkgui + qt5integration + dtkcore + …**

**关于 dtk 的库，在外网 github 和内网 gitlab 上面是保持一致的，对应的分支是时刻同步的；** 故从内网或者外网下载学习，都是这么开心和轻松。对于 dtk 的学习使用，主要掌握以下两个仓库 qt5integration 、 dtkwidget 的学习和使用，其中主要是在工作和兴趣开发中的书写代码的主要阵地。

#### [](#简述-dtk-的各仓库作用： "简述 dtk 的各仓库作用：")简述 dtk 的各仓库作用：

下面以一种清晰、简陋的理解，全局的概括的讲解 `qt5integration`、`dtkwidget`、`dtkgui`这三个比较常用的仓库（若是第一次接触的话，看完了后，你还记得每一个部分对应的什么麽？😶😶）


##### [](#qt5integration： "qt5integration：")[qt5integration：](https://github.com/linuxdeepin/qt5integration)

- **qt5integration 项目结构组成：**

  - 其主要是由如下几个子部分组成

    - **iconengines:** DIcon 插件相关，方便直接提取项目中 .svg 图片资源，常用如：

      ```cpp
      case SP_DialogResetButton:
                  icon = QIcon::fromTheme(QLatin1String("edit-clear"));
      ```

    - **imageformats:** 图标格式的支持，如新出来的 `dci` 图标格式的预览支持
    - **qt5deepintheme-plugin：** 主题插件和一些基础的图片 svg 资源存储地方，重点为 Resources 文件
    - **styleplugins：** deepin 的自定义 chameleon style 样式插件，基本所有 Dxxxxx 控件，都是在 chameleon.h chameleon.cpp 这两个里面实现的（也是我们最主要修改 code 的地方）
    - **styles：** 测试例子，也是 main.cpp 的函数入口

  - 绘画 dtk 控件的皮肤样式，主要是 chameleon 样式，其中主要的主要效果如下

  ![](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/20200514_165236.png) ![](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/2020-05-14_16-43-35.png)

#### [](#dtkwidget "dtkwidget:")[dtkwidget:](https://github.com/linuxdeepin/dtkwidget)

- **dtkwidget 项目结构组成：**
  - dtkwidget 其主要是由如下几个子部分组成
    - **examples:** 里面的 main.cpp 是整个程序的入口，通常在这里测试
    - **src：** dtk 的自定义 Dxxxx 控件，都存放于其下的 widgets 下， 如 [darrowbutton.h](https://github.com/linuxdeepin/dtkwidget/blob/master/src/widgets/darrowbutton.h) 、[darrowlinedrawer.cpp](https://github.com/linuxdeepin/dtkwidget/blob/master/src/widgets/darrowlinedrawer.cpp)、[ddialog.cpp](https://github.com/linuxdeepin/dtkwidget/blob/master/src/widgets/ddialog.cpp)、[dslider.cpp](https://github.com/linuxdeepin/dtkwidget/blob/master/src/widgets/dslider.cpp) 这些等（我们也通常在这里直接修改 Dxxx.h 和 Dxxx.cpp 文件），是 dtk 开发的主战场。
    - **tools：** 工具相关、可不管
  - 绘画 dtk 自定义的控件的皮肤样式，其中主要的主要效果如下

![](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/2020-05-14_17-16-43.png) ![](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/20200514171919.png)

#### [](#dtkgui "dtkgui:")[dtkgui:](https://github.com/linuxdeepin/dtkgui)

- **dtkgui 项目结构组成：**

  - 这个就是前面说的，现在很少需要修改，但是偶尔要会使用的仓库。比如设计师修改了他们的基础颜色的 Color 的 rgb 数值，调整了一下透明度等，他们给的参数，一般都是在这里来实现，将具体的颜色数值与 Qt 子定义的枚举对应起来。
  - 其中主要是修改 src-dguiapplicationhelper.cpp 该文件，一旦修改，所有其值，所有人的效果都会发生改变，谨慎。

    ![](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/2020-05-14_17-35-37.png)

### [](#下载地址： "下载地址：")下载地址：

[QtExamples](https://github.com/xmuli/QtExamples)

# DTK的历史起源、发展，和简单入门

欢迎 star 和 fork 这个系列的 qt/dtk 学习，附学习由浅入深的目录。

### [](#资料手册-amp-amp-故地址遗迹： "资料手册 && 故地址遗迹：")资料手册 && 故地址遗迹：

#### [](#资料手册： "资料手册：")资料手册：

- [Deepin、DTK 文档参考资料集合](https://ifmet.cn/posts/974df8a3/ifmet.cn/posts/e7de542e/)： 个人整理的一些 DTK 文档资料 (2020-01-05)
- [linuxdeepin](https://github.com/linuxdeepin): deepin 在 github 的开源地址； 大部分项目均已开源（含完整 dtk），且和内部保持一致，无论是想要学习 dtk 的开发，还是研究一些应用软件的使用，提交 pr，均可以在此 github 这个交友网站找到你的想要的资源
- [dtk api 文档](https://linuxdeepin.github.io/dtk/) ：有关 dtk 的 api 中文使用说明；更多注释，请阅源码注释
- [dtk 重绘控件的原理解析](https://github.com/xmuli/QtExamples)：其中第五章节，为 dtk 的源码架构分析、以及入门和开发，一些理解和实战
- [DTK 重绘自定义需求控件](https://xmuli.blog.csdn.net/article/details/104987446)： 从 0 立创造一个非 Qt 原生控件，且自定义其控件皮肤（系列有三篇）
- [DTK 常用和测试代码片](https://xmuli.blog.csdn.net/article/details/106187959)：dtk 的一些开发常用、和测试的代码片
- [使用 DTK 开发，D 开头的宏和命名空间的使用](https://blog.justforlxz.com/2018/01/12/%E4%BD%BF%E7%94%A8DTK%E5%BC%80%E5%8F%91/)：（早期）关于 dtk 宏的使用
- [DDE.dot](https://github.com/BLumia/BLumiaGist/tree/master/Misc/DDE) ：dtk 各仓库之间的联系图
- [deepin 官网](https://www.deepin.org/)：官网的发布社区版 deepin，也是一个获取的用户交流论坛
- **dtk 和 qt 源码 + 注释（最佳）**

#### [](#镜像下载： "镜像下载：")镜像下载：

- [deepin.org/download](https://www.deepin.org/download/)：deepin v20 的 iso 镜像下载地址（官网）
- [chinauos.com/cooperative](https://www.chinauos.com/cooperative)：uos v20 的 iso 镜像官网下载（官网）
- [deepin 镜像国际排名](https://distrowatch.com/table.php?distribution=deepin)： deepin 历史版本（国际排名）。
- &&
- [https://wiki.deepin.org/wiki/Dtk](https://wiki.deepin.org/wiki/Dtk)
- [如何给 DTK 添加文档](https://hualet.org/blog/2018/09/26/%E5%A6%82%E4%BD%95%E7%BB%99-dtk-%E6%B7%BB%E5%8A%A0%E6%96%87%E6%A1%A3/)
- [深度桌面环境](https://www.deepin.org/dde/)
- [关于 deepin 公司（前身）](https://www.deepin.org/aboutus/) （官网）
- [关于 uos 公司 （如今）](https://www.uniontech.com/about) 官网）

#### [](#参考-amp-amp-感谢： "参考 && 感谢：")参考 && 感谢：

在探究 dtk 的历史中，搜寻着一些并不多的网络博客文章，和前辈们、大佬们的沟通，口口相传，文中有使用他们的素材或者博客片段，或者 dtk 的相关介绍、使用技巧链接，表示好感和谢谢 Thanks♪(･ω･)ﾉ [zccrs](https://github.com/zccrs)、[shule](https://github.com/shule1987)、[hualet](https://github.com/hualet)、[BLumia](https://github.com/BLumia)、[xmuli](https://github.com/xmuli)、[justforlxz](https://github.com/justforlxz) …等
