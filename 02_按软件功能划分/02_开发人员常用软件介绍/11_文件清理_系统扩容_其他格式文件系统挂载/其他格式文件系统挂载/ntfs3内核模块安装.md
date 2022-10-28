---
title: ntfs3内核模块安装
description: deepin挂载ntfs文件系统
published: true
date: 2022-10-21T08:49:50.527Z
tags: 
editor: markdown
dateCreated: 2022-07-15T09:33:42.006Z
---

# NTFS3挂载NTFS文件系统
###### 核心功能
>ntfs3内核模块可以使window的ntfs磁盘分区挂载到deepin系统后实现权限的实时变更，而不是固定按照fstab里挂载的ntfs权限。

##### 下载补丁
```
wget https://ucare-resources.oss-cn-shenzhen.aliyuncs.com/pro/2022/05/178f047f89-f2c4-488e-8a85-d57a4db5d6c7 -O /tmp/ntfs3-5.17.patch.xz 
```
#### 解压安装
```
cd /usr/src
xzcat /tmp/ntfs3-5.17.patch.xz 

# 注册dkms
dkms   add   -m   ntfs3   -v   5.17

# 安装ntfs3内核模块到当前系统
dkms  autoinstall  -m   ntfs3  -v  5.17

# 确认安装
dkms   status
modinfo ntfs
```
#### 新建配置文件
```
sudo su - root
touch /etc/udev/rules.d/99-ntfs3.rules
echo 'SUBSYSTEM=="block", ENV{ID_FS_TYPE}=="ntfs", ENV{ID_FS_TYPE}="ntfs3"'>/etc/udev/rules.d/99-ntfs3.rules

touch /etc/udisks2/mount_options.conf
echo 'ntfs3_defaults=uid=$UID,gid=$GID,noatime,prealloc'>/etc/udisks2/mount_options.conf
echo 'ntfs3_allow=uid=$UID,gid=$GID,umask,dmask,fmask,iocharset,nohidden,sys_immutable,discard,force,sparse,showmeta,prealloc,noacsrules,acl'>>/etc/udisks2/mount_options.conf

```

#### 挂载NTFS文件系统
```
cat /etc/fstab 

# /dev/nvme0n1p1
/dev/nvme0n1p1 /data ntfs3 defaults,uid=1000,gid=1000,dmask=022,fmask=022 0 0

# /dev/nvme0n1p2
/dev/nvme0n1p2 /work ntfs3 defaults,uid=1000,gid=1000,dmask=022,fmask=022 0 0
```

#### 小结
默认ntfs格式只认挂载时的权限，挂载后不能修改权限；ntfs3可以把windows的磁盘文件挂载到deepin系统上用，然后可以修改文件目录的权限了。
这样我们在用docker或者有些软件需要修改目录权限的时候就不会报错了

参考:[使用NTFS3来取代ntfs-3g读写NTFS文件系统](https://club.uniontech.com/ucare/#/detail?id=b34dfc360c5c43f3a9d11ce1f36c6f8c&part=first)

		