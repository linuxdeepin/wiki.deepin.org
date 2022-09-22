---
title: linux休眠
description: 
published: true
date: 2022-07-05T12:57:44.651Z
tags: 休眠
editor: markdown
dateCreated: 2022-07-05T12:57:42.198Z
---

# linux 休眠

## 前言

休眠是一个很好用的功能，但是也是一个很容易出现问题的功能。

## 前提

休眠的原理是将内存的数据存放进硬盘里面，然后断电。开机后能够从硬盘里面还原数据到内存，然后就变成和关机之前一模一样的运行状态。当然其中还有很多细节调整，比如网络重连，时间的重设等。

休眠之所以容易出现问题，这是因为ACPI(高级配置和电源接口)对软硬件都是一项挑战，它要求硬件能支持休眠的电源管理调度，也要求软件驱动对其支持。

另外，linux对休眠功能的支持，要求你开启交换分区，内存的信息在休眠的时候将转储到交换分区之上。因此最好让你的交换分区和内存差不多大小。

## 背景知识

ACPI电源状态：

1. S0 ： 正常开机
2. S1 ： 待机（几乎所有硬件都需要维持供电，但进入停机状态）
3. S2 ： 
4. S3 ： 睡眠（挂起到内存，内存不能断电）
5. S4 ： 休眠（挂起到硬盘，可以断电，但是某些硬件驱动不支持S4就会阻止进入这个状态）
6. S5 ： 支持唤醒（支持网络唤醒功能，不能断电）

systemd 后台管理支持的电源功能：

1. `systemctl suspend` : S3
2. `systemctl hibernate`: S4
3. `systemctl suspend-then-hibernate`: S3一段时间后S4
4. `systemctl hybrid-sleep`: S4+S3

## 错误处理

我在5.1内核可以成功休眠，但是之后的5.2，5.3，5.4，5.4内核都无法成功休眠。一直认为内核升级就能修复这个问题，但迟迟等不到好的内核。因此对这个问题深入的研究一番：

```bash
sudo dmesg |grep -i acpi #查看内核输出信息中关于acpi的内容

#对相关信息进行梳理，发现比较可疑的内容
[  211.189078] ACPI: EC: event blocked
[  211.189079] ACPI: EC: EC stopped

# 貌似是被某种东西中断的休眠过程
```

我的硬件存在不少报错信息，应该是关于usb的：

```bash
sudo dmesg -l err # 输出错误信息
# 输出内容如下：
[    1.076121] pci 0000:00:00.2: AMD-Vi: Unable to write to IOMMU perf counter.
[    1.396121] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.POT5._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    1.396276] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.POT6._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    1.396423] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.POT7._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    1.396566] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.POT8._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    1.396699] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.POT9._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    1.396816] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.PO10._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    1.396943] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.PO11._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    1.397101] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.PO12._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    1.397641] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.PO13._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    1.397778] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.PO14._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    1.398752] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.POT1._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    1.398898] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.POT2._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    1.399358] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.POT3._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    1.399530] ACPI Error: Aborting method \_SB.PCI0.GPP1.PTXH.RHUB.POT4._PLD due to previous error (AE_AML_UNINITIALIZED_ELEMENT) (20190816/psparse-531)
[    4.579953] tpm_crb MSFT0101:00: can't request region for resource [mem 0xbb450000-0xbb453fff]

```

既然硬件存在问题，那么休眠的时候出现问题也很好理解，毕竟acpi是一个很高标准很严格的电源管理。

后来我在 arch wiki 里面找到一个方法，尝试了一下，发现居然可以解决我的问题：

```bash
sudo nano /etc/systemd/sleep.conf 

# 添加一行配置到里面
HibernateMode=shutdown
```

不知道原因，但是确实是解决我的问题，你如果遇到也可以试一下。


## 参考

1. stackExchange上类似的问题：<https://unix.stackexchange.com/questions/502235/system-fails-to-suspend-acpi-ec-interrupt-blocked>
2. arch wiki: <https://wiki.archlinux.org/index.php/Power_management/Suspend_and_hibernate>