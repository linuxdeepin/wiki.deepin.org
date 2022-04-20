##简介
本经验由论坛用户(yjgsz)分享,[原地址](https://bbs.deepin.org/forum.php?mod=viewthread&tid=139552&extra=)
##正文

论坛里一直有人问windows和deepin双启动的问题，安装双系统的电脑，硬盘配置各不相同，deepin安装程序在某些情况下不能很好解决这个问题。
加上不少试用deepin的是小白，用linux命令来解决启动问题有难度。
我自己是多系统启动，有4个硬盘（1SSD，2机械硬盘），有MAC、WINPE、WIN10、deepin等，一直不用deepin系统安装后的官方启动模式。
启动PE手动修改配置，用grub或grub2实现双启动，简单实用，只要会点WIN系统启动常识的都可以自己手动解决，不需要复杂的linux命令。

（最好进入WINPE操作，这样更方便。将压缩文件解压缩至ESP分区（引导分区）的根目录下，已包含BIOS和UEFI引导文件）

BIOS MBR模式

思路：正常启动Windows模式（bootmgr）引导BCD菜单，启动WIN系统，或选择grub，进入grub菜单（menu.lst）启动Deepin

1. 启动本机WIN系统，用BOOTICE编辑当前系统BCD，添加实模式启动项（grldr.mbr），完成添加grub启动项。
   也可以启动PE，编辑启动分区\boot\BCD。
2. 编辑根目录下menu.lst配置文件，添加（修改）WIN系统 和 Deepin启动项。
3. 如双系统都无法启动时，可进入PE用BOOTICE 修复主引导记录和分区引导记录，激活引导分区。

UEFI GPT模式

思路：开机先启动grub2，再选择启动WIN系统或Deepin

通过提取Lubuntu ISO中的grub2文件，修改\boot\grub\grub.cfg配置文件，添加（修改）WIN系统和Deepin启动项来实现双启动。

本压缩包已将原WIN系统的UEFI启动文件\efi\boot\bootx64.efi改名为bootx640.efi，

默认启动bootx64.efi 将启动UEFI grub2，调用grub.cfg。

1. 关闭UEFI安全启动模式
2. 编辑\boot\grub\grub.cfg，添加启动菜单，调用bootx640.efi来启动WIN系统
3. 添加Deepin 启动菜单，注意Deepin启动分区，如/dev/sdb4 

UEFI模式BCD位置\efi\microsoft\boot文件夹，UEFI模式下不需要修改BCD，UEFI引导文件扩展名为efi。

注：
  1. 不管是BIOS模式下menu.lst，还是UEFI模式下grub.cfg，都可以不用分区UUID，用/dev/sdxx代替，
     具体可用blkid命令或分区工具查看。这样格式化重装后不用因UUID变化而修改启动配置文件。
  2. （缺点）内核升级后可能导致Deepin无法启动，如最近一次内核升级后，启动文件

    vmlinuz-4.9.0-deepin3-amd64、initrd.img-4.9.0-deepin3-amd64 

    升级为vmlinuz-4.9.0-deepin4-amd64、initrd.img-4.9.0-deepin4-amd64 ，修改配置文件后即可启动。