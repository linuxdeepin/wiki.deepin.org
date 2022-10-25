---
title: deepin-community分支与Tag管理
description: 分支与Tag管理
published: true
date: 2022-10-25T02:09:05.119Z
tags: 
editor: markdown
dateCreated: 2022-09-21T02:21:35.049Z
---

**分支命名规范**

deepin 社区版为滚动发布制，一般没有维护分支，`master` 分支即为研发基线分支。对于开发者，若需要多个提交才能完成一个特性时，由于研发过程使用 GitHub 的 Fork + Pull Request 模式，故也不需要单独创建研发分支。但特殊情况下，若确需使用研发分支，则可使用 `develop/*` 作为临时的特性研发分支。


*  `master` 用于作为默认分支，接收 Pull Request，作为当前版本的开发分支使用
*  `develop/*` 仅特殊情况下允许使用的临时研发分支,如在适配RISC-V架构时需要给一些项目提交patch但是该patch暂时不适合合入主线，那么可以申请切出`develop/riscv6` 的分支单独维护，等到稳定后在合并到主线分支
   ps：如果有特殊的需求以上规范无法满足，请通过邮件列表 issues等方式联系我们进行处理

 

**Tag命令规范**

我们推荐采用 [semver](https://semver.org/lang/zh-CN/)的规范，格式为：

主版本号.功能版本.修复版本

> 基于上游开源项目，那么项目版本号为 upstreamversion.UOSversion。如 systemd 上游版本为 250,那么 systemd UOS 版本为 250.1、250.2·······；^

**分支与tag的创建申请**

**新建分支：**

请联系所属项目的SIG组管理员进行分支创建

> TODO： 暂未设计相关工作流管控分支的创建，若有想法欢迎在邮件列表 issues等渠道与我们讨论
{.is-info}


**Tag创建**： 

修改debian/changelog文件时，会自动触发auo-tag的工作流将changelog中的version创建成tag

