## ZFS简介
   ZFS是一款128bit文件系统，总容量是现有64bit文件系统的1.84x10^19倍，其支持的单个存储卷容量达到16EiB（2^64byte，即16x1024x1024TB）；一个zpool存储池可以拥有2^64个卷，总容量最大256ZiB（2^78byte）；整个系统又可以拥有2^64个存储池。可以说在相当长的未来时间内，ZFS几乎不太可能出现存储空间不足的问题。另外，它还拥有自优化，自动校验数据完整性，存储池/卷系统易管理等诸多优点。较ext3系统有较大运行速率，提高大约30%-40%。
   
   ZFS是基于存储池的，与典型的映射物理存储设备的传统文件系统不同，ZFS所有在存储池中的文件系统都可以使用存储池的资源。
   
## 协议问题

ZFS基于的CDDL协议与Linux内核的GPL授权有些不相容，因此ZFS一直未在Linux进行推广。但是随着OpenZFS、ZFS on Linux等项目的发展，ZFS已可以在Linux上正常使用。

## Root On ZFS安装步骤
注意，这里的步骤默认全盘安装。如果需要专门分区，请在分区的步骤中自己做出相应修改。
硬盘操作有风险，请提前做好数据备份。若有任何问题，本教程概不负责。
### 1. LiveCD的制作
这里以Windows环境和Deepin 15.4 RC为例。在官网下载好iso镜像，在本地挂载后，直接调用iso镜像内自带的maker即可。但是，该版maker默认只创建LiveCD-Installer的引导项，为了进入LiveCD环境，我们进行以下修改：

1. 定位到启动U盘根目录下，进入`boot\grub`文件夹，用合适的文本编辑器打开`grub.cfg`文件，你会看到以下内容：
```
if loadfont /boot/grub/font.pf2 ; then
	set gfxmode=auto
	insmod efi_gop
	insmod efi_uga
	insmod gfxterm
	terminal_output gfxterm
fi
set menu_color_normal=white/black
set menu_color_highlight=black/light-gray
menuentry "Install Deepin" {
	set gfxpayload=keep
	linux	/live/vmlinuz.efi boot=live union=overlay livecd-installer locale=zh_CN quiet splash --
	initrd	/live/initrd.lz
}
```

将第十四行的`livecd-installer`改为`livecd`。
2. 回到根目录，定位到`syslinux`文件夹下，打开`live.cfg`，同样将其中的`livecd-installer`改为`livecd`。

### 2. 为livecd环境配置zfs

由于Deepin官网目前自带的zfs软件包组存在难以言表的bug，我们这里采用换源的方法。另外对于尝鲜感兴趣的话，也可以试着在Github上直接clone下来[源代码](https://github.com/zfsonlinux/zfs)编译最新版，这里不再介绍。
具体步骤如下：

####1. 成为Root用户：
打开终端，输入
``` 
sudo -i
```

####2. 换源并更新
依次输入以下三条指令
```bash
echo deb http://ftp.debian.org/debian jessie-backports main contrib \
      >> /etc/apt/sources.list.d/backports.list
      
apt update
apt install --yes -t jessie-backports zfs-dkms gdisk
```

安装时需要针对内核专门编译，故耗时较长，请耐心等待。
为了以防万一，请在安装完后专门运行以下命令，防止zfs模块未正常加载：

```
modprobe zfs
```

####3. 硬盘格式化
以下硬盘操作需要先掌握几个基本指令：
```
ls -la /dev/disk/by-id
```
这个指令会按照硬盘id列举你的硬盘，下面我们默认使用的id是`scsi-SATA_disk1`。在操作时，请先查明你的硬盘id，并进行替换。
```
ls -la /dev/disk/by-partuuid
```
这个指令会按照硬盘分区uuid列举你的硬盘分区，在修改`fstab`时会用到。

如果你之前使用mdadm创建了阵列，这里需要清除`superblock`才行。具体指令为
```
mdadm --zero-superblock --force /dev/disk/by-id/scsi-SATA_disk1
```

下面，我们使用`gdisk`进行分区，如果你是`BIOS`用户，键入以下指令
```
sgdisk -a1 -n2:34:2047  -t2:EF02 /dev/disk/by-id/scsi-SATA_disk1
sgdisk     -n9:-8M:0    -t9:BF07 /dev/disk/by-id/scsi-SATA_disk1
sgdisk     -n1:0:0      -t1:BF01 /dev/disk/by-id/scsi-SATA_disk1
```
如果你是`UEFI`用户，使用以下指令
```
sgdisk     -n3:1M:+512M -t3:EF00 /dev/disk/by-id/scsi-SATA_disk1
sgdisk     -n9:-8M:0    -t9:BF07 /dev/disk/by-id/scsi-SATA_disk1
sgdisk     -n1:0:0      -t1:BF01 /dev/disk/by-id/scsi-SATA_disk1
```
然后就是建立`zpool`，我们采用官网的推荐配置。如需修改，请到[官网](http://zfsonlinux.org/)寻找手册进行参考。特别要注意的是，输入硬盘id时不要忘记了`-part1`(或者是你自己个性化分区时指定的主分区)，否则`zpool`会抹掉全盘，而预留的引导分区也就没有了。
```
zpool create -o ashift=12 \
      -O atime=off -O canmount=off -O compression=lz4 -O normalization=formD \
      -O mountpoint=/ -R /mnt \
      rpool /dev/disk/by-id/scsi-SATA_disk1-part1
```
还需要创建一系列的`dataset`
```
zfs create -o mountpoint=none rpool/ROOT

zfs create -o mountpoint=/ rpool/ROOT/deepin

zfs create -o mountpoint=/home rpool/HOME

zfs create -o mountpoint=none rpool/DEEPIN

zfs create -o mountpoint=/usr/src rpool/DEEPIN/src

zfs create -o mountpoint=/srv rpool/DEEPIN/srv
```
设置分区属性
```
zpool set bootfs=rpool/ROOT/deepin rpool
```
创建`swap`分区，把`[VOL]`项改为适宜你的大小。
```
zfs create -o sync=always -o primarycache=metadata -o secondarycache=none -V [VOL] rpool/swap

mkswap -f /dev/zvol/rpool/swap
```
####4. 安装系统
首先解压`squashfs`文件至制定分区
```
unsquashfs -f -d /mnt /lib/live/mount/medium/live/filesystem.squashfs

unsquashfs -f -d /mnt /lib/live/mount/medium/overlay/overlay-language-pack-zh-hans.squashfs

unsquashfs -f -d /mnt /lib/live/mount/medium/overlay/overlay-chinese-special-framework.squashfs
```
然后我们`chroot`进入安装目录进行设置.

```
cd /mnt

mount --rbind /dev dev

mount --rbind /dev/pts dev/pts

mount --rbind /proc proc

mount --rbind /sys sys

cp /etc/resolv.conf /mnt/etc/

chroot /mnt /bin/bash
```
在新系统中安装ZFS，这里又一次用到了`debian`的源，在系统安装后，为了防止冲突，请自行移除。
```bash
echo deb http://ftp.debian.org/debian jessie-backports main contrib \
      >> /etc/apt/sources.list.d/backports.list
      
apt update
apt install --yes -t jessie-backports zfs-dkms zfs-initramfs
```
不同之处在于这次安装后需要修改以下文件。
```
vi /usr/share/initramfs-tools/conf.d/zfs
for x in $(cat /proc/cmdline)
do
    case $x in
        root=ZFS=*)
            BOOT=zfs
            ;;
    esac
done
```
然后是安装`GRUB`
####Legency GRUB
```
apt install --yes grub-pc
```

同时用`gparted`格式化`gdisk`分配好的较小的（不是最小的）用于引导的分区为`ext4`或其他合适格式。在`fstab`中加入以下内容，`PARTUUID`部分请自行修改
```
PARTUUID /boot ext4 defaults,noatime  1 2
```
挂载分区
```
mkdir /mnt/boot

mount /dev/sda2 /mnt/boot
```
####UEFI GRUB
`scsi-SATA_disk1-part3`为之前`gdisk`分配的`512M`分区，请自行修改！
```
apt install dosfstools
mkdosfs -F 32 -n EFI /dev/disk/by-id/scsi-SATA_disk1-part3
mkdir /boot/efi
```
修改`fstab`, 加一行。其中`PARTUUID`为之前`gdisk`分配的`512M`分区的`PARTUUID`号，请自行查找。
```
PARTUUID /boot/efi vfat default 0 1
```
然后运行
```
mount /boot/efi
apt install --yes grub-efi-amd64
apt install --yes -t testing grub-efi-amd64
```


`grub`预配置到此结束。

接着是将`swap`分区的信息加入到`fstab`
```
/dev/zvol/rpool/swap                        none    swap  sw                0 0
```
修改 `/etc/hostname` ，如果不存在这个文件就新建一个，内容只需要写上你喜欢的主机名就行了。
然后添加新用户，按照需求修改所属组。`USERNAME`改为你喜欢的。
```
useradd -s /bin/bash -m -G users,cdrom,disk,audio,video,games,plugdev,bluetooth,netdev USERNAME
```
设置 root 和用户密码。
```
passwd root

passwd USERNAME
```
通过 `visudo` 命令，把你的用户加入 `sudoer` 列表。在 `root ALL=(ALL:ALL) ALL` 下面添加一行：
```
USERNAME ALL=(ALL:ALL) ALL
```
下面我们要完成`grub`安装，首先检查一下是否正常识别。
```
ln -s /proc/self/mounts /etc/mtab

grub-probe /

#如果输出zfs，则进行后续操作

update-grub

update-initramfs -u -k all

#legency
grub-install /dev/sda

#uefi
grub-install --target=x86_64-efi --efi-directory=/boot/efi \
      --bootloader-id=debian --recheck --no-floppy

#/dev/sda改为自己需要的
```
检查一下是否正常
```
ls /boot/grub/*/zfs.mod
```

编写 `/etc/hosts` ，内容如下，否则系统会出现各种奇怪的问题。里面的` Deepin `是我设置的主机名：
```
127.0.0.1	localhost
127.0.1.1   Deepin
# The following lines are desirable for IPv6 capable hosts

::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```
重新配置 `locale`,建议选择美国英语和中文简体。
```
dpkg-reconfigure locales
```
最后解除挂载，重启即可
```
mount | grep -v zfs | tac | awk '/\/mnt/ {print $3}' | xargs -i{} umount -lf {}
zpool export rpool
reboot
```
如果解除挂载失败，重启时可能报错，在报错界面强制挂载再解除挂载再重启即可！