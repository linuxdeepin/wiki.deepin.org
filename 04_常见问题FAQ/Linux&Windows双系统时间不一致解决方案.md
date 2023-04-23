---
title: Linux&Windows双系统时间不一致解决方案
description: 
published: true
date: 2023-03-13T05:47:49.615Z
tags: 
editor: markdown
dateCreated: 2023-03-13T03:27:55.785Z
---

# Linux & Windows双系统时间不一致解决方案
#### 一、 WINDOWS的时间和时区

Windows操作系统直接把CMOS时间认定为当前显示时间，不根据时区转换。这样每调整一次系统时区，系统会根据调整的时区来计算当前时间，确定后，也就同时修改了CMOS内的时间（即每调整一次时区，设置保存后，CMOS时间也将被操作系统改变一次，注意不同操作系统调整时间后，也会同时改变CMOS时间，这一点是共通的）。

#### 二、 LINUX的时间和时区

Linux和苹果操作系统以当前主板CMOS内时间做为格林威治标准时间，再根据系统设置的时区来最终确定当前系统时间（如时区设置为GMT+08:00北京时间时以及当前CMOS时间为03:00，那么系统会将两个时间相加得出显示在桌面的当前系统时间为11:00）

#### 三、 问题解决

**解决的办法有两个**

**让Windows使用Ubuntu的时间管理方式，就是启用UTC（世界协调时）**
**让Ubuntu按照Windows的方式管理时间，就是让Ubuntu禁用（世界协调时）**

个人建议第二种，因为通常Windows是主系统，不推荐对Windows进行这种修改，不过我还是都介绍一下：
1.在Windows下启用UTC
打开运行窗口（快捷键Win+R），然后输入regedit启动注册表编辑器，并找到一下目录位置：

> HKEY_LOCAL_MACHINE/SYSTEM/CurrentControlSet/Control/TimeZoneInformation/

添加一项类型为*REG_DWORD*的键值，命名为*RealTimeIsUniversal*，值为 1 然后重启后时间即回复正常

2.在Ubuntu下关闭UTC
这个用这个方法是我比较推荐的：按Ctrl+Alt+T调出终端，输入：

```bash
sudo vim /etc/default/rcS
```

找到

> UTC=yes

这一行，改成

> UTC=no

保存即可，时间修改立即生效。这样就可以解决Windows与Ubuntu双系统时间同步问题了

3.***(推荐)***
在Linux下打开终端，输入命令：

```bash
timedatectl set-local-rtc 1
```

然后再输入

```bash
timedatectl
```