---
title: Linux下如何创建systemd启动项实现开机自启动
description: 
published: true
date: 2022-10-25T07:03:50.787Z
tags: systemd
editor: markdown
dateCreated: 2022-06-28T05:31:32.494Z
---

# Linux下如何创建systemd启动项实现开机自启动
以下所有操作都需要root权限，请使用`sudo -i`。

创建如下文件：

```perl
/etc/systemd/system/myservice.service
```

内容：

```ini
[Unit]
Description=my service
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=root
ExecStart=/path-to/myservice.sh

[Install]
WantedBy=multi-user.target
```

------

`/path-to/myservice.sh`是要运行的程序，它需要一直保持运行，不会退出。如果`/path-to/myservice.sh`退出，`systemd`会以1秒一次的频率不断重启`/path-to/myservice.sh`。

如果你的`/path-to/myservice.sh`在运行完成后会退出，不需要自动重启，请把`Restart=always`改成`Restart=no`。

------

`After=network.target`表示联网后才会启动这个服务。如果你需要联网前启动，可以删掉。