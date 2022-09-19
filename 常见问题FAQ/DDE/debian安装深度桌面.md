---
title: debian安装深度桌面
description: 
published: true
date: 2022-09-19T11:08:19.639Z
tags: 
editor: markdown
dateCreated: 2022-09-19T10:07:46.726Z
---

# 如何在debian发行版上安装DDE桌面环境
debian镜像下载地址：https://www.debian.org/download
![2022-9-19_41312.png](/2022-9-19_41312.png)
## 镜像安装
1、在引导界面选择Graphical install（图形化安装），进入安装过程。
2、选择语言界面，选择中文。
3、区域选择，选择中国。
4、主机名，填写debian。
5、域名可不填，直接下一步。
6、设置root密码为root，超级管理员账户。
7、建立新用户，这个只是个昵称，不是登录时的用户名，可以根据自己喜好填。
8、接下来就是设置登录时的用户名了，设置时要多注意，并且一定要记住。
9、设置用户密码。
10、接下来该磁盘分区了，有空闲分区的话推荐使用安装程序进行自动分区，当然也可以手动分区。
11、自动分区的话如果是新手推荐“将所有文件放在同一个分区中”，有经验的就根据自己喜好调整。
12、手动分区的话一定要记住挂载/根目录，否则会报错。
13、Swap分区（交换分区）推荐大小为物理内存的两倍，比如实际内存为2G，swap给上4G就行。
14、完成调整后保存分区表即可。
15、需要注意的是需要记住挂载 根目录/ 的分区号，方便后面安装grub。
16、选择软件包进行安装，建议全不选，需要的后面会手动安装，在此时安装的桌面环境话会连接安全服务器更新内核，速度会非常慢。
17、安装完成后就是配置Grub了，如果不想用Grub替换MBR，就选手动输入。
18、然后输入前面配置的挂载根目录/的文件系统，比如前面用的是/sda1，这里就输入/dev/sda1。（不用特殊处理，直接忽略，按默认继续）
19、安装完成后，拔掉启动U盘，直接点继续。
20、直接按回车进入Debian，等系统加载完如果出现登录界面就说明启动成功了。
21、在login后输入root，password后输入设置的超级管理员密码，以超级管理员权限进入系统。

## 安装DDE桌面环境
使用命令 ```sudo vi /etc/apt/sources.list.d/deepin-git.list```添加以下内容:
```
deb [trusted=yes arch=amd64] https://deepin-community.github.io/debian-sid-dde-deps-repo sid main
deb [trusted=yes arch=amd64] https://deepin-community.github.io/debian-sid-dde-repo sid main
```
添加成功后执行：
``` 
sudo apt update
sudo apt install startdde
sudo apt install deepin-desktop-environment-core
sudo systemctl enable lightdm
```
安装完成后重启,登录进入DDE桌面环境
![截图_选择区域_20220919180706.png](/截图_选择区域_20220919180706.png)
