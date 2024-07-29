---
title: 甲辰计划 实习生可观测产出及进出机制
description: 
published: true
date: 2024-07-29T06:30:57.925Z
tags: 
editor: markdown
dateCreated: 2024-07-29T06:30:57.925Z
---

# 可观测产出统计规则

> 原则上任何可在 Internet 上访问到的、与甲辰计划有关的贡献均为可观测产出。

月末将通过脚本自动统计每位实习生的贡献作为基本值，如有遗漏或不在自动统计的范围内的可观测产出，需要在下个月内提出。

自动统计部分：在 GitHub `deepin-community` 和/或 `linuxdeepin` 组织下的：

- 任何新增、被合并的 PR
	- `https://github.com/pulls?q=org%3AOrgID+merged%3A20xx-xx-xx..20xx-xx-xx+author%3AYourGitHubID`
  - `https://github.com/pulls?q=org%3AOrgID+created%3A20xx-xx-xx..20xx-xx-xx+author%3AYourGitHubID`
- 任何提出的 issue
  - `https://github.com/issues?q=org%3AOrgID+created%3A20xx-xx-xx..20xx-xx-xx+author%3AYourGitHubID`

其它部分（需要自行提出，以下仅为举例）：

- 博客文章
- 公开发布的视频
- 提交给上游的贡献

# 进入与退出机制

## 进入实习

- 所有实习生均需要完成必做流程：*使用 实体 RISC-V 设备 或 qemu-user 搭一个打包的环境。* 方可进入正式实习并开始统计可观测产出。
- 实习生需要提交自己的 GitHub ID 以便开通相应权限和月末的贡献统计

## 退出实习

- 连续三个月无产出视为放弃实习
- 新进入的实习生前两个月无产出视为放弃实习
