---
title: 控制中心更新失败、连接VPN失败，如何查看详细错误日志
description: 
published: true
date: 2022-06-16T03:16:20.670Z
tags: 
editor: markdown
dateCreated: 2022-06-16T03:14:41.511Z
---

# 控制中心更新失败、连接VPN失败，如何查看详细错误日志
## 控制中心更新失败,查看log

终端输入命令: cat ~/.cache/deepin/dde-control-center/dde-control-center.log

搜索UpdateFailed关键字, 就能看到具体升级失败的原因

特别说明:

可能在控制中心会存在多个错误信息, 最好是先删除上面的日志, 再重启控制中心(必须要重启,才能生成新的dde-control-center.log), 这样就能保证只有一个UpdateFailed;

假如不想删除dde-control-center.log, 则可以根据时间去查看UpdateFailed出现的信息

## 连接VPN失败
终端输入命令: sudo journalctl -b -f -u NetworkManage