---
title: linuxdeepin 贡献指南
description: 向 linuxdeepin 组织下进行代码贡献时的参考手册指南文档
published: true
date: 2022-09-07T07:46:24.977Z
tags: 
editor: markdown
dateCreated: 2022-09-01T05:39:03.035Z
---

# 关于 linuxdeepin 组织

[linuxdeepin](https://github.com/linuxdeepin/) 组织用于开发 deepin 公司自研和 fork 的各个项目，如 DDE、DTK 等，这些项目的 Bug 管理和代码提交均在此组织中进行。

# 贡献指南

本文默认为您已经了解了代码贡献流程，如您对此方面存在疑惑，请参阅《[贡献者手册](/zh/开发者指南/contributing-handbook)》。

## 可以接受的代码

我们很乐意帮助合并社区贡献者的代码，但并非所有类型的贡献都能够被并入，故在开始贡献代码前，请确保您的变更是可以被接受的。

### 缺陷修复（bug fix）或拼写更正（fix typo）

我们可以接受缺陷修复（bug fix）或拼写更正 (fix typo) 类的问题修补，故您无需顾虑您的代码在符合下方准则的情况下是否可以并入。

### 新特性（feature）或重构（refactor）

deepin 同时存在开源社区版本和面向企业等实体的专业版本，我们的主要功能特性的开发并非由社区驱动，故如果您希望对某个模块进行重构或增加额外的支持（如插件），或是希望实现新的特性并希望您的新特性合并进来，请先考虑在对应的项目仓库发起一个新的 Issue 描述您的特性/实现目标，然后与我们讨论相关的细节来确保您的功能能够接纳并合并到我们的主线代码中。

如果您希望增加的特性在最终的讨论后还是无法被接受，也请希望您理解。如果您希望的话，您可以创建并维护您自己的衍生版 (fork) 来使用，也欢迎您将您的派生版本分享给社区用户，或和有相同需求的社区用户一同维护对应的社区仓库。

### 翻译（translation）与本地化（l10n）

我们的多国语言翻译流程并不在 GitHub 中进行，如果您希望修正（非英语的）本地化翻译问题，或是增加新的本地化语言支持等相关内容，请移步 [Transifex](https://www.transifex.com/linuxdeepin/public/) 平台。

若您希望修正英语原文中存在的问题，请参见上述的 拼写更正（fix typo） 相关的描述。

## 开发规范

### 代码风格

linuxdeepin 组织中的项目大多是使用 Qt/C++ 开发，因此我们目前只规定了这些项目的代码风格，此规范发布在 [deepin-styleguide](https://github.com/linuxdeepin/deepin-styleguide) 项目，目前最新为 [v0.0.1](https://github.com/linuxdeepin/deepin-styleguide/releases/tag/v0.0.1) 版本。此代码风格对于 C/C++ 项目是通用的，但是如果某个项目中也定义了自己的代码风格要求（如 fork 自上游的非 deepin 自研项目），则优先遵守项目自身的要求，项目中未定义的地方则继续遵守此要求。

### API 设计原则

一个良好的 API 设计对代码的可持续性和使用使用门槛都能有很大的增益，特别是对于 DTK 等开发库项目，必须要要严格遵守设计原则。要想设计好一个原则，需要长时间的积累，运用大量的编程经验，吸取历史教训才能完成制定，不可随意而为，另外 deepin 的众多自研项目均是使用 Qt 开发，因此综合多方面的考虑，我们决定自研项目与 Qt 项目的 API 设计原则保持一致，即遵守[此原则](https://wiki.qt.io/API_Design_Principles)。此外，对于 fork 自上游的项目，需要遵守此项目原本的设计原则。

### 开源/许可协议合规准则

自研项目需遵守[《开源代码合规准则》](https://wiki.deepin.org/zh/%E5%BC%80%E5%8F%91%E8%80%85%E6%8C%87%E5%8D%97/%E5%BC%80%E6%BA%90%E4%BB%A3%E7%A0%81%E5%90%88%E8%A7%84%E5%87%86%E5%88%99)。
fork 自上游的项目，需遵守项目自身的规范。

### 二进制兼容性解决方案

对于 Qt/C++ 项目，我们采用 D-Pointer 机制来解决二进制兼容性问题，此机制已在 DTK 开发库项目中应用。关于 D-Pointer 的详细介绍请参见[此文档](https://wiki.qt.io/D-Pointer)。

### 分支管理

对于现行项目的分支命名规范，请参阅 [Git 分支命名规范](/zh/开发者指南/规范文档/branch-naming-convention)。

# 社区角色

我们以如下三类名称描述不同参与程度的社区贡献者。

## Contributor 贡献者

参与过 linuxdeepin 组织内任何程度的贡献，如报告 Issue，提交 PR 的，均为 linuxdeepin 组织的 Contributor。

## Developer 开发者

提交 Pull Request，且不仅仅是 fix typo 类的贡献，而是为所贡献的项目带来了更大的收益，如解决 Bug，添加功能，此类贡献者即为 linuxdeepin 组织的 Developer。

以开发者身份参与贡献的成员，在满足相关要求的情况下，可以自我提名或联系其他维护者，将自己提名为 Maintainer 维护者。

## Maintainer 维护者

各个项目的主要 Developer（代码贡献量排名靠前的社区成员），可以被此项目现有的 Maintainer 或 linuxdeepin 组织的 Owner 提名为此项目的 Maintainer。

### 相关要求

相对于普通开发者，我们需要确保维护人员能承担维护项目的责任，故对维护人员具有一定要求：

1. 熟悉代码库（对于大型项目，至少应了解项目大体结构，并熟悉自己所维护的特定模块）
2. 能够熟练运用 GitHub 进行代码评审
3. 作为开发者，向项目提交并合入过至少 10 次代码贡献
4. 参与过至少 10 次对其他开发者的代码评审

### 职责与权利

1. 负责自己相应部分的开发或维护，也参与对应代码相关的变更的代码评审。
2. 核心维护者也将主导项目的技术选型，代码架构和发布规划。
3. Maintainer 可以提名其他开发者成为 Maintainer。
4. 组织相关的研发、分享或讨论会议。