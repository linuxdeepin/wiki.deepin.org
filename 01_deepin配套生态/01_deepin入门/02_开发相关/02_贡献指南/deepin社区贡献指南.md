---
title: deepin-community 贡献指南
description: 
published: true
date: 2022-11-16T07:36:53.083Z
tags: 
editor: markdown
dateCreated: 2022-09-08T05:55:30.564Z
---

# 关于

[deepin-community](https://github.com/deepin-community) 组织用于维护 deepin 发行版中的所有软件包，这些仓库的代码一般都是直接从上游仓库复制而来，且在它的基础上添加与打包相关的 debian 目录，日常只会修改此目录来维护上游项目。deepin 发行版相关的所有开发和维护工作均在此组织中进行。

# 贡献指南
本文默认为您已经了解了代码贡献流程，如您对此方面存在疑惑，请参阅[贡献者手册](https://wiki.deepin.org/zh/01_deepin%E9%85%8D%E5%A5%97%E7%94%9F%E6%80%81/01_deepin%E5%85%A5%E9%97%A8/02_%E5%BC%80%E5%8F%91%E7%9B%B8%E5%85%B3/02_%E8%B4%A1%E7%8C%AE%E6%8C%87%E5%8D%97/%E5%8F%82%E4%B8%8E%E5%85%B1%E4%BA%AB)

# 预备知识
## deepin 发行版的工作模式
关于软件包到仓库的流程请参阅[仓库流转规范](https://wiki.deepin.org/zh/01_deepin%E9%85%8D%E5%A5%97%E7%94%9F%E6%80%81/01_deepin%E5%85%A5%E9%97%A8/02_%E5%BC%80%E5%8F%91%E7%9B%B8%E5%85%B3/04_%E4%BB%93%E5%BA%93/%E4%BB%93%E5%BA%93%E6%B5%81%E8%BD%AC%E8%A7%84%E8%8C%83)
## ISO 构建&发布流程
关于ISO构建与发布相关流程请参阅[ISO构建&发布流程](https://wiki.deepin.org/zh/01_deepin%E9%85%8D%E5%A5%97%E7%94%9F%E6%80%81/01_deepin%E5%85%A5%E9%97%A8/02_%E5%BC%80%E5%8F%91%E7%9B%B8%E5%85%B3/ISO%E6%9E%84%E5%BB%BA&%E5%8F%91%E5%B8%83%E6%B5%81%E7%A8%8B)

## topic仓库机制说明
关于存放构建产物，方便多项目交互开发[topic仓库机制](https://wiki.deepin.org/zh/02_%E6%8C%89%E8%BD%AF%E4%BB%B6%E5%8A%9F%E8%83%BD%E5%88%92%E5%88%86/02_%E5%BC%80%E5%8F%91%E4%BA%BA%E5%91%98%E5%B8%B8%E7%94%A8%E8%BD%AF%E4%BB%B6%E4%BB%8B%E7%BB%8D/01_%E7%BC%96%E7%A8%8B%E5%BC%80%E5%8F%91/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6/%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9/topic%E4%BB%93%E5%BA%93%E6%9C%BA%E5%88%B6%E8%AF%B4%E6%98%8E)

## 软件开发生命周期管理
关于软件包的引入退出原则介绍[软件开发生命周期管理](https://wiki.deepin.org/zh/01_deepin%E9%85%8D%E5%A5%97%E7%94%9F%E6%80%81/01_deepin%E5%85%A5%E9%97%A8/02_%E5%BC%80%E5%8F%91%E7%9B%B8%E5%85%B3/01_%E8%BD%AF%E4%BB%B6%E5%8C%85/%E8%BD%AF%E4%BB%B6%E5%BC%80%E5%8F%91%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E7%AE%A1%E7%90%86)

## 软件包构建指南
关于deepin下的软件包构建说明[软件包构建](https://wiki.deepin.org/zh/02_%E6%8C%89%E8%BD%AF%E4%BB%B6%E5%8A%9F%E8%83%BD%E5%88%92%E5%88%86/02_%E5%BC%80%E5%8F%91%E4%BA%BA%E5%91%98%E5%B8%B8%E7%94%A8%E8%BD%AF%E4%BB%B6%E4%BB%8B%E7%BB%8D/01_%E7%BC%96%E7%A8%8B%E5%BC%80%E5%8F%91/%E6%89%93%E5%8C%85%E5%B7%A5%E5%85%B7/%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9/%E5%A6%82%E4%BD%95%E5%9C%A8deepin%E7%A4%BE%E5%8C%BA%E6%9E%84%E5%BB%BAdeb%E6%A0%BC%E5%BC%8F%E8%BD%AF%E4%BB%B6%E5%8C%85)

## 分支与tag管理
关于deepin-community组织下的分支与tag管理请参阅[deepin-community分支与Tag管理](https://wiki.deepin.org/zh/01_deepin%E9%85%8D%E5%A5%97%E7%94%9F%E6%80%81/01_deepin%E5%85%A5%E9%97%A8/02_%E5%BC%80%E5%8F%91%E7%9B%B8%E5%85%B3/deepin-community%E5%88%86%E6%94%AF%E4%B8%8ETag%E7%AE%A1%E7%90%86)

## deepin-community协作流程
 	  关于代码提交，commit检查，软件包的集成，仓库的创建等流程请参考该文档, [deepin-community协作流程](https://wiki.deepin.org/zh/01_deepin%E9%85%8D%E5%A5%97%E7%94%9F%E6%80%81/01_deepin%E5%85%A5%E9%97%A8/02_%E5%BC%80%E5%8F%91%E7%9B%B8%E5%85%B3/02_%E8%B4%A1%E7%8C%AE%E6%8C%87%E5%8D%97/deepin-community%E5%8D%8F%E4%BD%9C%E6%B5%81%E7%A8%8B)

## 可接受的代码形式
> ps: 所有代码形式应该以 GitHub 的 Fork + Pull Request 模式提交
{.is-info}

### 打补丁
针对deepin环境下特有的改动应以patch文件的方式提交，若该patch已被上游项目接受应当删除patch合入到源码中。debian目录补丁应用方式参见软件包构建中的相关说明

### 修复安全漏洞
参考打补丁的形式，若上游项目已合并该patch则合入patch到源码中，通过软件包更新的流程方式发布到仓库中。

### 升级包版本
修改源码形式同步上游新版本的代码，commit注明上游版本来源，若该项目有针对deepin环境下的特有patch则应保证patch的正常应用。

### 软件包的更新/删除/新增
[软件包的更新/删除/新增](https://wiki.deepin.org/zh/01_deepin%E9%85%8D%E5%A5%97%E7%94%9F%E6%80%81/01_deepin%E5%85%A5%E9%97%A8/02_%E5%BC%80%E5%8F%91%E7%9B%B8%E5%85%B3/01_%E8%BD%AF%E4%BB%B6%E5%8C%85/%E8%BD%AF%E4%BB%B6%E5%8C%85%E7%9A%84%E6%B7%BB%E5%8A%A0_%E6%9B%B4%E6%96%B0_%E5%88%A0%E9%99%A4)

# 社区角色
## 贡献者 Contributor


定义：贡献者是社区中持续活跃的贡献者，可以参与 SIG 组活动，主动发现问题提交issues和PR。

要求：

* 拥有Github上的注册账户
* 订阅邮件列表
* 在Github上提交或审核PR
* 在Github上对问题进行归档或评论
* 积极参与1个或多个SIG组
* 阅读贡献指南、开发者手册
  责任与权利：

* 响应分配的问题和PR
* 贡献的代码应该
  * 经过良好的测试
  * 能够让测试用例始终通过
  * 解决后继发生的错误或问题
* 可以分配问题或 PR, 可以@其他成员进行评论
  
## 维护者 Maintainer

定义：维护者是 SIG 组的组长或者管理委员会成员，也是软件包的维护者，能够像 Committer 一样审查和批准代码贡献。代码审查的重点是代码质量和正确性，而批准的重点是对贡献的整体接受度。所有 Committer 的责任与权力，Maintainer 均具有。除此之外，Maintainer 还承担了团队的技术路线、内外协调等工作。

要求：

* 作为贡献者至少 3 个月
* 作为至少参与了 12 次 PR 的审阅
* 审阅或合并至少 30 个基本 PR 到代码库
* 熟悉代码库
* 可以自我提名，也可以由子项目Maintainer提名，并且没有其他子项目Maintainer的反对
  责任与权利：

确定 SIG 所负责项目的技术路线：包括规划和决策 SIG 技术方向、路标

社区角色权限与SIG组的权限存在一定的重复，社区角色应该在SIG角色的更上一层，当SIG组不在活跃相关项目缺乏维护，由Committer team临时处理PR等流程，SIG组owner应当具有当前SIG组下边的所有项目的write权限，tag的创建需走release的工作流提交PR的形式。

权限说明：

Contributor**：**具有PR提交权限

Committer：具有deepin-community下所有项目的review权限

Maintainer：具有Committer所有权限，具有仓库创建审批等权限


* 规划、架构演进。
* 制定 SIG 所负责项目的发布计划：确定 SIG 的关键需求和发布计划；参与社区的 PM 活动，并协调 SIG 计划和社区版本的里程碑时间表匹配。
* 参与社区协调活动：作为 SIG 的代表参与社区技术委员会或理事会组织的活动和特定会议等。
* 召集 SIG 组会议：定期召集 SIG 会议，决策 SIG 内上升的争议。