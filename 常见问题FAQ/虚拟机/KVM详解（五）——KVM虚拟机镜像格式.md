---
title: KVM详解（五）——KVM虚拟机镜像格式
description: 
published: true
date: 2022-11-24T12:40:01.113Z
tags: 
editor: markdown
dateCreated: 2022-11-24T12:38:08.010Z
---

# KVM详解（五）——KVM虚拟机镜像格式
## 一、虚拟机常用镜像格式介绍
目前，虚拟机的主流镜像格式有raw、cow、qcow、qcow2以及vmdk，下面，我就详细介绍一下这些主流的虚拟机镜像格式。

（一）raw格式
raw格式是一种很早的镜像格式，格式比较原始、简单，性能上也还不错。并且raw格式镜像的一个突出好处是它支持转换成其他格式的镜像，或者作为其他格式镜像转换的中间格式。但是，raw格式的一个突出缺点就是不支持快照。 CentOS6的虚拟机KVM和XEN默认使用raw格式。

（二）cow、qcow和qcow2格式
cow、qcow和qcow2是另外的镜像格式，cow格式、qcow格式目前已经逐步被qcow2格式所取代，qcow2格式是目前的一种主流镜像格式，性能上与raw格式相差无几，但是支持虚拟机快照。CentOS7的KVM虚拟机镜像默认使用qcow2格式。

（三）vmdk格式
vmdk是Vmware虚拟的镜像格式，其整体性能非常出色，稳定性和其他方面的能力很好，也支持快照。
