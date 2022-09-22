---
title: Pull Request 机器人命令列表
description: 介绍关于 GitHub 相关组织下所配置的辅助工具机器人的命令用法
published: true
date: 2022-08-31T08:36:45.474Z
tags: 
editor: markdown
dateCreated: 2022-08-31T08:27:50.019Z
---

# 可用命令总览

命令                  | 说明
---------------------|---------------------------
`/check`             | 重新触发所有 CI 检查
`/check jobname`     | 重新触发 *jobname* 检查
`/assign @username`  | 指派 @username 处理 PR
`/review @username`  | 添加 @username 为代码评审人员
`/merge`             | 在满足所有合并要求时，执行合并
`/+1` or `/approve`  | 添加一个 approve 标签（限维护者使用）


## `/check jobname` 用法

假定你要重新触发一个显示为 `build-distribution / check_job / archlinux-build` 的任务，那么你需要使用 `/check archlinux-build` 或 `/check check_job/archlinux-build` 任一方式进行触发。

# 使用油候脚本简化命令操作

1. 在浏览器安装 Tampermonkey 插件
2. 在 Tampermonkey 插件打开管理面板 - 实用工具，找到“从 URL 安装”，输入 https://raw.githubusercontent.com/deepin-community/deepin-chatopt-script/main/chatopt.js 
3. 安装成功后，在 GitHub 的 Pull Request页面评论框下放会自动添加 /merge 和 /check 相关操作按钮。

# 合并要求

1. 至少有一个有效的 Approval（加分/合并许可）
2. 所有 CI 检查通过

请注意: `/approve` 仅为添加标记的目的，对于实际的代码评审，请使用 GitHub 的代码评审流程。

