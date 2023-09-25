---
title: deepin-ports 仓库管理
description: 介绍 deepin-ports 的架构适配仓库及其管理
published: true
date: 2023-09-25T02:04:51.022Z
tags: 
editor: markdown
dateCreated: 2023-08-21T04:02:03.889Z
---

# 仓库介绍

目前 `deepin-ports` 仓库尚未合入主线，CI 流程未就绪，`deepin-ports` 主仓库地址为：

```
https://ci.deepin.com/repo/deepin/deepin-ports/deepin-port-stage1/
```

如需实机测试，需要使用 debootstrap 并自行构建内核。

# 仓库合并计划

## 合并目标

0. 在 deepin 主线构建出 RV64/LA64 的基本工具链
1. 将 deepin-ports 的仓库的 patch （几乎）完全合并入主线
2. 将 deepin-ports 必需的高版本软件包在主线中（几乎）完全升级
3. 将 deepin-ports 中无法合入主线的小部分软件包在主线的分支中良好地维护

主线构建项目： https://build.deepin.com/project/show/deepin:Develop:main

## 合并方式

由于 RV64/LA64 架构的软件包版本超前/落后于 `deepin` 主线，需要建立 `deepin-ports`，纳入 OBS 的构建体系。（可考虑在 deepin-community 的对应包仓库下建立 `ports/{ARCH}` 的分支）

合并遵循 **升级优先于降级，主线优先于 port** 的规则。

### 超前版本的合并

版本超前的软件包需要等待主线更新到对应版本后进行 patch 的合并。

### 落后版本的合并

版本落后的软件包，需要更新至主线的对应版本，并重新打 patch，测试构建。

### 基础组件的合并

对于 llvm/gcc/glibc/rust/binutils 等影响较大的系统基础组件，应当与主线形成一致决策后选择合并策略。

### 未被合并的组件

对于主线尚不接受更新的组件，在主线仓库建立对应的分支，维护对应的 patch。

## 构建平台支持

### riscv64

构建资源： SG2042 EVB x2

### loongarch64

构建资源：3C5000 x1

# 相关文件

- [版本差异表](https://docs.google.com/spreadsheets/d/1rc8iJo7I9JTxvMvAC7RHhVKHMhcjav7H3QMETcFL4ZE/edit?usp=sharing)