---
title: linux 权限管理
description: 
published: true
date: 2022-08-11T09:25:18.018Z
tags: 
editor: markdown
dateCreated: 2022-08-11T09:23:58.775Z
---

# linux 权限管理
## 前言
linux作为一个服务器起家的操作系统，比相对工作站起家的windows有着更加注重安全性的传统。

在linux中管理安全性的技术有：
1. UGO
1. ACL
1. SELinux
1. PAM
## UGO
UGO （User、Group、Other）即用户，用户组，他人，三个主体的权限组合。
```
ls -l
# 将会输出当前文件夹的UGO权限信息
# 如：
-rw-r-xr--  1 htqx htqx    3328 1月  20 10:31 cmake_install.cmake
```
组成：
1.第一个字符表示文件的类型：d目录、l链接、s套接字、b块设备、c字符设备、p管道、-普通文件。
2.后续三个字符为一组，表示用户、用户组、他人的权限
r ： 读
w ： 写
x ： 可执行
3.htqx 是我的用户名
4.htqx 是我的用户组
linux的程序或脚本的可执行性并不类似windows，是通过文件类型（扩展名）来定义的，而是通过权限x。

也可以用数字来表示：

4 ：读
2 ：写
1 ：可执行
并且可以相加来组合，如3=2+1，5=4+1，6 = 4+2， 7= 4+2+1。因此可以通过 756 这样的组合表示用户读写+执行，用户组只读+执行，他人读写。
```
# 修改文件的权限
# u 用户
# g 用户组
# o 他人
# a 三者
# + 增加权限
# = 设置权限
# - 删除权限
# 721 数值表示法设置权限
chmod u+r filename
chmod go=wx filename
chmod 721 filename

# 改变用户和用户组
# 改成root 用户和root用户组
sudo chowm root:root filename
```
## 文件夹
同理，但文件夹的意义有所变化：

r：列出文件
w：删除文件
x：进入文件夹，如 cd 命令
rx： 只读
wx： 可写
一般而言，文件夹都要rw权限（因为很多读取操作其实蕴含了要进入该文件夹的），表示只读。同理，wx即表示可写。
## 扩展位
其实权限并非只有3组，而是4组，还有一组隐藏来做特殊用途。

当文件可执行时：

- 4：借用用户权限
- 2：借用用户组权限
当是文件夹时：

- 2：目录内文件继承用户组(递归)
- 1：目录内文件不可所属用户之外的人删除（非递归）
```
# 复制一个特权命令到本地
# u+s = 4 即借用用户权限
# 这个特权命令是root用户的，不信可以 ls 看看
# 然后就等于借用 root 的权限执行hwclock
# 普通用户自行这个是报错的，不会显示硬件时间
sudo cp /sbin/hwclock .
sudo chmod u+s hwclock 
# 或者用数字方法来设置
sudo chmod 4755 hwclcok
ls -l hwclock 
./hwclock
```
## umask
默认创建的文件是666权限，文件夹是777，同时还有个蒙版（掩码） umask 022。 即666-022=644， 777-022=755。
```
# 查看当前蒙版
umask
# 更改为002
umask 002
```
## ACL
ACL （Access Control List）访问控制表。相对基本的linux UGO权限，ACL提供了多用户，多用户组控制的能力。但要求文件系统自身支持该扩展。
```
# 文件系统通过acl选项挂载
# 看不懂可以忽略
mount -t ext4 -o rw,acl /dev/sda1 /mnt

# ls -l 表现为权限位后有一个 + 符号

# 为了演示，去除其他人可执行权限
sudo chmod o-x hwclock
./hwclock #出错

# 获取acl信息
getfacl hwclock
# 输出：
# file: hwclock
# owner: root
# group: root
# flags: s--
user::rwx
group::r-x
other::r--

# 设置acl权限
# -m 设置
# u:lp:rw  u:用户:权限
# g:htqx:rx  g:用户组:权限
# 用逗号分隔多组
# -x 表示去除
sudo setfacl -m u:lp:rw,g:htqx:rx hwclock
getfacl hwclock
# 输出：
# file: hwclock
# owner: root
# group: root
# flags: s--
user::rwx
user:lp:rw-
group::r-x
group:htqx:r-x
mask::rwx
other::r--

# 查看UGO，多了个 + 号，表示有acl权限
ls -l hwclock 
-rwsrwxr--+ 1 root root 100560 1月  20 16:27 hwclock

# 因为 acl 添加htqx用户组，当前用户htqx又具备了可执行权限
./hwclock
```
## PAM
PAM（Pluggable Authentication Modules）可插拔认证模块。这个是linux账户登录验证的一个框架。

层次结构：

1.会话登录（应用层）
2.PAM-API 接口层，由系统管理员配置模块
3.PAM-SPI 服务模块
认证管理模块
账号管理模块
会话管理模块
口令管理模块
相关路径：

1./etc/pam.d/ ：配置目录
2./lib/security/ ：服务模块
3./etc/security/: 服务模块配置参数