---
title: Deepin系统运维
description: 
published: true
date: 2022-10-03T15:55:01.831Z
tags: 运维
editor: markdown
dateCreated: 2022-06-28T08:12:39.925Z
---

# 第一章Deepin系统运维

## 1.1桌面配置技巧

### 1.1.1 Super + S 切换工作区

<kbd>Super</kbd> 就是 <kbd>⊞</kbd> 键
这个工作区快捷切换键最初由Ubuntu发行版引入。
目前Deepin只能简单的将多个工作区放在一行横向排列。
缺少智能的多行自适应布局。

### 1.1.2 善用强大的 Super 键

- <kbd>Super</kbd> ：启动器
- <kbd>Super</kbd> + <kbd>S</kbd> ：显示工作区
- <kbd>Super</kbd> + <kbd>W</kbd> ：显示当前工作区的窗口
- <kbd>Super</kbd> + <kbd>A</kbd> ：显示所有工作区的窗口
- <kbd>Super</kbd> + <kbd>D</kbd> ：显示桌面
- <kbd>Super</kbd> + <kbd>E</kbd> ：文件管理器
- <kbd>Super</kbd> + <kbd>L</kbd> ：锁屏

### 1.1.3 启动器搜索支持拼音

打开启动器页面后，键盘输入“xk”，将显示“显卡驱动管理器”，支持模糊匹配。简拼、全拼都可。

### 1.1.4 自然滚动 与 macOS、Win10 一致

控制中心-鼠标：打开“自然滚动”选项，可实现笔记本电脑触控板双指上下滚动页面时与手机、平板、macOS、Win10 保持相同体。

### 1.1.5 Chrome、VSCode 使用 自定义标题栏

系统原生标题栏很难看，和Chrome和VSCode自身的风格非常不协调。
Google Chrome浏览器可以在标题栏右键，去掉`使用系统标题栏和边框`。
VSCode可以在“文件-首选项-设置-窗口-Title Bar Style”中选择`custom`。这样就美观多了。

### 1.1.6 恢复默认文件管

安装Visual Studio Code后，会发现在谷歌浏览器中下载文件后，如果点击“在文件夹中显示”时弹出Visual Studio Code窗口。
`解决办法：`在文件管理器里随意创建一个空文件夹，然后在这个文件夹上点击右键，从右键菜单里选择“打开方式”，把“文件管理器”设置为默认程序即可。此外，控制中心有常用的默认程序配置功

### 1.1.7 卸载搜狗输入法，改用 Google 拼音

由于搜狗输入法存在严重的内存泄露（开机大约8小时，内存占用将达到3G)，在方修复内存泄露之前，建议替换成fcitx输入法。卸载搜狗输入法：`sudo apt purge sogoupinyin`完成卸载后，一定要注销或者重启。杀掉所有fcitx进程`killall fcitx`确认这条命令没有任何输出了：`pgrep fcitx`
删除旧配置：

```bash
rm -r ~/.sogouinput
rm -r ~/.config/SogouPY*
rm -r ~/.config/sogou*
rm -r ~/.config/fcitx*
#安装Google拼音输入法：
sudo apt install fcitx-googlepinyin fcitx-ui-classic
#启动测试：
fcitx-autostart
#如无报错，成功
```

深入了解fcitx安装情况：`fcitx-diagno`

### 1.1.8 应用商店卸载软件

<kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>T</kbd> 打开终端，输入命令：`sudo apt install -f`回车执行后重新应用商店卸载即

### 1.1.9 修改文件扩

方法一：文件管理器，在要修改的文件上弹出右键菜单，”属性“窗口可完成文名、扩展名修改。
方法二：进入文件所在目录，右键”在终端中打开“，执行：`sudo mv filename.txt filename`

### 1.1.10 系统快照，折腾无忧

首先从应用商店搜索安装`Timeshift`，通过为系统制作快照，让系统还原到你要的时间点。折腾必备，强烈推荐！
创建快照前先做一些设定。
位置：选择/和/home之外的分区，推荐“数据盘”
用户：建议对当前用户选择“包含隐藏文件”
筛选：可随意添加，-号代表排除，+号代表添加。同一目录“排除在前，添加后！”例如我的筛选列表如下：

```
- /home/loaden/.steam
- /home/loaden/.deepinwine
- /home/loaden/.wine
- /home/loaden/.config/google-chrome

+ /home/loaden/.**

- /root/**
```

配置完成后即可创建快照。
第一次快照需要完全备份，时间比较长，请耐心等待。之后凡是没修改的文件都建立软链接，速度就非常快了。
如果进不去桌面，可以在命令行下恢复。
查看：`sudo timeshift --list`
还原：`sudo timeshift --restore`
更多用法：`timeshift --h`

### 1.1.11 删除不需要的文件打开

文件管理器进入主目录，<kbd>Ctrl</kbd> + <kbd>H</kbd> 显示隐藏文件，进入.confg目录，编mimeapps.list

### 1.1.12 ShadowSocks 代理上网

ShadowSocks软件自身配置完成后，还需要打开网络代理，才能科学上网。
方法一：手动代理控制中心-网络-系统代理-手动：SOCKS代理“127.0.0.1”,口“1080”。
方法二：自动代理控制中心-网络-系统代理-自动,配置URL“file:///homeloaden/.ss.pac”

```bash
sudo apt install python3-pip
sudo pip3 install genpacgenpac --pac-proxy "SOCKS5 127.0.01:1080" --gfwlist-proxy="SOCKS5127.0.0.1:1080" --output=~/.ss.pac
```

### 1.1.13 启动器隐藏不想看到的启动项

文件管理器打开系统盘，进入/usr/share目录，在applications目录上右键“管理员身份打开”。在不想看到的启动项图标上右键，“打开方式”选择“编辑器”添加：`NoDisplay=true` 保存。

### 1.1.14 启动器创建“我的世界”启

在应用商店里安装软件后，就可以在启动器里找到该软件的一个启动项，启动软变得非常方便。在启动项右键菜单上还提供了“发送到桌面”、“发送到任务栏”“开机自动启动”等简捷功能。

而所谓的启动项，本质就是一个后缀为“.desktop”的文件，可以在系统盘/usrshare/applications/里看到。

但是，在/usr/share/applications/里创建启动项并不是最佳的位置。
最佳位置在主目录 ~/.local/share/applications 里。

下面以创建Minecraft(我的世界)为例讲解。
首先用编辑器创建文件 ~/.local/share/applications/Minecraft.desktop,添加以下内容：

```ini
[Desktop Entry]
Categories=Game;
Comment=MinecraftExec=/home/loaden/Zz/Minecraft/Minecraft.sh
Icon=minecraft
Name=Minecraft
Name[zh_CN]=我的世界
Terminal=false
Type=Application
X-Deepin-Vendor=user-custom
```

特别注意1：这里所有的路径都必须是绝对路径，而且不识别$HOME，~这些常见的bash变量。

特别注意2：Icon虽然可以用绝对路径的图标，但不能自适应大小。推荐把图标拷贝到~/.local/share/icons/hicolor/的各个尺寸目录里，文件名为minecraft。

Minecraft.sh就是我的世界的启动脚本，内容如下：

```shell
#!/bin/bash
cd $HOME/Zz/Minecraft
java -jar HMCL.jar
```

HMCL启动器下载地址：[https://github.com/huanghongxun/HMCL](https://github.com/huanghongxun/HMCL)
需要安装Java运行时：`sudo apt install default-jre openjfx`

### 1.1.15 VirtualBox 支持 USB2.0/3.0 

1. 首先要安装扩展：`sudo apt install virtualbox-extension-pack`
2. 其次要添加用户组：`sudo adduser loaden vboxusers`
3. 重启生效。
4. 查看确认用户组：`groups loaden`
5. 从用户组中删除：`sudo deluser loaden vboxuse`

### 1.1.16 VirtualBox 虚拟机减肥

1. 关闭虚拟机休眠功能：`powercfg /h off`
2. 关闭系统还原3.磁盘碎片整理
3. 下载sdelete，执行：`sdelete -c -z C: D:`
4. 关闭虚拟机
5. `VBoxManage modifyhd --compact WINXP.vdi`

## 1.2 常见终端命令行

### 1.2.1 更换主机名

```bash
sudo deepin-editor /etc/hostname
```

替换成新的主机名，重启电脑生效。

### 1.2.2 找到命令所在路径和所属软件包

1. 查找编辑器所在路径：`which deepin-editor`
	输出“/usr/bin/deepin-editor”
2. 查询文件属于哪个包：`dpkg -S /usr/bin/deepin-editor`
	输出“deepin-editor: /usr/bin/deepin-editor”
	冒号之前的“deepin-editor”就是软件

### 1.2.3 APT 软件包管理扩展用法

大多数的软件安装、卸载都应该在深度系统“应用商店”里可视化操作。下面以软件包“package-name”为例，总结软件包相关的常用命令：

```bash
#查找软件
apt search package-name
#重新安装软件
sudo apt install --reinstall package-name
#安装并修复依赖关系
sudo apt install -f package-name
#卸载软件同时清除系统配置
sudo apt purge package-nam
#自动卸载不需要的软件并清除配置
sudo apt autoremove --purge
#查看已安装软件包版本
apt list package-name
#列出并筛选软件包
apt list |grep qt5
#列出所有已安装软件包
apt list --installed
#查看软件包内文件明细
dpkg -L package-name
```

### 1.2.4 批量编码转换

1. 安装影音转换软件：`sudo apt install libav-tools`
2. 创建运行脚本：`touch conv.sh`
3. 右键设置可执行权限，或者：`chmod +x conv.sh`
4. 用编辑器打开`conv.sh`，添加以下内容（参数可酌情修改）:

```bash
#!/bin/bash
for file in $(find . -type f -a -name '*.mov'); do
    avconv -i "$file" -c:v h264 -b:v 3000k -r:v 25 -c:a ac3 -b:a 192k -r:a 44100 -ac 2 "$ (expr "$file" : '\(.*\)\.mov').mp4"
    [ "x$?x" == "x0x" ] && rm "$file"
done
```

## 1.3 系统维护与技巧

### 1.3.1 硬盘或分区克隆及恢复

1. 推荐使用Clonezilla（再生龙）：http://www.clonezilla.org/
2. dd命令底层低效，仅供备用：
	备份：`sudo dd if=/dev/sda1 of=~/elf.bak status=progress`
	还原：`sudo dd if=~/elf.bak of=/dev/sda1 status=progress`
	压缩备份：`sudo dd if=/dev/sda1 status=progress | gzip -c  ~/elf.bak`
	压缩还原：`gunzip -c ~/elf.bak | sudo dd of=/dev/sda1 status=progress`

### 1.3.2 备份还原 MBR 和分区表

```bash
#备份MBR
sudo dd if=/dev/sdX of=/path/to/mbr bs=512 count=1
#还原MBR
sudo dd if=/path/to/mbr of=/dev/sdX bs=512 count=1
#MBR只还原分区表
sudo dd if=/path/to/mbr of=/dev/sdX bs=66 skip=446 count=1
#MBR清空引导记录
sudo dd if=/dev/zero of=/dev/sdX bs=446 count=1
#备份分区表
sudo sfdisk -d /dev/sdX  /path/to/fqb
#还原分区表
sudo sfdisk /dev/sdX < /path/to/fqb
```

### 1.3.3 解决与 Windows 时间不同步

```bash
sudo dpkg-reconfigure tzdata #时区设置
sudo hwclock --localtime --systohc #使用本地时间并写入主板
```

正常情况下，Deepin已经具备了网络时间同步功能，如一旦失效，则可以：

```bash
sudo apt install ntpdate #安装同步时间软件
sudo ntpdate-debian #网络时间同步
```

方法二：

```bash
timedatectl set-local-rtc 1 --adjust-system-clock
```

使用systemd启动之后，时间由`timedatectl`来管理，重启完成将硬件时间UTC改为CST。

`timedatectl set-local-rtc 0` 恢复UTC

查看：`cat /etc/adjtime`

### 1.3.4 删除不需要的旧内核

1. 查询所有内核：`dpkg --get-selections | grep linux`
2. 正在使用的内核不能删除：`uname -r`
3. 删除不需要的内核：`sudo apt purge XXX`

### 1.3.5 清除已卸载软件遗留配置

```shell
dpkg --get-selections | grep deinstall | sed 's/deinstall/\lpurge/' | sudo dpkg --set-selections; sudo dpkg -Pa
```

### 1.3.6 开机进入命令行

1. 开机进入命令行：`sudo systemctl disable lightdm`
2. 恢复开机进桌面：`sudo systemctl enable lightdm`

### 1.3.7 系统启动分析

1. 显示启动系统过程中用户态和内核态所花的时间：`systemd-analyze`
2. 显示每个启动项所花费的时间明细：`systemd-analyze blame`

### 1.3.8 假死后安全重启系统

同时按住 <kbd>Alt</kbd> + <kbd>SysRq</kbd> + <kbd>B</kbd>，可实现安全重启，其中 <kbd>SysRq</kbd> 就是Print Screen键。

适合假死或黑屏时重启系统。注意有 <kbd>Fn</kbd> 功能键的，视配置可能需要同时按住 <kbd>Fn</kbd> 功能键。

### 1.3.9 进程相关命令

```bash
pgrep XXX #查询进程IDps -ef |grep XXX #显示所有进程信息，连同命令行
ps -aux | grep XXX #列出目前所有的正在内存当中的程序
```

可以用 | 管道和 more 连接起来分页查看：

```bash
ps -aux | moresudo netstat -tulpn | grep :21 #查询端口
pkill XXX #杀掉
killall XXX #全杀
```

### 1.3.10 Live 模式修复 GRUB 引导

打开Terminal，或者进入Shell，执行`sudo -i && fdisk -l`找到系统安装分区sdaX，然后按顺序执行：

```bash
mount /dev/sdaX /mnt
mount /dev/sdaY /mnt/boot/efi
mount --bind /dev /mnt/dev
mount --bind /proc /mnt/proc
mount --bind /sys /mnt/sys
chroot /mnt
update-grubgrub-install /dev/sda
exit
```

不同硬盘酌情修改。

### 1.3.11 离线安装软件

当没有网络连接的电脑上需要安装某个软件时，可以使用下面的脚本下载该软件以及依赖包，在脱机电脑上执行：
`sudo dpkg -i *.deb`
如果上述安装命令最后报错，则需要执行：
`sudo apt install -f`

如果提示缺少软件包（依赖），则拷贝依赖包名（例如XXX），在有网络的电脑上执行：
`apt download XXX`

再拷贝到脱机电脑上双击安装即可。
批量下载脚本如下：

```bash
#! /bin/bash
pkg="$*"

function getDepends() {
    ret=\`apt-cache depends $1 | grep -i 依赖 | sed 's/(.*)//' | cut -d: -f2\`
    if [[ -z $ret ]]; then
        echo "$1 No depends"
        echo -n
    else
        # echo $ret
        # apt-cache depends $1 |grep -i 依赖
        # echo ''
        for i in $ret; do
            if [[ $(echo $pkg | grep -e "$i ") ]]; then
                # echo "$i already in set"
                echo -n
            elif [[ $i =~ '<' ]]; then
                echo "Drop $i"
            elif [[ "$i" != "libc6" && "$i" != "libcairo2" && !("$i" =~ "glib") && !("$i" =~ "gtk") && !("$i" =~ "font") ]]; then
                # echo "Download $i ing"
                pkg="$pkg $i"
                getDepends $i
            fi
        done
    fi
}

for j in $@; do
    getDepends $j
done

# echo $pkg
apt download $pkg -d -y
```

### 1.3.12 制作 USB 启动盘

```bash
sudo dd if=/path/to/the/downloaded/iso of=/path/to/the/USB/device
#显示进度
sudo dd if=/path/to/the/downloaded/iso of=/path/to/the/USB/devicestatus=progress
```

### 1.3.13 查看与同步 GPT 分区 UUID

1. 查看UUID方法一：`blkid`
2. 查看UUID方法二：`ls -l /dev/disk/by-uuid`
3. 同步：`sudo vi /etc/fst`

### 1.3.14 配置命令行默认 Python 版本

不推荐，可能导致某些依赖python的软件出现问题！在此仅作为示例，介绍`update-alternatives`的基本用法。

首先需要在系统中添加Python2.7、Python3.5的选项，默认为Python3.5（优先级20）

```bash
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 10
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.5 20
#显示
update-alternatives --display python
#切换
sudo update-alternatives --config python
```

## 1.4 Windows 软件配置

### 1.4.1 安装运行 Windows 绿色软件

进入绿色软件所在目录，右键“在终端中打开”，执行：
`deepin-wine Windows绿色软件.exe`

### 1.4.2 为大型 Windows 软件创建独立运行环境

建议每个大型软件使用一个独立的容器（运行环境），下面以安装Rosetta Stone为例。

```bash
#第一步：拷贝或者创建容器
cp -r ~/.deepinwine/Deepin-QQ ~/.rosetta
#或者
WINEPREFIX=~/.rosetta deepin-wine winecfg
#第二步：安装
WINEPREFIX=~/.rosetta deepin-wine RosettaStone5.0.37.exe
#如遇安装或者运行异常，可调试：
WINEDEBUG=+pid,+tid,+process WINEPREFIX=~/.rosetta deepin-wine $HOME'/.rosetta/drive_c/Program Files/Rosetta Stone/Rosetta StoneLanguage Training/Rosetta Stone.exe'
```

参考官方维基：[https://wiki.deepin.org/wiki/Deepin-wine](https://wiki.deepin.org/wiki/Deepin-wine)
注意：64位程序需要安装wine64，效果不佳。

### 1.4.3 修改 QQ 聊天窗口文字大小

如果觉得QQ聊天窗口字体太小了，可以打开终端，执行：

```bash
WINEPREFIX=~/.deepinwine/Deepin-QQ deepin-wine winecfg
```

弹出窗口中切换到“显示”页面，“屏蔽分辨率”增大dpi，确定重启。

### 1.4.4 双击运行 Windows 软件

进入主目录，文件管理器Ctrl+H显示隐藏文件，进入`.local/share/applications`目录。
创建文件：deepinwine.desktop，添加如下内容并保存：

```ini
[Desktop Entry]
Categories=System;Utility;
Exec=/usr/bin/deepin-wine %F
Name=Wine
Terminal=false
NoDisplay=true
MimeType=application/x-ms-dos-executable;
Type=Application
```

然后在Windows的exe可执行文件上右键，"打开方式“选择Wine作为默认程序。

### 1.4.5 适配绿色版 Photoshop CC

1.删除DeepinQQ旧容器：`rm -rf ~/.deepinwine/Deepin-QQ`
2.启动器运行QQ，等待出现登陆容器后再关闭
3.删除默认旧容器：`rm -rf ~/.wine`
4.借DeepinQQ容器可避免标题乱码：`cp -r ~/.deepinwine/Deepin-QQ ~/.wine`
5.修改为Windows 7系统：`deepin-wine winecfg`
6.安装Phoneshop CC绿色版（thx Ansifa）

## 1.5 疑难问题解答

### 1.5.1 开机画面卡死或者黑屏

grub引导菜单界面，按e进入编辑模式，修改`splash quiet`所在行，在其后尝试添加：

```
acpi_osi=! acpi="windows 2009"
```

<kbd>F10</kbd> 启动。原理：据说是因为有些老旧的BIOS无法识别高版本的Linux内核，所以grub加上这个参数就可以欺骗BIOS让它以为系统是windows7，然后就可以正常启动了。如果还无法成功，可以尝试以下参数：`nomodeset` 或 `nouveau.modeset=0`

### 1.5.2 关闭错误报警蜂鸣声

检查模块是否启动：`lsmod |grep pcspkr`

- 临时关闭：`sudo rmmod pcspkr`
- 永久关闭：
	- 方法一：
		```bash
		deepin-editor ~/.profile 添加下面三行内容
		sudo -S rmmod pcspkr <<EOF
		password(你的密码)
		EOF
		```
		在Shell脚本中，通常将EOF与 << 结合使用，表示后续的输入作为子命令或子Shell的输入，直到遇到EOF为止，再返回到主Shell,即将‘你的密码’当做命令的输入。
	- 方法二：
		`sudo deepin-editor /etc/modprobe.d/blacklist.conf` 添加 `blacklist pcspkr`

经测试，虽然pcspkr不再自启动，但电脑启动时会发出奇怪噪音，错误报警声仍然存在。

备注：
如果开机画面屏幕输出`Driver 'pcspkr' is already registered`则可用方法二解决。

### 1.5.3 显卡驱动与性能测试

可以通过在终端执行FPS测试命令，判断自己显卡驱动是否正常工作：`vblank_mode=0 glxgears`

一般FPS能达到3000以上，就说明显卡驱动能正常工作了。

建议使用应用商店的“显卡驱动管理器”切换或者安装驱动。不到迫不得已，请不要升级内核。

NVIDIA显卡安装闭源驱动后，vblank_mode=0选项无效，需要从自带的配置程序中取消取消“垂直同步刷新”功能。

```bash
sudo apt install nvidia-settings
nvidia-settings
```

### 1.5.4 开机出现 Firmware Bug 日志

错误日志内容：

> \[Firmware Bug\]: TSC_DEADLINE disabled due to Errataplease update microcode to version: 0x22 (or later)

解决办法：`sudo apt install intel-microcode` 或 `sudo apt install amd64-microcode`

检查安装：`dmesg | grep microcode`

问题来源：[stackoverflow.com](https://stackoverflow.comquestions/48036877debian-firmware-bug-tsc-deadline-disabled-due-to-errata)

应用这个补丁后，会降低CPU性能。家用不推荐。

# 第二章 终端操作技巧集锦

## 2.1 以管理员身份自启动

编辑 ~/.profile 添加下面三行内容：

```bash
sudo -S rmmod pcspkr <<EOF
password(你的密码)
EOF
```

在Shell脚本中，通常将EOF与 << 结合使用，表示后续的输入作为子命令或Shell的输入，直到遇到EOF为止，再返回到主Shell,即将‘你的密码’当做命令输入。

## 2.2 递归更改文件所有者

```bash
chown -R loaden:loaden *
```

## 2.3 从指定类型文件中查找

```bash
find . -name '*.c' | awk '{print "grep -i -nH keyword "$1}' | bin/bash
find . -name '*.c' -exec grep -i -nH "keyword" {} \;
```

## 2.4 更好的搜索方法

```bash
grep -i "search_string" . -r --include=*.txt
grep "search_string" . -r --include=*.txt --include=*.cpp--include=*.h
```

## 2.5 将行末多个空行转换成一个空行

```bash
find . -name '*.tex' -exec sed -i '/^$/{N;/^\n*$/D}' {} \;
```

## 2.6 ls 只显示目录名

```bash
ls -l |grep ^d 或 ls -d */
```

## 2.7 ls 只显示文件

```bash
ls -l |grep ^-
```

## 2.8 ls 显示软链接

```bash
ls -n
```

## 2.9 为子目录和文件设置不同权限

```bash
find . -type d -exec chmod 755 {} \;
find . -type f -exec chmod 644 {} \;
```

## 2.10 为指定类型文件设置可执行权限

```bash
find . -name 'commit-msg' -type f -exec chmod +x {} \;
find . -name '*.sh' -type f -exec chmod +x {} \;
```

## 2.11 获取脚本文件所在路径

包含文件：`$0`
只要路径：`dirname "$0"`

## 2.12 批量文本替换

```bash
grep "old" -rl ./ |xargs sed -i "s/old/new/g"
grep "Objbase.h" -rl . --include=*.cpp --include=*.h |xargs sed-i "s/Objbase.h/objbase.h/g"
#或
sed -i "s/原字符串/新字符串/g" `grep 原字符串 -rl 所在目录
#举例
grep "图像" -rl . --include=*.tex |xargs sed -i "s/图像/图象/g"
```

## 2.13 获取 CPU 个数并四则运算后导出环境变量

```bash
export MAKE_JOBS=$(echo $(nproc)*2-1|bc)
#应用：
make -j $MAKE_JOBS
```

## 2.14 命令行解压缩到指定目录

```bash
tar xvf XXX.tar.xz -C /opt
```

不需要添加J选项，tar会根据压缩包名称识别压缩包格式。
所以xvf应该可以作为万能参数了。

## 2.15 通过 git 实现跨平台的 grep 用法

```bash
git grep -li pkgconfig -- \*.pr?
```