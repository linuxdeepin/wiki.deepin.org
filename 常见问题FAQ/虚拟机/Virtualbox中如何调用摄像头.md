---
title: Virtualbox中如何调用摄像头
description: 
published: true
date: 2022-08-02T06:13:24.975Z
tags: virtualbox 摄像头
editor: markdown
dateCreated: 2022-08-02T06:00:49.003Z
---

# Virtualbox中如何调用摄像头
## 1.先加入把本用户加入组 vboxusers 和 disk （需要注销后再次登录才生效）
```
sudo usermod -aG vboxusers $USER
sudo usermod -aG disk $USER
```

