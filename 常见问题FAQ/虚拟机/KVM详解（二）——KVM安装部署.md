---
title: KVM详解（二）——KVM安装部署
description: 
published: true
date: 2022-11-23T01:21:51.602Z
tags: 
editor: markdown
dateCreated: 2022-11-23T01:21:26.084Z
---

# KVM详解（二）——KVM安装部署
一、硬件设置
为了进行KVM的安装，我们首先进行一些硬件上的配置。
我们使用费Vmware的虚拟机，配置2个G的内存，同时新加一块30G的硬盘，以供KVM的虚拟机安装使用，然后再给虚拟机设置4个处理器，同时，打开处理器的虚拟化引擎，在虚拟化Intel、虚拟化CPU和虚拟化IOMMU等选项上打上勾，配置完成后的页面如下所示：
