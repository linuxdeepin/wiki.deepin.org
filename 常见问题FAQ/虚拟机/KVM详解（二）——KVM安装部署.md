---
title: KVM详解（二）——KVM安装部署
description: 
published: true
date: 2022-11-23T01:24:26.142Z
tags: 
editor: markdown
dateCreated: 2022-11-23T01:21:26.084Z
---

# KVM详解（二）——KVM安装部署
一、硬件设置
为了进行KVM的安装，我们首先进行一些硬件上的配置。
我们使用费Vmware的虚拟机，配置2个G的内存，同时新加一块30G的硬盘，以供KVM的虚拟机安装使用，然后再给虚拟机设置4个处理器，同时，打开处理器的虚拟化引擎，在虚拟化Intel、虚拟化CPU和虚拟化IOMMU等选项上打上勾，配置完成后的页面如下所示：

![2022-11-23_10458.png](/2022-11-23_10458.png)

上述配置完成后，我们执行命令：

```bash
cat /proc/cpuinfo | grep vmx
1
```

> 注意：
> 如果您的计算机时AMD的CPU，则应该执行命令：
> cat /proc/cpuinfo | grep svm

执行结果如下：

![2022-11-23_44171.png](/2022-11-23_44171.png)

可以看出，现在，我们的虚拟机CPU已经支持虚拟化了。
此外，为了之后KVM的运行，我们最好还要安装Linux的图形化页面，安装过程请见文章Linux桌面图形化安装详解，在该文章已经有了详细介绍，在今天就不过多赘述了。

二、KVM安装
接下来，我们开始进行KVM的安装。
执行命令：

```yum install qemu-kvm virt-manager libvert libguestfs-tools virt-install libvert-python```
1
上述6个软件作用如下：
1、qemu-kvm
KVM的主程序，KVM的虚拟化模块。
2、virt-manager
KVM的图形化管理工具。
3、libvert
虚拟化服务。
4、libguestfs-tools
虚拟机的系统管理工具。
5、virt-install
安装虚拟机的使用工具，内含一些实用命令，如virt-clone等。
6、libvert-python
python调用libvert虚拟化服务的api接口库文件。
在上述软件安装完毕后，我们执行命令：

`systemctl start libvirtd`

即可开启KVM虚拟化服务。
执行命令：

`lsmod | grep kvm`

执行结果如下所示：

![2022-11-23_41188.png](/2022-11-23_41188.png)

执行命令：

```bash
virt-manager
```

即可打开KVM的虚拟系统管理器了，结果如下所示：

![2022-11-23_9807.png](/2022-11-23_9807.png)

三、KVM网络桥接功能设置
在KVM安装完毕后，我们进行网络桥接功能的设置，以方便我们的KVM虚拟机在创建后，可以通过桥接的方式来进行上网。KVM的虚拟机已经默认支持使用NAT的方式来进行上网，如果我们不进行这部分的配置，那么我们的KVM虚拟机也可以通过NAT的方式来上网，但是不能通过桥接的方式来上网。
首先，我们备份一下我们的网卡文件（我的是/etc/sysconfig/network-scripts/ifcfg-ens32，其他设备的也在该目录下，但是文件名可能略有差异）。我们打开我们的网卡文件，删除有关IP地址、子网掩码、网关、DNS等的配置，即下图所示的红圈中的内容：
