---
title: 什么是dbus
description: 
published: true
date: 2022-10-25T02:17:38.339Z
tags: dbus
editor: markdown
dateCreated: 2022-06-24T05:46:16.681Z
---

# 什么是dbus
DBUS是一种高级的进程间通信机制，主要用于进程间函数调用以及进程间信号广播。一般来说，dbus会提供Methods，Properties和Signals。

- Methods 函数调用：

DBUS可以实现进程间函数调用，进程A发送函数调用的请求（Methodcall消息），经过总线转发至进程B。进程B将应答函数返回值（Method return消息）或者错误消息（Error消息）。

- Properties 属性：

Dbus可以设置相关属性，当某个属性发生变化，dbus会往下发一个signal通知其属性改变。

- Signal 消息广播：

进程间消息广播（Signal消息）不需要响应，接收方需要向总线注册感兴趣的消息类型，当总线接收到“Signal消息”类型的消息时，会将消息转发至希望接收的进程。