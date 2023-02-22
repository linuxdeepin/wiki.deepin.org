---
title: Linux 下恢复误删文件
description: 
published: true
date: 2023-02-22T02:57:01.505Z
tags: linux
editor: markdown
dateCreated: 2023-02-21T09:17:35.091Z
---

#  Linux 下恢复误删文件
本文来自论坛用户littlebat在论坛中的分享-[《Linux 下恢复误删文件》](https://bbs.deepin.org/post/243925)
首发地址：学习日记 https://www.learndiary.com/2022/09/linux-file-recovery/

### 一、数据误删背景

这次是一名科技工作者客户在 Ubuntu Linux（版本大概是20.04） 下用 rm 命令误删除了一个重要文件夹里面的文件，然后又在同一分区上装了一个数据恢复软件试图恢复。结果失败。他在我回复他不能保证恢复数据的程度后，去找了一个人帮他恢复，结果可能不理想，又联系我帮他恢复。视频演示地址：
[视频链接](https://www.bilibili.com/video/BV1s14y187MK?share_source=copy_web&vd_source=d1925b070926f23b2b6676137251e9ea)
提示：本演示视频分为“恢复过程”和“恢复小结”如下两部分
 
### 二、数据恢复过程

首先我要求客户长按电源键关闭电脑，避免关机过程中的数据写入操作进一步破坏丢失的文件。然后，我用向日葵远程到客户同一台机器上的 Windows 系统，制作了 Ubuntu 的 U 盘 Live 系统。然后，从 U 盘启动 U 盘里的 Live 系统，在 Live 系统中安装向日葵，我用向日葵远程到客户的 Ubuntu Live 系统进行操作。

下面的操作均未挂载硬盘上的分区。用lsblk -f命令查看了客户的磁盘分区情况，发现客户有一个单独的 /home 分区，格式是ext4，而被删除的文件是位于家目录下的一个文件夹中。用户之前一直运行着硬盘上的 Ubuntu 系统，这样，系统可能会随时写入数据，如 .cache 缓存文件夹，加之用户还在家目录里面安装了一个数据恢复软件。之前，也有其他人帮他作了一些未知的操作。所以，我预判这次的数据恢复程度不容乐观。这里假设这个分区是 /dev/sda3。

先调整 Live 系统的时区为正确的 +8 时区，问清了客户数据删除的大概时间。然后，我在 Live 系统中安装了 testdisk 和 ext4magic。

硬盘是 /dev/sda，执行 sudo testdisk /dev/sda，验证了客户删除数据的具体位置，查看删除数据的上级目录时间（目录内容更新时间），证实了客户删除文件的时间。然后退出 testdisk。

然后挂载硬盘的另一个分区作为存储恢复数据的地方。假设这个用于存储恢复数据的分区是 /dev/sda1，切换到挂载这个分区的目录下如: /mnt/tmp，执行`sudo ext4magic /dev/sda3 -a "$(date -d "2022-09-25 15:00:00" +%s)" -r`，表示恢复2022年9月25日15点后删除的文件。

恢复的文件保存在 RECOVERDIR 中，根据文件删除的路径是找不到的。比如，删除的文件是位于 /home/user/dir 下面，在 RECOVERDIR 下相应的路径 RECOVERDIR/home/user/dir 是找不到的。

最后有效的文件都位于 RECOVERDIR/MAGIC-2 下面，但是没有原来的目录结构和文件名了。下面的文件是 ext4magic 自动归类的，比如客户要找回的 python 文件是位于 RECOVERDIR/MAGIC-2/text/x-script.python，而一种后缀名为 .rmf 的文件则位于 RECOVERDIR/MAGIC-2/application/octet-stream 下面。这次寻找的方法是根据丢失文件的内容含有的关键字。比如，python 文件基本都会引用一个特殊的包比如叫“specialpackage”。而 .rmf 文件虽然没有归类到文本文件，但是我用 vim 打开，看到大部分是可识别的文本，夹杂一些乱码，其中也含有这个格式文件开头特有的关键字如“mykeyinhead”。

当然，根据实际情况，你也可以尝试用文件大小或修改时间等文件属性查找。但建议属性留有余地，我曾经测试过找回来的文件丢失了一些修改步骤，相应的修改时间似乎也丢失了。

### 三、数据恢复结果

从最后的结果来看，在最初作了错误的数据覆盖操作的情况下，也找回了很多 python 文件和一些 rmf 文件，客户对恢复的文件还是基本满意的。

### 四、数据误删恢复小结

1. 重要的数据要在不同的介质备份。

2. 误删数据后不要往相同分区作写入操作。未被占用可以直接卸载的分区就直接卸载。如果是有自动写入操作的分区（如 / 和 /home 等分区）的话，建议直接按住电源按纽数秒关闭电脑。

3. 上面直接卸载的分区可以直接在电脑上安装恢复软件进行恢复；但系统启动后会直接作写操作的分区就用 Live Linux 启动进行恢复操作。

4. 为了尽可能多的找回丢失的文件，我也给上面这位客户尝试用跟 testdisk 伴生的 photorec 进行恢复。photorec 是根据文件类型的特征，逐扇区扫描硬盘找回删除的文件。不会管文件的删除时间。photorec 也可以指定只搜索特定类型的文件。但因为客户找的用于保存数据的移动硬盘中途出现IO错误导致恢复中断，另外，通过上面 ext4magic 找回的文件可能已经达到了客户的心理预期，后来就放弃了这种耗时很长、恢复文件占用空间很大的方法。

5. 网上的不少文章讲到可以通过文件系统日志恢复删除的文件，还能保留原有的文件名称。但据我的测试，在新版本的操作系统中（如：Ubuntu 20.04）的 ext4 文件系统中删除和恢复文件是办不到的。旧版本的系统，如在 Ubuntu 16.04 Linux 中的 ext4 文件系统中删除和恢复文件则是可以保留原有的文件名和目录结构的。我测试过 ext4magic、R-Linux、R-Studio、UFS Explorer、Recovery Explorer，统统一样的效果。extundelete 干脆就不工作。

### 五、参考资料

1. Unix/Linux undelete/recover deleted files https://unix.stackexchange.com/questions/80270/unix-linux-undelete-recover-deleted-files

2. File recovery https://wiki.archlinux.org/title/File_recovery

3. Ext4magic https://ext4magic.sourceforge.net/ext4magic_en.html

4. TestDisk https://www.cgsecurity.org/wiki/TestDisk_CN

5. PhotoRec https://www.cgsecurity.org/wiki/PhotoRec_CN