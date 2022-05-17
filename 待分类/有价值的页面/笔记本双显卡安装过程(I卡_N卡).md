---
title: 笔记本双显卡安装过程(I卡 N卡)
description: 
published: true
date: 2022-05-17T11:36:08.666Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:51:46.780Z
---

## 简介
本经验有论坛用户(walle)分享，[原文地址](https://bbs.deepin.org/forum.php?mod=viewthread&tid=141410&page=1&extra=#pid345736)。
## 正文
* 删除以往所有安装的驱动/大黄蜂

```
sudo apt remove --purge nvidia*
sudo apt remove --purge bumblebee*
```

* 打开驱动管理器，点击驱动进行安装
* 再安装一些相关软件

```
sudo apt-get install bumblebee-nvidia bumblebee primus
```

* 使用独显启动程序

```
optirun command #使用独显运行command程序
optirun -b primus command #使用独显运行command程序,提升性能
```

* 可以安装 nvidia-smi，sudo nvidia-smi执行之，可以看到驱动已经安装，设备正常运转
* 执行 sudo lspci -v 可以查看到详细信息
* 在使用optirun命令开启独显前，可以使用lspci命令查看设备，可以看到独显最后的信息为(rev ff).ff代表设备关闭  
使用optirun命令后，再次lspci，可以看到独显最后的信息为(rev a2)，这代表独显已经正常使用了

### ps
安装好驱动后，nvidia x server seetings是打不开的，点击后会弹出错误提示，然后很误导我就按照之前的思路去折腾了。命令“optirun xxx"可用来调用N卡运作处理程序xxx（程序名）的图形计算 ，不过图形渲染仍由I卡负责，性能提升有限。   

更新：

archwiki

bumblebee配置下所有需要独显的操作都需要调用optirun或者primusrun

`optirun -b none nvidia-settings -c :8`

### 黑屏/进不去系统  救急三法

ctrl+alt+2进入2号控制台

```
1： sudo apt remove --purge nvidia*
2:   sudo rm /etc/X11/xorg.conf
3:   sudo apt install xorg
```

三种原因各不同，不详细说了，挨个试 总有一款能救你

### 其他坛友补充
按着上面的教程做……重启可以显示登录界面但无法输入密码又是什么情况T-T试着sudo apt install xorg，诶！成功登录！果然是删除nvidia时误删了xorg。

然后optirun glxinfo|grepNVIDIA，嗯驱动安装成功，optirun glxgears -info，显示小齿轮动画，可以愉快地使用optirun命令调用独显运行程序了，感觉显示效果好了不少 ；）

*TIPS*

有时因为界面跳的太快进不了tty的时候，可以开机选择“Advanced options for *****”这一行按回车，然后选中最后是“（recovery mode）”这一行按“E”进入编辑页面，把ro改成rw，按F10，然后进入root shell。