---
title: 代码提交
description: 
published: true
date: 2022-09-21T02:35:16.905Z
tags: 
editor: markdown
dateCreated: 2022-09-21T02:35:16.905Z
---

# 可以接受的代码
> ps: 所有代码形式应该以 GitHub 的 Fork + Pull Request 模式提交
{.is-info}

## 打补丁
针对deepin环境下特有的改动应以patch文件的方式提交，若该patch已被上游项目接受应当删除patch合入到源码中。debian目录补丁应用方式参见软件包构建中的相关说明

## 修复安全漏洞
参考打补丁的形式，若上游项目已合并该patch则合入patch到源码中，通过软件包更新的流程方式发布到仓库中。

## 升级包版本
修改源码形式同步上游新版本的代码，commit注明上游版本来源，若该项目有针对deepin环境下的特有patch则应保证patch的正常应用。