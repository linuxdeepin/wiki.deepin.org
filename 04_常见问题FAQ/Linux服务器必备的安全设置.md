---
title: Linux服务器必备的安全设置
description: 
published: true
date: 2023-06-14T13:39:42.142Z
tags: 
editor: markdown
dateCreated: 2023-06-14T13:39:42.142Z
---

好不容易买了服务器，如果因为自己的疏忽，被黑客黑掉的话，那真的是太糟糕了！

下面告诉你一些简单的方法提高服务器的安全系数，我的云服务器就是这么配置的，虽然有些麻烦，但是感觉安心一些。

### 修改 ssh 登陆配置

打开 ssh 配置文件

```
vim /etc/ssh/sshd_config

#修改以下几项
Port 10000
#更改SSH端口，最好改为10000以上，别人扫描到端口的机率也会下降。防火墙要开放配置好的端口号，如果是阿里云服务器，你还需要去阿里云后台配置开发相应的端口才可以，否则登不上哦！如果你觉得麻烦，可以不用改
 
Protocol 2
#禁用版本1协议, 因为其设计缺陷, 很容易使密码被黑掉。
 
PermitRootLogin no
#尝试任何情况先都不允许 Root 登录. 生效后我们就不能直接以root的方式登录了，我们需要用一个普通的帐号来登录，然后用su来切换到root帐号，注意 su和su - 是有一点小小区别的。关键在于环境变量的不同，su -的环境变量更全面。
 
PermitEmptyPasswords no
＃禁止空密码登陆。
```

最后需要重启 sshd 服务

```
service sshd restart
```

### 禁止系统响应任何从外部 / 内部来的 ping 请求

```
echo “1”> /proc/sys/net/ipv4/icmp_echo_ignore_all
```

其默认值为 0

### 用户管理

下面是基本的用户管理命令

```
查看用户列表：cat /etc/passwd
查看组列表：cat /etc/group
查看当前登陆用户：who
查看用户登陆历史记录：last
```

一般需要删除系统默认的不必要的用户和组，避免被别人用来爆破：

```
userdel sync
userdel shutdown
# 需要删除的多余用户共有：sync shutdown halt uucp operator games gopher
groupdel adm
groupdel games
# 需要删除的多余用户组共有：adm lp games dip
```

Linux 中的帐号和口令是依据 /etc/passwd 、/etc/shadow、 /etc/group 、/etc/gshadow 这四个文档的，所以需要更改其权限提高安全性：

```
chattr +i /etc/passwd
chattr +i /etc/shadow
chattr +i /etc/group
chattr +i /etc/gshadow
```

如果还原，把 +i 改成 -i , 再执行一下上面四条命令。

注：i 属性：不允许对这个文件进行修改，删除或重命名，设定连结也无法写入或新增数据！只有 root 才能设定这个属性。

### 创建新用户

创建新用户命令：adduser username

更改用户密码名：passwd username

个人用户的权限只可以在本 home 下有完整权限，其他目录要看别人授权。而经常需要 root 用户的权限，这时候 sudo 可以化身为 root 来操作。我记得我曾经 sudo 创建了文件，然后发现自己并没有读写权限，因为查看权限是 root 创建的。

sudoers 只有只读的权限，如果想要修改的话，需要先添加 w 权限：chmod -v u+w /etc/sudoers 然后就可以添加内容了，在下面的一行下追加新增的用户：wq 保存退出，这时候要记得将写权限收回：chmod -v u-w /etc/sudoers

### 赋予 root 权限

- 方法一：修改 /etc/sudoers 文件，找到下面一行，把前面的注释（#）去掉

```
## Allows people in group wheel to run all commands
# 去掉下面一句的前面的注释 # 
%wheel ALL=(ALL) ALL
# 然后修改用户，使其属于root组（wheel），命令如下：
# usermod -g root uusama
```

修改完毕，现在可以用 uusama 帐号登录，然后用命令 su – ，即可获得 root 权限进行操作。

- 方法二（推荐）：修改 /etc/sudoers 文件，找到下面一行，在 root 下面添加一行，如下所示：

```
## Allow root to run any commands anywhere
root ALL=(ALL) ALL
uusama ALL=(ALL) ALL
```

修改完毕，现在可以用 uusama 帐号登录，然后用命令 sudo -s ，即可获得 root 权限进行操作。

- 方法三：修改 /etc/passwd 文件，找到如下行，把用户 ID 修改为 0 ，如下所示：

```
uusama:x:500:500:tommy:/home/uusama:/bin/bash
# 修改后如下
uusama:x:0:500:tommy:/home/uusama:/bin/bash
```

保存，用 uusama 账户登录后，直接获取的就是 root 帐号的权限。