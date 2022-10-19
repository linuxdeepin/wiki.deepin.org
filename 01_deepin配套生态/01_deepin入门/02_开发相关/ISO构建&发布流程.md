---
title: ISO构建&发布流程
description: 
published: true
date: 2022-09-15T08:07:23.792Z
tags: 
editor: markdown
dateCreated: 2022-09-15T06:35:57.547Z
---

镜像构建&发布流程
# 一、镜像构建
github 自动镜像构建说明        
日构建镜像，每天凌晨3点在release项目的工作流自动触发构建镜像构建默认使用 stable仓库 testing仓库进行构建，目前镜像构建只有x86架构
v23日构建镜像地址： https://cdimage.deepin.com/daily/

## 二、发布流程
产品发布小组决策版本发布周期以及策略，完成发布评审后相关镜像会同步至cdimage github 百度网盘 www.deepin.org 等渠道，仓库届时同步推送更新。详细流程请参阅[deepin版本生命周期管理](/zh/开发者指南/贡献指南/deepin版本生命周期管理)

![版本发布流程图.drawio_(1).png](/开发者指南/版本发布流程图.drawio_(1).png)