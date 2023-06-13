---
title: Linux系统时间老是不准怎么办
description: 
published: true
date: 2023-06-13T01:37:13.146Z
tags: 
editor: markdown
dateCreated: 2023-06-13T01:37:13.146Z
---

- - 

### 一、使用 NTP 服务时间同步

1. 安装 ntp

```
[root@node ~]# yum -y install ntp
```

1. 启动 ntp 服务

```
[root@node ~]# systemctl start  ntpd
[root@node ~]# systemctl enable  ntpd
Created symlink from /etc/systemd/system/multi-user.target.wants/ntpd.service to /usr/lib/systemd/system/ntpd.service.
```

