---
title: Pull Request 机器人命令列表
description: 介绍关于 GitHub 相关组织下所配置的辅助工具机器人的命令用法
published: true
date: 2022-08-31T08:27:50.019Z
tags: 
editor: markdown
dateCreated: 2022-08-31T08:27:50.019Z
---

Command              | Explain                                | 说明
---------------------|----------------------------------------|---------------------------
`/check`             | Retrigger all CI checks                | 重新触发所有 CI 检查
`/check jobname`     | Retrigger *jobname*                    | 重新触发 *jobname* 检查
`/assign @username`  | Assign @username for the given PR      | 指派 @username 处理 PR
`/review @username`  | Add @username to reviewer              | 添加 @username 为代码评审人员
`/merge`             | Merge PR if meed all merge requirement | 在满足所有合并要求时，执行合并
`/+1` or `/approve`  | Add a approve label (maintainer-only)  | 添加一个 approve 标签

`/check jobname` Usage | `/check jobname` 用法
-----------------------|----------------------
Assume you want to retrigger a job says<br> `build-distribution / check_job / archlinux-build`,<br> then you need to use <br>`/check archlinux-build` or <br>`/check check_job/archlinux-build`<br>to retrigger it. | 假定你要重新触发一个显示为 <br>`build-distribution / check_job / archlinux-build` <br>的任务，那么你需要使用 <br>`/check archlinux-build` 或 <br>`/check check_job/archlinux-build` <br>任一方式进行触发。

使用油候脚本简化命令操作：
1. 在浏览器安装tampermonkey插件
2. 在tampermonkey插件打开管理面板 - 实用工具，找到“从 URL 安装”，输入 https://raw.githubusercontent.com/deepin-community/deepin-chatopt-script/main/chatopt.js 
3. 安装成功后，在 GitHub 的 Pull Request页面评论框下放会自动添加 /merge 和 /check 相关操作按钮。

Merge Requirement:

1. At least one valid approval (from people who have [at leaset *write* permission](https://docs.github.com/en/organizations/managing-access-to-your-organizations-repositories/repository-roles-for-an-organization#permissions-for-each-role)).
2. All CI check passed.

Please note: `/approve` is only for marking purpose, for actual code review, you still need to go through the GitHub's code review process.

合并要求:

1. 至少有一个有效的 Approval（加分/合并许可）
2. 所有 CI 检查通过

请注意: `/approve` 仅为添加标记的目的，对于实际的代码评审，请使用 GitHub 的代码评审流程。

