---
title: 常规 Pull Request 代码贡献流程
description: 介绍基于 Fork + Pull Request 的代码贡献流程
published: true
date: 2022-10-25T02:39:37.311Z
tags: 
editor: markdown
dateCreated: 2022-10-10T03:58:08.468Z
---

# 流程概述

> 如果你完全不熟悉 GitHub 的工作流程，建议优先参阅[GitHub 提供的官方文档](https://docs.github.com/cn/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests)。下面附带了一些简要说明可供参考。
{.is-info}

当您希望向在 GitHub 上进行开发的项目进行代码贡献时，通常会需要使用 Fork + Pull Request 的开发流程来发起代码贡献。

此流程中，您需要先从原始项目中派生（Fork）出一个复刻仓库，并在派生得到的复刻仓库中进行代码修改。修改完毕后，再由复刻仓库向上游原始仓库发起拉取请求（Pull Request），并接受来自上游的代码评审。当上游可以接受相应的代码后，上游会将对应的拉取请求合并（Merge）到上游代码之中，此时即完成了一次代码贡献。

此流程涉及到的关键步骤如下。

# 相关步骤

我们假定希望向 linuxdeepin 名下的 dde-file-manager 仓库发起代码贡献。此时，位于 linuxdeepin 组织下的 dde-file-manager 仓库我们可以称之为 **上游仓库**。

为了方便，下面的步骤中，我们将假设贡献者（您）的 GitHub 用户名为 `username`。

## 创建派生复刻仓库 (Fork)

当我们需要对一个上游仓库进行修改时，我们需要通过 GitHub 的 Fork 功能在自己的账户下创建一个派生仓库。

![2022-10-10_22755.png](/2022-10-10_22755.png)

派生仓库与上游仓库是两个有关联关系的仓库，假设你的 GitHub 用户名为 `username`，当派生仓库创建完毕后，你会得到一个位于 `https://github.com/username/dde-file-manager` 的 git 仓库。**你应当 clone _你所创建的派生仓库_ 到本地**，并在自己的派生仓库中进行相应的修改。修改完毕后，当修改推送到你的派生仓库后，你将会在派生仓库或上游仓库的主页看到创建 PR 的相应提示。

### 提示

1. 由于维护项目会是很频繁的事情，可以考虑将上游仓库添加为一个 remote 以便更新代码，例如 `git remote add upstream https://github.com/username/dde-file-manager.git` 来添加名为 `upstream` 的远程端来追踪上游仓库。
2. **强烈建议** 尽量在新的本地分支中进行代码变更，以便后续主线分支可以更方便的更新。

## 常规 Pull Request 发起流程

**假定**自己克隆了自己的派生仓库到本地，派生仓库对应的 remote 名为 `origin`，自己的修改在 `fix-bug-12345` 分支进行，则当本地 `fix-bug-12345` 推送到 `origin` 后，可在 GitHub 看到近似下图的提示

![2022-10-10_37908.png](/2022-10-10_37908.png)

点击后即可看到创建 PR 的相关界面：

![2022-10-10_38948.png](/2022-10-10_38948.png)

在此界面填写相关信息后提交即可。若需要，也可直接在此界面（右侧）添加 review 人员。
PR 创建后，即可看到对应的 PR 页面，相关的评论，CI 检查和 review 状态都会显示在其中，此后，邀请同组具有 maintain 权限的组员进行 review，并在得到 Approve 后即可进行合入。

## 修改 Pull Request

若代码存在问题，**无需**关闭 Pull Request 并重新创建新的 Pull Request。你可以直接在发起Pull Request 时所使用的原分支直接推送新的提交**或**覆盖原有的提交（后者适用于需要修改原有的提交信息的情况），推送后，相应的提交会直接更新在原 Pull Request 中，也会自动重新触发相关检查。