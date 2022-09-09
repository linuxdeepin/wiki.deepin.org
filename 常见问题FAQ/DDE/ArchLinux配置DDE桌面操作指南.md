---
title: ArchLinux配置DDE桌面操作指南
description: 
published: true
date: 2022-09-09T03:30:10.804Z
tags: arch dde
editor: markdown
dateCreated: 2022-09-08T09:54:52.738Z
---

# 一、Arch Linux简介
![2022-9-8_45498.png](/2022-9-8_45498.png)

Arch Linux 是一个独立开发的 x86-64 通用 GNU/Linux 发行版，其用途广泛，足以适应任何角色。开发侧重于简单、极简主义和代码优雅。Arch 是作为一个最小的基础系统安装的，由用户配置，通过仅安装其独特目的所需或所需的东西来组装他们自己的理想环境。官方没有提供 GUI 配置实用程序，大多数系统配置是通过编辑简单的文本文件从 shell 执行的。Arch 努力保持领先，通常提供大多数软件的最新稳定版本。

Arch Linux 使用自己的 `Pacman` 包管理器，它将简单的二进制包与易于使用的包构建系统结合在一起。这允许用户轻松管理和自定义包，从官方 Arch 软件到用户自己的个人包，再到来自 3rd 方来源的包。存储库系统还允许用户轻松构建和维护自己的自定义构建脚本、包和存储库，从而鼓励社区发展和贡献。

最小的 Arch 基础包集位于精简的` [core] `存储库中。此外，官方的`[extra]`、`[community]`、`[testing]`仓库提供了数千个高质量的包来满足您的软件需求。Arch 还提供 Arch Linux 用户存储库 (AUR)，其中包含超过 49,000 个构建脚本，用于使用 Arch Linux makepkg 应用程序从源代码编译可安装包。

Arch Linux 使用“滚动发布”系统，允许一次性安装和永久软件升级。通常不需要将 Arch Linux 系统从一个“版本”重新安装或升级到下一个“版本”。通过发出一个命令，Arch 系统就可以保持最新状态并处于最前沿。

Arch 努力使其软件包尽可能接近原始上游软件。补丁仅在必要时应用，以确保应用程序与安装在最新 Arch 系统上的其他软件包一起正确编译和运行。

Arch Linux 是一个多功能且简单的发行版，旨在满足有能力的 Linux® 用户的需求。它功能强大且易于管理，使其成为服务器和工作站的理想发行版。把它带到你喜欢的任何方向。

```Linux发行版=Linux内核+包管理工具+桌面环境```

从表面来看，各个Linux发行版不同之处在于发行版集成的工具、包管理工具以及软件仓库的不同，而不是桌面环境（DE）的不同，当然还有稳定性、服务等方面的不同。

常见的Linux发行版有Ubuntu、Fedora、openSUSE、Debian、Mint、CentOS、Arch、Gentoo、Deepin

![2022-9-8_21062.jpg](/2022-9-8_21062.jpg)



# 三、Arch Linux下载

## 官网镜像下载地址

https://archlinux.org/releng/releases/  

![2022-9-8_13335.png](/2022-9-8_13335.png)

![2022-9-8_31037.png](/2022-9-8_31037.png)

# 四、安装注意事项及步骤
## 注意事项
- 安装 `ArchLinux` 需要联网;
- `mount` 挂载是将 硬盘里分区 挂载到 **live 环境**
- `chroot` 是在 **live 环境** 和 **安装到磁盘的系统** 之间切换
- 设置了分区表之后, 还需要格式化
- 没有网络,需要安装 `NetworkManager` 和 执行 `dhcpcd`(实体机需要）
- 磁盘的分区表有两种方式 `uefi + gpt + efi` 或 `legacy + mbr`
- 安装启动器有：`grub(bios 启动)` 或 `grub + efibootmgr(EFI 启动)`

## 安装思路梳理
1. 清空一个 **磁盘**
2. 对 **磁盘** 设置 **分区表**
3. 对 **子分区** 选择适合的 **TYPE**；
4. 选用各自的 **文件类型** 格式化 **子分区** ；
5. 下载 `archLinu_xxxxx.iso` 镜像
6. 设置第一启动项
7. 开机自动进入 **live 环境**
8. 将 **磁盘** 挂载
9. 换源
10. 安装 **基本包**（含内核等）
11. 配置 `fstab` 后，`chroot` 切换操作权
12. 安装 **必须软件包** ，设置 `Locale`
13. 设置 `root` 密码，和新建一个用户
14. 安装引导 `grub`
15. 退出和重启系统
16. 开启 `NetworkManager` 服务自动联网
17. 安装桌面环境 `DDE`
19. 详细的个性化配置
20. 享受 `ArchLinux` 的快乐

# 五、详细安装步骤
## 1.在VMware里面新建系统:


- 选择自定义(高级)，因为 `VM` 对 `ArchLinux` 没有直接支持； 选择 `Linux(L)` 时候选择选用 `Linux 5.x` 的内核版本

  ![img](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/20200730173637.png)

- 挂载 `ios` 镜像

  ![img](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/20200730173716.png)

- 设置为 `EFI` 方式启动

  ![img](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/image-20200730133901341.png)



启动虚拟机，默认选择第一个：

![img](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/image-20200730135432879.png)

等待一阵如下跳动

![img](https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic/2020/image-20200730135447732.png)

进入到 **live 环境** （此是内存条里面，加载的刚才的 ios 镜像系统），*注意此时 root 用户是 红色*

![2022-9-9_69077.png](/2022-9-9_69077.png)


## 2. 更新系统时钟

`timedatectl set-ntp true`



## 3. 磁盘分区
对已有分区表的一整块、未格式化的 磁盘，进行分区后得到2个 子分区，且对每一个 子分区 选择适合的 TYPE；

输入以下命令：

输入以下命令：

```bash
cfdisk

# 选择底部 gpt ，回车
# 选择 NEW ，回车，输入 1G，类型选择 EFI SYstem 格式
# 选择 NEW ，回车，输入 4G，类型选择 Linux swap 格式
# 选择 NEW ，回车，剩下的 35G，类型选择 默认 Linux filesystem 格式
# 选择 Write ， 输入 yes，回车 表示写入保存
# 选择 Quit ，退出
```

![2022-9-9_11887.png](/2022-9-9_11887.png)

![2022-9-9_92116.png](/2022-9-9_92116.png)

EFI分区格式化
```
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda3
```
创建swap分区：

```
mkswap /dev/sda2
swapon /dev/sda2
```

挂载分区：
```
mount /dev/sda3 /mnt
mkdir -p /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi

```

查看磁盘分区情况
```
lsblk -f
```
![2022-9-9_14915.png](/2022-9-9_14915.png)


## 4.更新为国内镜像源

```
reflector --country China --age 72 --sort rate --protocol https --save /etc/pacman.d/mirrorlist
```

将最新的镜像源更新为国内的，保存在/etc/pacman.d/mirrorlist目录下

也可以手动替换到“/etc/pacman.d/mirrorlistg”中
```
Server = https://mirrors.bfsu.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.cqu.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.dgut.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.neusoft.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.nju.edu.cn/archlinux/$repo/os/$arch
Server = https://mirror.redrock.team/archlinux/$repo/os/$arch
Server = https://mirrors.sjtug.sjtu.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.xjtu.edu.cn/archlinux/$repo/os/$arch
```

## 5.安装 Arch Linux
安装基本系统（包括linux内核以及基础软件包），这里相比参考文章多给了几个软件包，因为这些对用户来说还是比较重要的 ，有几种内核可以安装：

普通内核(linux linux-headers)
lts稳定版内核(linux-lts linux-lts-headers)
zen内核(linux-zen,linux-zen-headers)
按自己的需求安装就可以

这里需要提前说一下，linux-zen 内核不支持 nvidia 显卡，有这个需求的就别装了，如果是原版 linux 内核的话，就要做好随时滚挂的准备，最近的 5.18 内核更新就会导致 nvidia-5.15 版本驱动失效无法开机，如果你希望稳定使用，就选择 linux-lts 内核和linux-lts-headers，并安装相应的 nvidia-lts 驱动（后面会有详细说明），不过不用太担心，即便是系统安装完成，你也可以随时切换自己想要的内核版本。

LTS内核
`pacstrap /mnt base linux-lts linux-lts-headers linux-firmware base-devel `

普通内核
`pacstrap /mnt base linux linux-headers linux-firmware base-devel`

## 6.写入分区表：
`genfstab -U /mnt >> /mnt/etc/fstab`

## 7.进入新系统

`arch-chroot /mnt`

## 8.配置系统

pacman -S vim sudo vim ttf-dejavu lightdm xorg-server deepin-kwin deepin deepin-extra networkmanager

## 9.设置语言
输入“vim /etc/locale.gen”，删除前面的“#”，保存。
![2022-9-9_18468.png](/2022-9-9_18468.png)

```
locale-gen
echo LANG=en_US.UTF-8 >> /etc/locale.conf
```

## 10.设置root用户密码

设置root用户的密码

```
输入“passwd”，再输入密码，密码不会显示
```

## 11.创建新用户

执行如下命令，很坑的的一点，如果安装深度环境 DDE 的话，必须要新建用户
```
useradd -m -G wheel -s /bin/bash free
```
-m：创建用户主目录（/home/[用户名]）
-G：用户要加入的附加组列表；此处将用户加到wheel组中，之后可以给这个组执行sudo命令的权限
-s：指定了用户默认登录shell的路径，此处设置为bash的路径

设置密码：
```
passwd free
```
然后输入两次密码即可。提权， 修改 /etc/sudoers文件，删除wheel组前面的注释（#）即可：
```
Uncomment to allow members of group wheel to execute any command
%wheel ALL=(ALL) ALL
```

## 12.安装grub

EFI 启动方式, 需安装 grub 和 efibootmgr
`pacman -S grub efibootmgr`


然后，还需要将其安装到EFI分区当中：
`grub-install --recheck /dev/`   # 注意：此处的 /dev/sda 后没有数字

生成一个grub的配置文件
`grub-mkconfig -o /boot/grub/grub.cfg`

## 13.重启系统
```
exit
umount -R /mnt
reboot
```

## 14.启动网络服务
systemctl enable NetworkManager



参考资料：

1. https://wiki.archlinux.org/        Arch维基
2. https://wiki.archlinux.org/title/Installation_guide             Arch安装指南
3. [给 GNU/Linux 萌新的 Arch Linux 安装指南 rev.B](https://blog.yoitsu.moe/arch-linux/installing_arch_linux_for_complete_newbies.html)
4. [在VMWare上安装Arch Linux](https://www.cnblogs.com/freerqy/p/8502838.html)
5. [ArchLinux安装（Deepin v20桌面环境）](https://zhuanlan.zhihu.com/p/141067184)