---
title: deepin-ports 合并计划
description: 介绍 deepin-ports 的架构适配仓库及其合并计划
published: true
date: 2024-02-02T01:45:21.842Z
tags: 
editor: markdown
dateCreated: 2023-08-21T04:02:03.889Z
---

# 仓库介绍

## 进展更新

**deepin:stable 稳定仓库 20240201 更新**
|仓库|main|community|dde|实机测试|镜像|发布状态|
|--|--|--|--|--|--|--|
|`riscv64`|🟩 大部分构建成功|🟩 大部分构建成功|✅ 全部构建成功|✅ DDE 桌面环境已就绪|🟩 构建完成|🟩 等待 Beta3 发布|
|`loong64`|🟩 大部分构建成功|🟩 大部分构建成功|🟩 大部分构建成功|🟩 DDE 桌面环境基本就绪|⬛ 尚未开始|⬛ 尚未发布|


## 设备支持

预计 `loong64` 支持所有 3A5000/3A6000 处理器的龙芯平台。
预计 `riscv64` 支持以下设备：
- Hifive Unmatched
- Sifive VisionFive 2
- Sipeed LicheePi 4A
- DC ROMA

## 仓库列表

旧的 `deepin-ports` 仓库如下：

```
https://ci.deepin.com/repo/deepin/deepin-ports/deepin-port-stage1/
```

新的 `deepin` 主线仓库同 CI 滚动构建仓库：

```
https://ci.deepin.com/repo/obs/deepin:/Develop:/main/standard/
https://ci.deepin.com/repo/obs/deepin:/Develop:/community/standard/
https://ci.deepin.com/repo/obs/deepin:/Develop:/dde/standard/
```

新的 `deepin` 主线仓库同 CI 稳定构建仓库：

```
https://ci.deepin.com/repo/obs/deepin:/stable:/main/stable/
https://ci.deepin.com/repo/obs/deepin:/stable:/community/stable/
https://ci.deepin.com/repo/obs/deepin:/stable:/dde/stable/
```

如需实机测试，需要使用 debootstrap 并自行构建内核。

# 镜像构建脚本

```
https://github.com/YukariChiba/deepin-ports-image/
```

# 仓库合并计划

## 合并目标

~~0. 在 deepin 主线构建出 RV64/LA64 的基本工具链~~ (已完成)
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

构建资源： 
- SG2042 EVB x4
- SG2042 Server x2 (山东大学提供)

### loongarch64

构建资源：
- 3C5000 x3
- 3A5000 x2
- 3A6000 x2

# 相关文件

- [版本差异表](https://docs.google.com/spreadsheets/d/1rc8iJo7I9JTxvMvAC7RHhVKHMhcjav7H3QMETcFL4ZE/edit?usp=sharing)