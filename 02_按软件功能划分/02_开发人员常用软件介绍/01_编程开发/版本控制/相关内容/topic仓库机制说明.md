---
title: topic仓库机制说明
description: 仓库
published: true
date: 2022-11-25T07:12:17.310Z
tags: 
editor: markdown
dateCreated: 2022-09-15T08:19:12.095Z
---

**简介**

为方便可持续开发集成等需求，启动topic的管理机制，对代码变更进行标签化的分类，可以很方便的选择提交PR时自动构建的产物存放的仓库，方便进行rc（release-candidate）阶段的多源码包相关性项目进行联调测试，简化集成流程，提高集成准确率、自动化率，方便后期的主题相关的代码、软件包仓库的回溯。


**使用方式**

1. 从deepin-comunity fork 项目到个人账户下
2. 从个人账户下clone项目到本地
3. 添加upstream相关信息

```plain
 git remote add upstream "urlxxxx"
 git checkout upstream/master #切到upstream的游离分支
 git checkout -b topic-grep-test #切出本地分支,若需要使用topic机制需采用topic-开头的形式命名分支

```

3. 修复代码，推送topic开头命令的分支到个人账户下的仓库中
4. 提交PR，从topic分支合入到upstream的master分支
5. 构建通过后的deb包自动推送到topic仓库中

```plain
仓库地址：https://ci.deepin.com/repo/topics/beige/ #合并之后自动在ci仓库下创建grep-test的仓库
```

6. 仓库使用方式

```plain
echo "deb [trusted=yes] https://ci.deepin.com/repo/topics/beige/grep-test/ unstable main
" >> /etc/apt/source.list
```

7. 参考样例
   [https://github.com/deepin-community/grep/pull/5](https://github.com/deepin-community/grep/pull/5)

