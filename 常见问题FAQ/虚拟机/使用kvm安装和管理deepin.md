---
title: 使用kvm安装和管理deepin
description: 
published: true
date: 2022-06-27T06:39:06.901Z
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

# 3. 创建虚拟机

依然按照上一篇的思路，我们这里开始创建虚拟机

![3](https://storage.deepin.org/thread/202203061503147387_image.png)

该对话框表示virt-manager管理工具，连接到哪个libvirtd服务（虚拟机管理服务）。我这里选择的是本地（名称是我自定义，可以修改），virt-manager也可以管理远程虚拟机。

![4](https://storage.deepin.org/thread/202203061505246831_image.png)

上图需要注意的地方：如果iso文件在比如nfs，nas，获取其他什么地方，当前deepin用户不具备此文件的读写权限时，安装虚拟机将会报错。所以强烈建议：把iso文件放在本地文件系统上，简单说，放在【下载】目录下吧

接下来是处理器和内存配置，如下图，我配置4核4G内存，应该够用，根据自己物理机配置选择即可。

![5](https://storage.deepin.org/thread/202203061507385429_image.png)

接下来是虚拟磁盘的大小设置和存放位置，如下图，我们选择自定义。

如果选择默认，即勾选【为虚拟机创建磁盘镜像】，然后设置个大小。这样也可以，但是这根自定义有什么区别呢？区别在于：上面这个选择，会立刻给虚拟机分配你设置文件的大小，而下面自定义，只是预分配设置的大小，磁盘文件是个稀疏文件（自行查询了解）。当然磁盘非常大的小伙伴直接忽略吧

点击【前进】，出现如下图所示，按照标注操作。

![4](https://storage.deepin.org/thread/202203061505246831_image.png)

上图需要注意的地方：如果iso文件在比如nfs，nas，获取其他什么地方，当前deepin用户不具备此文件的读写权限时，安装虚拟机将会报错。所以强烈建议：把iso文件放在本地文件系统上，简单说，放在【下载】目录下吧

接下来是处理器和内存配置，如下图，我配置4核4G内存，应该够用，根据自己物理机配置选择即可。

![5](https://storage.deepin.org/thread/202203061507385429_image.png)

接下来是虚拟磁盘的大小设置和存放位置，如下图，我们选择自定义。

如果选择默认，即勾选【为虚拟机创建磁盘镜像】，然后设置个大小。这样也可以，但是这根自定义有什么区别呢？区别在于：上面这个选择，会立刻给虚拟机分配你设置文件的大小，而下面自定义，只是预分配设置的大小，磁盘文件是个稀疏文件（自行查询了解）。当然磁盘非常大的小伙伴直接忽略吧

![6](https://storage.deepin.org/thread/202203061508415356_image.png)


