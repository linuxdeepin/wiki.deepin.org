---
title: 甲辰计划 实习生入门指导
description: 
published: true
date: 2024-07-08T03:44:37.985Z
tags: 
editor: markdown
dateCreated: 2024-07-08T03:44:37.985Z
---

# 入门

## 重要链接一览

- [GitHub 软件包仓库](https://github.com/deepin-community/)
- [构建系统 - main 开发分支](https://build.deepin.com/project/show/deepin:Develop:main)
- [构建系统 - community 开发分支](https://build.deepin.com/project/show/deepin:Develop:community)

# 工作流

## 修包

```plantuml
@startuml
hide empty description

state 查找需要修复的包 : build.deepin.com\n(main,community,dde)
state 本地打包验证
state 修复软件包
state 本地打包测试修复
state 推送代码提交PR : github.com/deepin-community
state PR合并
state 请求集成 : 合并的 PR 下评论 /integrate
state 集成issue创建 : 由 bot 完成，自动评论在对应 PR 中
state issue中提供测试建议并提交测试 : 按照 bot 所给的模板\nassign 操作可能需要权限，可以提交给 mentor
state 集成进入testing仓库 : 流程自动完成

note left of PR合并 : 可见产出+1
note left of 集成进入testing仓库 : 可见产出+1

[*] --> 查找需要修复的包
查找需要修复的包 --> 本地打包验证
本地打包验证 -[dotted]-> 查找需要修复的包 : 没有问题
本地打包验证 --> 修复软件包 : 发现问题
修复软件包 --> 本地打包测试修复
本地打包测试修复 --> 推送代码提交PR
推送代码提交PR -[dotted]-> 修复软件包 : CI 不通过，需要修改
推送代码提交PR --> PR合并
PR合并 --> 请求集成
请求集成 --> 集成issue创建
请求集成 -[dotted]-> 修复软件包 : 集成构建失败
集成issue创建 --> 编辑issue中测试建议并提交测试
issue中提供测试建议并提交测试 -[dotted]-> 修复软件包 : 测试失败
issue中提供测试建议并提交测试 --> 集成进入testing仓库 : 测试成功
集成进入testing仓库 --> [*]


@enduml
```

# 