---
title: 使用kvm安装和管理deepin
description: 
published: true
date: 2022-06-27T06:36:57.001Z
tags: kvm
editor: markdown
dateCreated: 2022-06-27T06:20:43.293Z
---

# 使用kvm安装和管理deepin 

## 1.概述  

在Linux操作系统平台，我们可以使用的虚拟化软件有很多选择。主流包括：VMware workstations，Virturl Box，KVM等。

在上一篇分享中《【deepin安装系列(1)】使用VMware workstation 安装和管理deepin》，我是在window平台下使用VMware workstations虚拟化软件，来创建虚拟机，安装deepin操作系统。本篇分享主要介绍在Linux平台下，使用KVM虚拟化套件，来创建和安装deepin操作系统。

## 2. 配置虚拟化环境

无论在哪个操作系统上使用虚拟化环境，首先最基本的要求就是BIOS里面开启了虚拟化技术。这个部分略。

本篇在deepin操作系统安装虚拟化环境，只要命令行执行以下命令即可：

```
sudo apt install qemu-kvm virt-manager
```

等待命令执行完成之后，可以在启动器看到一个虚拟机管理应用图标，咯：

![1](https://storage.deepin.org/thread/202203061458102790_image.png)

点击【虚拟系统管理器】，会出现一个授权认证对话框，输入deepin个人账号密码，即可如下界面

<policyconfig>
    <action id="org.libvirt.unix.monitor">
      <description>Monitor local virtualized systems</description>
      <message>System policy prevents monitoring of local virtualized systems</message>
      <defaults>
        <!-- Any program can use libvirt in read-only mode for monitoring,
             even if not part of a session -->
        <allow_any>yes</allow_any>
        <allow_inactive>yes</allow_inactive>
        <allow_active>yes</allow_active>
      </defaults>
    </action>

    <action id="org.libvirt.unix.manage">
      <description>Manage local virtualized systems</description>
      <message>System policy prevents management of local virtualized systems</message>
      <defaults>
        <!-- Any program can use libvirt in read/write mode if they
             provide the root password -->
        <allow_any>yes</allow_any>
        <allow_inactive>yes</allow_inactive>
        <allow_active>yes</allow_active>
      </defaults>
    </action>
</policyconfig>

![2](https://storage.deepin.org/thread/202203061501145541_image.png)

