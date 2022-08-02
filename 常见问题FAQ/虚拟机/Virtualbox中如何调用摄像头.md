---
title: Virtualbox中如何调用摄像头
description: 
published: true
date: 2022-08-02T06:32:03.189Z
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

## 2.下载 Oracle VM VirtualBox Extension Pack

VirtualBox 需要安装 Oracle VM VirtualBox Extension Pack 才能使用摄像头。

进入 VirtualBox 官网下载页面：https://www.virtualbox.org/wiki/Downloads


![官网下载 Oracle VM VirtualBox Extension Pack](https://i.loli.net/2019/03/24/5c96c81bb95f4.png)

注意：必须保证 VirtualBox 版本与 Oracle VM VirtualBox Extension Pack 版本一致。

否则会安装失败。

安装 Oracle VM VirtualBox Extension Pack
在 管理 -> 全局设定 -> 扩展 -> 新增下载好的 Oracle VM VirtualBox Extension Pack 并安装


![2022-8-2_91382.png](/2022-8-2_91382.png)