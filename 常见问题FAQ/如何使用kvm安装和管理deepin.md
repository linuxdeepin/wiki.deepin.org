---
title: 如何使用kvm安装和管理deepin
description: 本篇主要介绍在Linux平台下，使用KVM虚拟化套件，来创建和安装deepin操作系统。
published: true
date: 2022-06-23T03:09:48.173Z
tags: deepin, kvm
editor: markdown
dateCreated: 2022-06-23T03:09:48.173Z
---

# 使用kvm安装和管理deepin
## 1. 配置虚拟化环境
无论在哪个操作系统上使用虚拟化环境，首先最基本的要求就是BIOS里面开启了虚拟化技术。这个部分略。

本篇在deepin操作系统安装虚拟化环境，只要命令行执行以下命令即可：

`sudo apt install qemu-kvm virt-manager`

等待命令执行完成之后，可以在启动器看到一个虚拟机管理应用图标，如下：

image.png

点击【虚拟系统管理器】，会出现一个授权认证对话框，输入deepin个人账号密码，即可。但频繁授权挺繁琐，可取消虚拟系统管理授权。方法如下：
`sudo vim /usr/share/polkit-1/actions/org.libvirt.unix.policy`
内容修改如下：
```vim
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
```
image.png

## 2. 创建虚拟机

image.png

该对话框表示virt-manager管理工具，连接到哪个libvirtd服务（虚拟机管理服务）。我这里选择的是本地（名称是我自定义，可以修改），virt-manager也可以管理远程虚拟机。

点击【前进】，出现如下图所示，按照标注操作。

image.png

上图需要注意的地方：如果iso文件在比如nfs，nas，获取其他什么地方，当前deepin用户不具备此文件的读写权限时，安装虚拟机将会报错。所以强烈建议：把iso文件放在本地文件系统上，简单说，放在【下载】目录下吧

接下来是处理器和内存配置，如下图，我配置4核4G内存，应该够用，根据自己物理机配置选择即可。

image.png、

接下来是虚拟磁盘的大小设置和存放位置，如下图，我们选择自定义。

如果选择默认，即勾选【为虚拟机创建磁盘镜像】，然后设置个大小。这样也可以，但是这根自定义有什么区别呢？区别在于：上面这个选择，会立刻给虚拟机分配你设置文件的大小，而下面自定义，只是预分配设置的大小，磁盘文件是个稀疏文件（自行查询了解）。当然磁盘非常大的小伙伴直接忽略吧。

image.png

点击【管理】，出现如下对话框：

image.png

点击+号，开始创建虚拟磁盘，出现以下对话框：

image.png

按照上面的配置，点击完成即可，出现以下对话框

image.png、

点击选择卷即可

image.png

点击【前进】出现虚拟机命名界面，如下图，我们命名为deepin20

image.png

至此，虚拟机已经创建完成，可以开始安装操作系统了

本节以下内容为个人操作系统，与上面操作无任何影响：

image.png

如上图，我勾选了在安全前自定义配置，主要想进行两方面的配置

配置网卡的MAC地址
配置VNC连接操作
image.png

然后点击显示协议，配置如下图

image.png

这两项配置完成以后，点击【应用】，就可以点击左上角的【开始安装】，进行操作系统安装啦！

## 3. 安装操作系统
点击【开始安装】，就开始进入到deepin的安装界面

image.png

这个过程跟《deepin安装系列(1)】使用VMware workstation 安装和管理deepin》一样，可完全参考，为节省篇幅，此处省略安装过程

## 4. 用virsh的快照管理命令来管理deepin
在第4小节中，我们完成了deepin的安装和基本配置。现在进入到了deepin登录界面。我们同样在进行任何操作之前可以对deepin使用快照管理的方式，保证我们能够对deepin虚拟机进行版本控制。这样做的好处就是能够回滚到我们保存的某个快照，重新拉一个时间线，继续倒腾。不至于重新安装虚拟机。

本节主要讨论的是虚拟机关闭情况下，创建快照。在虚拟机启动情况下使用快照，有些问题。

VMware workstations对这一块做的就比较直接易操作

#### 4.1 试验virt-manager快照管理功能
首先我们创建一个如下的虚拟机状态，比如我们什么都没有配置

image.png

然后关闭虚拟机，通过virt-manager菜单栏的【查看】-【快照】，能够看到左下角有操作选项

image.png

创建一个快照，然后命名，添加一些描述、如图

image.png

点击完成，就可以看到有些内容显示了

image.png

然后启动虚拟机，在桌面创建一个文件夹

image.png

再关闭虚拟机。之后选择，从快照启动

image.png

上面会提示，是否从快照启动。选择是。然后再启动虚拟机，查看到虚拟机已经是快照的状态，无新建文件夹存在

image.png

#### 4.2 试验virsh 快照管理功能
这里不再试验，仅提供一些命令，需要注意的是，使用快照最好是在虚拟机关机情况下进行，否则会报错：

sudo virsh snapshot-create-as deepin20 deepin20_001 创建deepin20的快照deepin20_001
sudo virsh snapshot-revert deepin20 deepin20_001 将deepin20的状态恢复到快照deepin20_001的状态
sudo virslit snapshot-list 查看当前全部的快照
sudo snapshot-delete deepin20 deepin20_001 删除deepin20的某个快照
虚拟机不关机情况下的快照管理，等我摸索好了，再更新补充吧

经过测试，在centos上部署的kvm，比如centos7，通过virsh snapshot-create-as/snapshot-revent来进行虚拟机的快照创建和恢复，不需要关闭虚拟机即可操作。

建议阶段性的折腾虚拟机，在开机之前，都是用virt-manager做个快照，添加一个说明吧、

当然虚拟机克隆的功能也是可以使用的，把阶段性的虚拟机状态保存为克隆模板，不断迭代或者回滚，都是极好的选择。主要看个人爱好和习惯吧。