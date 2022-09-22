---
title: deepin系统配置与技巧汇总
description: 
published: true
date: 2022-07-28T09:31:18.255Z
tags: 
editor: markdown
dateCreated: 2022-07-28T09:31:15.283Z
---

# deepin系统配置与技巧汇总
## Super+S切换工作区
Super键就是WIN键。
这个工作区快捷切换键最初由Ubuntu发行版引入。
目前Deepin只能简单的将多个工作区放在一行横向排列。
缺少智能的多行自适应

## 善用强大的Super键
- Super+S ：显示工作区
- Super+W ：显示当前工作区的窗口
- Super+A ：显示所有工作区的窗口
- Super ：启动器
- Super+D ：显示桌面
- Super+E ：文件管理器
- Super+L ：锁屏

## 启动器搜索支持拼音
打开启动器页面后，键盘输入“xk”，将显示“显卡驱动管理器”，支持模糊匹配。
简拼、全拼都可以。

## “自然滚动”与macOS、Win10一致体验
控制中心-鼠标：打开“自然滚动”选项，可实现笔记本电脑触控板双指上下滚动页面时与手机、macOS、Windows 10保持相同体验。

## Chrome、VSCode使用自定义标题栏
系统原生标题栏很难看，和Chrome和VSCode自身的风格非常不协调。
Google Chrome浏览器可以在标题栏右键，去掉“使用系统标题栏和边框”。
VSCode可以在“文件-首选项-设置-窗口-Title Bar Style”中选择“custom”。
这样就美观多了。

## 系统快照，折腾无忧
首先从应用商店搜索安装Timeshift，通过为系统制作快照，让系统还原到你想要的时间点。
创建快照前先做一些设定。
位置：选择/和/home之外的分区，推荐“数据盘”
用户：建议对当前用户选择“包含隐藏文件”
筛选：可随意添加，-号代表排除，+号代表添加。同一目录“排除在前，添加在后！”
例如我的筛选列表如下：

- /home/loaden/.deepinwine
- /home/loaden/.minecraft
- /home/loaden/.wine
+ /home/loaden/.**
- /root/**
配置完成后即可创建快照。
第一次快照需要完全备份，时间比较长，请耐心等待。之后凡是没修改的文件都只建立软链接，速度就非常快了。
如果进不去桌面，可以在命令行下恢复。
查看：sudo timeshift --list
还原：sudo timeshift --restore
更多用法：timeshift --help

## APT软件包管理扩展用法
大多数的软件安装、卸载都应该在深度系统“应用商店”里可视化操作。下面以软件包“package-name”为例，总结部分命令行扩展用法：

- 查找软件：apt search package-name
- 重新安装软件：sudo apt install --reinstall package-name
- 安装并修复依赖关系：sudo apt install -f package-name
- 卸载软件同时清除系统配置：sudo apt purge package-name
- 自动卸载不需要的软件并清除配置：sudo apt install -f package-name
- 查看已安装软件包版本：apt list package-name
- 列出并筛选软件包：apt list |grep qt5
- 列出所有已安装软件包：apt list --installed
- 查看软件包内文件明细：dpkg -L package-name

## 找到命令所在路径和所属软件包
- 以深度系统“编辑器”为例：which deepin-editor
- 输出：/usr/bin/deepin-editor
- 查询：dpkg -S /usr/bin/deepin-editor
- 输出：deepin-editor: /usr/bin/deepin-editor 冒号之前的“deepin-editor”就是软件包。
## 假死后安全重启系统
同时按住Ctrl+Alt+SysRq+B，可实现安全重启，其中SysRq就是Print Screen键。
适合假死或黑屏时重启系统。
注意有Fn功能键的，视配置可能需要同时按住Fn功能键。
经测试，Alt+SysRq+B可以达到相同的效果。

## 更换主机名
sudo deepin-editor /etc/hostname
替换成新的主机名，重启电脑。

## 进程相关命令
- pgrep XXX #查询进程ID
- ps -ef |grep XXX #显示所有进程信息，连同命令行
- ps -aux |grep XXX #列出目前所有的正在内存当中的程序
- 可以用 | 管道和 more 连接起来分页查看：ps -aux |more
- sudo netstat -tulpn | grep :21 #查询端口
- pkill XXX #杀掉
- killall XXX #全杀
## 制作USB启动盘
```
sudo dd if=/path/to/the/downloaded/iso of=/path/to/the/USB/device
```
显示进度：
```
sudo dd if=/path/to/the/downloaded/iso of=/path/to/the/USB/device status=progress
```

## Live模式修复GRUB引导
打开Terminal，或者进入Shell，执行“sudo -i && fdisk -l”找到系统安装分区sdaX，然后按顺序执行：
```
mount /dev/sdaX /mnt
mount --bind /dev /mnt/dev
mount --bind /proc /mnt/proc
mount --bind /sys /mnt/sys
chroot /mnt
update-grub
grub-install /dev/sda
exit
```
不同硬盘酌情修改。

## 安装运行Windows绿色软件
进入绿色软件所在目录，右键“在终端中打开”，执行：
deepin-wine Windows绿色软件.exe

为大型Windows软件创建独立运行环境
建议每个大型软件使用一个独立的容器（运行环境），下面以安装Rosetta Stone为例。
第一步：拷贝或者创建容器
```
cp -r ~/.deepinwine/Deepin-QQ ~/.rosetta 或者 WINEPREFIX=~/.rosetta deepin-wine winecfg 第二步：安装
WINEPREFIX=~/.rosetta deepin-wine RosettaStone5.0.37.exe 如遇安装或者运行异常，可调试：
WINEDEBUG=+pid,+tid,+process WINEPREFIX=~/.rosetta deepin-wine $HOME'/.rosetta/drive_c/Program Files/Rosetta Stone/Rosetta Stone Language Training/Rosetta Stone.exe' 参考官方维基：https://wiki.deepin.org/wiki/Deepin-wine
```
注意：64位程序需要安装wine64，效果不佳。

## 硬盘或分区克隆及恢复
推荐使用Clonezilla（再生龙）：http://www.clonezilla.org/
dd命令底层低效，仅供备用：

- 备份：sudo dd if=/dev/sda1 of=~/elf.bak status=progress
- 还原：sudo dd if=~/elf.bak of=/dev/sda1 status=progress
- 压缩备份：sudo dd if=/dev/sda1 status=progress | gzip -c > ~/elf.bak
- 压缩还原：gunzip -c ~/elf.bak | sudo dd of=/dev/sda1 status=progress
## 备份还原MBR和分区表
- 备份MBR：sudo dd if=/dev/sdX of=/path/to/mbr bs=512 count=1
- 还原MBR：sudo dd if=/path/to/mbr of=/dev/sdX bs=512 count=1
- MBR只还原分区表：sudo dd if=/path/to/mbr of=/dev/sdX bs=66 skip=446 count=1
- MBR清空引导记录：sudo dd if=/dev/zero of=/dev/sdX bs=446 count=1
- 备份分区表：sudo sfdisk -d /dev/sdX > /path/to/fqb
- 还原分区表：sudo sfdisk /dev/sdX < /path/to/fqb
## 开机画面卡死或者黑屏
grub引导菜单界面，按e进入编辑模式，修改“splash quiet”所在行，在其后尝试添加：
acpi_osi=! acpi="windows 2009"
F10启动。
原理：据说是因为有些老旧的BIOS无法识别高版本的Linux内核，所以grub加上这个参数就可以欺骗BIOS让它以为系统是windows7，然后就可以正常启动了。
如果还无法成功，可以尝试以下参数：
nomodeset 或
nouveau.modeset=0

## 关闭错误报警蜂鸣声
检查模块是否启动：lsmod | grep pcspkr
临时关闭：sudo rmmod pcspkr
永久关闭：
方法一：
deepin-editor ~/.profile 添加下面三行内容
sudo -S rmmod pcspkr <<EOF
password(你的密码)
EOF
在Shell脚本中，通常将EOF与 << 结合使用，表示后续的输入作为子命令或子Shell的输入，直到遇到EOF为止，再返回到主Shell,即将‘你的密码’当做命令的输入。
方法二：
sudo deepin-editor /etc/modprobe.d/blacklist.conf 添加 blacklist pcspkr
经测试，虽然pcspkr不再自启动，但电脑启动时会发出奇怪噪音，错误报警声仍然存在。
备注：
如果开机画面屏幕输出”Driver 'pcspkr' is already registered“则可用方法二解决。

## 测试显卡性能
可以通过在终端执行FPS测试命令，判断自己显卡驱动是否正常工作：
vblank_mode=0 glxgears
一般FPS能达到3000以上，就说明显卡驱动能正常工作了。
官方维基：https://wiki.deepin.org/index.php?title=%E6%98%BE%E5%8D%A1&language=en#.E7.AE.80.E4.BB.8B
NVIDIA显卡安装闭源驱动后，vblank_mode=0选项无效，需要从自带的配置程序中取消取消”垂直同步刷新“功能。
sudo apt install nvidia-settings
nvidia-settings