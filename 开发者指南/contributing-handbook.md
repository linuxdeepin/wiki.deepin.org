---
title: 开发者手册
description: 关于开发者如何进行代码贡献的指南文档
published: true
date: 2022-09-01T06:12:30.575Z
tags: 
editor: markdown
dateCreated: 2022-08-29T07:13:11.941Z
---

# 参与贡献

欢迎！无论您是想要实现新的特性，还是想贡献简单的 Patch ，甚至只是想修改文档中的错别字，我们都非常欢迎并感谢您的帮助。

如果您还不清楚应当从何处以及如何获取源码，请先参阅 [获取和使用源码](/zh/开发者指南/获取和使用源码)。

# 贡献通道

1. linuxdeepin 组织：[linuxdeepin 贡献指南](/zh/开发者指南/贡献指南/linuxdeepin-contributing-handbook)
2. deepin-community 组织：TODO

> TODO: 提供并链接到 deepin-community 组织下仓库的贡献方式文档
{.is-warning}

# 贡献流程概览

与 deepin 所相关的研发工作流均位于 GitHub，故代码贡献流程与 GitHub 常规的 Fork + Pull Request 的流程并无太大差异，关于 GitHub PR 的介绍请查看[此文档](https://docs.github.com/cn/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)。

需要注意的是，deepin 对其中的一些步骤有额外的要求，例如我们具备对提交信息格式的要求等。对于相关的要求，可参阅下方的 "注意事项" 段落以及对应组织的代码贡献指南文档。

大致的代码贡献流程如图所示：

```plantuml
start
partition “初始阶段” {
  :注册 GitHub 账号;
  :复刻（Fork）仓库;
  :在复刻的仓库中进行代码修改;
}
partition “发起贡献” {
  :发起 Pull Request;
	if (CLA 检查) is (未签署 CLA) then
  	:签署 CLA;
  endif
}
partition “代码评审阶段” {
	repeat
    split
      :代码评审;
    split again
      :自动化测试;
    end split
  backward:推送修改到复刻分支;
  repeatwhile (若检查未通过)
}
:合入代码;
stop
```

## 其他参考文档

- [创建 GitHub 账号](https://docs.github.com/cn/get-started/signing-up-for-github/signing-up-for-a-new-github-account)
- [关于 Pull Request](https://docs.github.com/cn/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
- [git 使用手册](https://git-scm.com/book/en/v2)（[中文版](https://git-scm.com/book/zh/v2)）
- [贡献许可协议](/zh/开发者指南/贡献许可协议)

# 许可协议和行为准则

## 开源许可协议与贡献者协议

每个项目均有附带其对应的许可协议，在您开始着手代码贡献之前请确保您理解并同意对应的许可协议。若未另外声明，则您的代码将使用与对应项目相同的许可协议进行授权。

为了便于 deepin 可以在必要时进行代码许可协议的调整来解决许可协议问题，在您打算进行代码贡献时，还需要签署相应的 CLA 协议（注：对于个人贡献者而言可以简单的在线完成签署）。与此相关的详情可参见 [贡献协议](/zh/开发者指南/贡献协议) 页面。

如果您对此相关有任何疑问，请邮件咨询 support@deepin.org。

## 行为准则

健康的社区需要大家共同创造，在此我们使用 [Contributor Covenant 2.1 贡献者行为准则](https://www.contributor-covenant.org/zh-cn/version/2/1/code_of_conduct/)，请大家一同遵守。

# 获取代码

目前 deepin 开源项目的所有源代码均可在 GitHub 获取，故您只需要找到对应的项目，然后通过 `git clone` 或你喜欢的方式获取代码即可。

由于 deepin 的各个项目的 `master` 分支均是开发分支，某个应用程序的 `master` 分支状态可能会依赖其他也同时处于 `master` 分支状态的版本。因此，直接获取 `master` 分支的代码在很多情况下并不总是保证能够编译通过。故当您从 git 仓库获取版本时，建议您在必要时先 `git checkout` 切换到对应您所需要的 tag 上，再尝试构建。

项目的对应 `README.md` 文档通常会有构建项目等相关的步骤描述，请参阅这些文档以帮助您构建项目。

# 提交补丁或 Pull Request（拉取请求）

我们接受 GitHub Pull Request ，开发提交的合并均通过 Pull Request 进行，你可以直接点击对应项目的 Fork 按钮得到你自己的 Fork，在其上方进行提交，并在修改完毕后直接通过 GitHub 网页发起 Pull Request 即可。对于 Pull Request 的介绍和使用方式，可以参阅 [GitHub 帮助文档中的 “关于拉取请求” 部分](https://docs.github.com/cn/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)。

得益于 GitHub 所提供的代码 Review 和自动化工具，我们可以很方便的进行代码评审并使用自动构建来及时的发现代码中存在的问题。如果您还不熟悉 GitHub 的代码评审流程，可参阅 [GitHub 的代码评审相关文档](https://docs.github.com/cn/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/about-pull-request-reviews)。

## 持续集成检查

为了确保代码质量，并确保项目合规，我们为项目配置了相应的持续集成检查。其中，常见的检查包括如下几项：

- CLA 贡献者协议检查（`clacheck`）
- Commit 信息检查（`commitlint`）
- UOS 环境构建检查（`build-deb`）
- deepin 和其它发行版构建检查（`build-distribution`）

其中，关于 Commit 信息的检查请参阅 [Commit-提交信息规范](/zh/开发者指南/规范文档/Commit-提交信息规范)。

### GitHub 持续集成辅助工具

为了使开发过程变得更轻松，使研发人员和贡献者可以更灵活的处理持续集成检查遇到的问题，我们为 GitHub 的持续集成提供了一些便捷指令，来执行类如指派代码评审人员、重新触发指定检查、主动进行代码合并等动作。对于这些命令，我们也提供了相关的插件脚本，来使开发者可以自动化输入对应命令。

对于相关的持续集成辅助工具和用法，请参阅 [pull-request-bot-commands-list](/zh/开发者指南/pull-request-bot-commands-list)。

## 软件包开发仓库

请参阅 [使用action创建新的git仓库](/zh/开发者指南/仓库/使用action创建新的git仓库)

## 注意事项

在确保您的修改可以被接受后，您就可以编写 Patch 并通过上述段落中提到的方式提交您的 Patch  / Pull Request 了。在您着手准备 Patch 时，请留意下方的注意事项：

1. **Commit 信息规范**
请参考 [Commit-提交信息规范](/zh/开发者指南/规范文档/Commit-提交信息规范)，
2. **一个 Pull Request 只做一件事**
您可能会希望对某个项目做一系列的改动，但为了保证代码的提交信息的有序，也为了保证代码审核方便有序，请保证您的一个 Pull Request 只做一件事，对于不同的修改内容，请提交多个不同的 Pull Request 来实现。
3. 在代码的修改与提交过程中，可能涉及到一些与 deepin 相关的品牌专有名词，此时，请参阅 [品牌专有名词指导方针](/zh/开发者指南/branding-guideline) 文档。

# 取得联系/寻求帮助

如果阅读上方的内容后依然有疑问，或是在任何步骤中遇到了问题，都可以直接与开发者们取得联系，一同进行讨论交流。

我们提供常规的 Issue 与讨论板、邮件列表、即时聊天等途径以供联系。具体的联系方式请参阅 [交流方式](/zh/关于Deepin/Deepin社区/交流方式) 页面。

Happy Hacking！