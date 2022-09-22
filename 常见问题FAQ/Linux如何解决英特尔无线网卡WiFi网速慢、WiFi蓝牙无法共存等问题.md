---
title: Linux如何解决英特尔无线网卡WiFi网速慢、WiFi蓝牙无法共存等问题
description: 
published: true
date: 2022-06-28T02:33:38.487Z
tags: wifi 蓝牙 无线网卡
editor: markdown
dateCreated: 2022-06-28T02:33:36.460Z
---

# Linux如何解决英特尔无线网卡WiFi网速慢、WiFi蓝牙无法共存等问题
使用本教程前，请先确认你机器里有英特尔无线网卡。该教程只适用于英特尔无线网卡。其他无线网卡修改`iwlwifi.conf`没有任何效果！

无线网卡的型号可以在“设备管理器”应用中查看，或者使用如下命令查看：

```perl
lspci | grep -i wireless
```

比如我的输出为：

```makefile
03:00.0 Network controller: Realtek Semiconductor Co., Ltd. RTL8822CE 802.11ac PCIe Wireless Network Adapter
```

说明我的无线网卡是瑞昱的，不是英特尔的，不需要使用该教程，用了也毫无效果。

如果你的输出带有“Intel”字样，则可以使用该教程。

------

`/etc/modprobe.d/iwlwifi.conf`里保存了英特尔无线网卡的参数，不同网卡需要设置不同参数，才能达到最佳工作状态。但是，Linux系统通常无法帮你配置为最佳状态，因为配置不当可能会导致你完全无法连接任何无线网络，或者网速变成龟速。而不同无线网卡所需的最佳配置是不同的！

无线网卡的Linux驱动不是英特尔编写的，英特尔也不会配合Linux发行版进行参数调试。所以，Linux发行版通常只会采用“大多数无线网卡都能用”的“保守设置”，这会限制某些高性能无线网卡的网速，但是也可以确保另一些低性能无线网卡不至于彻底断网。要获得最佳性能，你必须自行修改参数。

下面介绍该文件的结构。比如，UOS中该文件的默认内容如下（可以通过`cat /etc/modprobe.d/iwlwifi.conf`看到）：

```undefined
options iwlwifi 11n_disable=1 bt_coex_active=0 power_save=0 swcrypto=1
```

该文件的结构如下：

- 开头是固定的`options iwlwifi`，后面接一系列可选参数。
- 参数格式为`名称=值`，由空格分隔。
- 如果参数没有给出，相当于给出了“默认”值。所以如果你只想将参数设为默认值，则不必给出参数。

可选参数如下（可以通过`modinfo iwlwifi | grep parm`命令获得，仅翻译可能需要调整的项目。未翻译项目请自行探索其用途）：

- `11n_disable`: 802.11n（WiFi4）功能开关，bitmap类型：0 完全启用WiFi4功能，1 完全禁用WiFi4功能（WiFi严重变慢），2 禁用发送链路聚合，4 禁用接收链路聚合，6 禁用发送和接收链路聚合，8 强制启用发送链路聚合。 (建议值 0 [完全启用WiFi4功能]。如 0 效果不佳，建议 8 [强制启用发送链路聚合]。**发行版把该参数设为1就是很多人WiFi很慢的原因**，但是发行版有理由这么做，因为设为0的话，另一些人可能会断网。所以建议你先尝试其他值，效果均不佳再考虑设为1。)
- `bt_coex_active`: 蓝牙/WiFi共存开关：1 蓝牙/WiFi可以同时开启。0 蓝牙/WiFi不可同时开启。 (默认: 1 [可以同时开启]，建议: 1 [可以同时开启]。但如果开启蓝牙后WiFi严重变慢，则可能应该设为0。)
- `power_save`: 1 启用WiFi省电模式，0 禁用WiFi省电模式 (默认: 0 [禁用省电模式]，建议: 0 [禁用省电模式])
- `swcrypto`: 1 使用软件加密，0 使用硬件加密 (默认 0 [硬件加密]，建议 0 [硬件加密]。但如果设为0后断网，则应该设为1。)
  对该选项的粗浅解释：无线网卡可以负责和WiFi密码有关的工作，但是如果你想，你也可以让CPU负责。交给无线网卡处理可以更快，但是如果不兼容，那只能CPU接管。
- `disable_11ac`: 802.11ac（WiFi5）功能开关：0 启用WiFi5，1 禁用WiFi5 (默认: 0 [启用WiFi5]，建议: 0 [启用WiFi5]。不支持5GHz的网卡无需关注该选项。)
- `disable_11ax`: 802.11ax（WiFi6）功能开关：0 启用WiFi6，1 禁用WiFi6 (默认: 0 [启用WiFi6]，建议: 0 [启用WiFi6]。不支持WiFi6的网卡无需关注该选项。)
- `amsdu_size`: amsdu size 0: 12K for multi Rx queue devices, 2K for AX210 devices, 4K for other devices 1:4K 2:8K 3:12K 4: 2K (default 0) (int)
- `fw_restart`: restart firmware in case of error (default true) (bool)
- `nvm_file`: NVM file name (charp)
- `uapsd_disable`: disable U-APSD functionality bitmap 1: BSS 2: P2P Client (default: 3) (uint)
- `enable_ini`: Enable debug INI TLV FW debug infrastructure (default: true (bool)
- `led_mode`: 0=system default, 1=On(RF On)/Off(RF Off), 2=blinking, 3=Off (default: 0) (int)
- `power_level`: default power save level (range from 1 - 5, default: 1) (int)
- `remove_when_gone`: Remove dev from PCIe bus if it is deemed inaccessible (default: false) (bool)

所以UOS给出的默认参数就非常糟糕了，具体如下：

- `11n_disable=1`禁用了WiFi4，所以整个WiFi不管怎么样都会很慢。
- `bt_coex_active=0`使蓝牙和WiFi不能同时开启。
- `power_save=0`关了省电模式，这很好，没问题。
- `swcrypto=1`关了硬件加密，不仅占用了更多CPU资源，还有让WiFi变慢的风险。

所以，更合理的设置是：

```undefined
options iwlwifi 11n_disable=0 bt_coex_active=1 power_save=0 swcrypto=0
```

- 启用WiFi4
- 允许WiFi/蓝牙共存
- 关闭省电模式
- 使用硬件加密

通过以下命令可以修改为这个“更合理的设置”：

```bash
# 创建配置文件
sudo echo 'options iwlwifi 11n_disable=0 bt_coex_active=1 power_save=0 swcrypto=0' | sudo tee /etc/modprobe.d/iwlwifi.conf

# 更新initramfs让配置文件生效，不同的系统方法不同。如果忽略这一步，重启后配置文件可能不会生效。

# Debian, Ubuntu, Deepin, UOS
sudo update-initramfs -k all -u

# Fedora
sudo dracut --force --no-hostonly

# Archlinux
sudo mkinitcpio -P

# 最后重启，配置文件就会生效
```

注意`sudo`会询问你电脑的开机密码，输完回车即可，输入过程中不会有任何显示（就像输入没反应一样），这是终端的安全措施。

命令执行完成后重启生效。

注意每次修改`iwlwifi.conf`之后都需要执行`sudo update-initramfs -k all -u`并重启，否则更改可能不会生效。

如果重启后你的无线网卡连不上WiFi热点，或者出现其他问题，请尝试调整参数，比如改成：

```bash
# 创建配置文件
sudo echo 'options iwlwifi 11n_disable=8 bt_coex_active=1 power_save=0 swcrypto=0' | sudo tee /etc/modprobe.d/iwlwifi.conf

# 更新initramfs让配置文件生效，不同的系统方法不同。如果忽略这一步，重启后配置文件可能不会生效。

# Debian, Ubuntu, Deepin, UOS
sudo update-initramfs -k all -u

# Fedora
sudo dracut --force --no-hostonly

# Archlinux
sudo mkinitcpio -P

# 最后重启，配置文件就会生效
```

或者

```bash
# 创建配置文件
sudo echo 'options iwlwifi 11n_disable=0 bt_coex_active=0 power_save=0 swcrypto=1' | sudo tee /etc/modprobe.d/iwlwifi.conf

# 更新initramfs让配置文件生效，不同的系统方法不同。如果忽略这一步，重启后配置文件可能不会生效。

# Debian, Ubuntu, Deepin, UOS
sudo update-initramfs -k all -u

# Fedora
sudo dracut --force --no-hostonly

# Archlinux
sudo mkinitcpio -P

# 最后重启，配置文件就会生效
```

你可以上网搜索最适合你网卡的设置。

