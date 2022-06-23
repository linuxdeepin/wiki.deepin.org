---
title: 如何创建samba共享文件夹
description: 在 Linux 上创建共享文件夹，并对不同的用户赋予不同的访问权限
published: true
date: 2022-06-23T11:39:21.257Z
tags: 
editor: markdown
dateCreated: 2022-06-23T11:39:21.257Z
---

# 开启Samba共享的两种方式
在深度文件管理器以及鹦鹉螺(naitulus)文件管理器中，都可以在文件夹右键菜单中找到共享文件夹的入口。但这种方式只能单用户或匿名共享，那么如何进行多用户分权的共享呢？

---
## 方法一. 使用 `net` 命令开启文件夹共享

其实，深度文件管理器与鹦鹉螺文件管理器都是通过 `net` 命令实现的文件夹共享，但实现较为简单，鹦鹉螺文件管理器的共享插件代码也是开源的，通过查看代码，发现共享的核心调用其实就是 `net` 命令的执行：
`net usershare add <share_name> <share_path> <share_comment> <share_acl> <share_allow_guest>`
例如，通过命令：
`net usershare add myVideo ~/Video Everyone:F "" guest_ok=y`
就将用户目录下的 Video 文件夹，以匿名的方式共享给了所有人，并且都具有全部的读写权限。

---

在多用户分权共享时，原理其实也如此，不过所使用的参数更详细具体。
首先，因为我们要分用户/用户组对同一共享文件夹访问权限做配置，因此需要创建多个用户：
`sudo groupadd g1 && sudo groupadd g2`
我们创建了 `g1` 和 `g2` 两个用户组。

`sudo useradd u1 -G g1 && sudo useradd u2 -G g2`
然后创建了两个用户 `u1` 和 `u2`，并分别对其设置为 `g1` 组和 `g2` 组。

第三步，需要将用户添加到 samba 用户列表中，并单独设置每个用户的密码。
`sudo smbpasswd -a u1`
对于 `u2` 用户也是一样的操作，
```
uos@uos-PC:~$ sudo smbpasswd -a u1
New SMB password:
Retype new SMB password:
Added user u1.

```

---

我们现在已经具备一个需要共享的文件夹 `/home/uos/publicShare` 以及两个用户 `u1` 和 `u2` 了，那么我们现在要共享该文件夹，并且 `u1` 用户具备完全访问权限，`u2` 用户仅具备读取权限。

我们假定用户目录 `/home/uos/publicShare` 已经共享出去了，并且未开启匿名。那么，在目录 `/var/lib/samba/usershares` 目录下，将会生成一个文件 `publicshare`。该文件描述了我们的共享配置信息：

```
#VERSION 2
path=/home/uos/publicShare
comment=""
usershare_acl=S-1-1-0:f
guest_ok=n
sharename=publicShare

```
其中，`path` 字段即为我们要共享的文件夹，这些字段与上文中介绍的命令参数一一对应不再赘述。

那么，要不同的用户具有不同的用户权限，则关注 `usershare_acl` 字段。
`S-1-1-0:f` 表示用户 `Everyone` 具有完全访问权限。`S-1-1-0` 是用户 `Everyone` 的 `SID`，`f` 表示完全访问权限，与 `f` 对应的为 `r` 表示只读权限。

一切就很简单了，我们需要分别获取两个用户的 `SID` 然后配置到这个文件里就可以啦。

---

通过命令行获取用户 `SID` 需借助命令 `wbinfo`。
> 如果没有这个命令的话，通过 `sudo apt install -y winbind` 进行安装，该工具可以互相转换 `SID/UID/GID`。更多使用细节参见 `man wbinfo`。

执行命令：
```
wbinfo -U　｀id -u｀
```
命令输出的字符串（例如：sid-for-u1）记录备用。
同理获得 sid-for-u2。

1. 直接修改共享文件
我们可以直接将共享配置文件中的 `usershare_acl` 字段进行修改：
```
usershare_acl=sid-for-u1:f,sid-for-u2:r
```
保存文件并退出，此时，在客户端访问该文件夹，分别以不用用户访问，就具备了不同的权限了。

2. 也可以通过 `net` 命令指定用户权限，原理是一样的
```
net usershare add myShare /home/uos/publicShares "no comment" u1:F,u2:R
```

该步骤将在 `/var/lib/samba/usershares` 目录下生成 `myshare` 文件，其 `usershare_acl` 字段与 1 中的一致。

---

相同的方法，对于组权限控制也是一致的。


## 方法二. 修改 `smb.conf` 文件配置共享
