---
title: linux调整现有分区大小
description: 
published: true
date: 2022-10-25T07:02:55.871Z
tags: 
editor: markdown
dateCreated: 2022-08-11T08:37:50.504Z
---

# linux调整现有分区大小
## 前言
有时候，计划赶不上变化，当你发现分区分小了怎么办？本文提供一个思路。
## 演示
```
# 创建10G 映像盘

dd if=/dev/zero of=ddu.img  bs=1G count=10 status=progress 
fdisk ddu.img

#分成300m 和9700m两个分区


# 加载映像盘分区设备

sudo losetup -Pf ddu.img

# 查看分区
lsblk

# 比如分区格式gpt，/dev/loop0p1 是分区1，/dev/loop0p2是分区2
sudo mkfs.vfat /dev/loop0p1 #格式化
sudo mkfs.ext4 /dev/loop0p2 #格式化

mkdir -p mnt/root
sudo mount /dev/loop0p2 mnt/root #挂载分区
sudo cp -a /* mnt/root #模拟填充数据

# 磁盘满了，怎么扩展
# 卸载挂载点
sudo umount mnt/root
# 卸载设备(如果不是映像盘不需要这步)
sudo losetup -D
# 模拟更多空间
# oflag=append 追加数据
# conv=notrunc 不截断数据
# 两个参数必须一起用，否则每次输出都会立刻截断，从0开始填充，那就覆盖了原始数据，而不是追加了
# 追加10G可用空间
dd if=/dev/zero of=ddu.img oflag=append bs=1G count=10 status=progress conv=notrunc
# 安装gdisk
sudo apt install gdisk  #必须借助这个工具来操作gpt分区的扩展
gdisk ddu.img
# 按x进入专家模式
# 按e将gpt分区表的备份数据重新移动到磁盘末尾，这是fdisk没有的功能。如果gpt分区表的备份不在正确位置，那么就会出错。
# 按w保存修改即可

# gpt表位置对了，但是分区大小没有调整
# 要调整分区，原理很简单，就是删除分区
# 删除分区并不会删除数据，只是修改了分区表的定义，从哪里到哪里，什么分区类型而已，实际上，删除了我们重建一模一样的设置，那么它还是原来的样子。
# 因此，修改分区大小就很简单了，只要开始的位置，分区类型保持原样，结束的位置进行调整即可
# 实际操作是删除分区2，重新分区，因为是最后的分区，保持默认开始，默认结束，它自动调整为最大。
fdisk ddu.img
# 加载映像设备分区
sudo losetup -Pf ddu.img
# 这时候注意，虽然调整了分区表
# 但是没有调整文件格式定义的范围
# 这是两层概念。一般文件系统如果不支持调整大小，那么调分区表的大小是没有意义的
# ext系列文件系统支持
sudo fsck -y /dev/loop0p2 # 做磁盘调整之前都要先fsck
sudo resize2fs -p /dev/loop0p2 #自动扩展到分区表允许的最大大小
sudo mount /dev/loop0p2 mnt/root #挂载分区，查看内容是否还在
lsblk # 查看容量是否变大了
```