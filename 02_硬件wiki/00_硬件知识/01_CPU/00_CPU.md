---
title: 00_CPU
description: CPU基础知识
published: true
date: 2022-10-17T06:30:38.836Z
tags: 硬件, 处理器
editor: markdown
dateCreated: 2022-05-05T04:18:12.514Z
---

# CPU 中央处理器
## CPU简介
CPU是整个计算机的运算大脑，是计算机最重要的部分之一

## CPU架构
CPU架构可以理解为CPU的规范，不同的架构使用不同的指令集。
指令集分为两种，一种是CISC复杂指令集，一种是RISC精简指令集，两种指令集各有优势
目前CPU的主流架构有PC端的x86，MIPS，移动端的ARM，RISC-V以及我国自研LoongArch架构
x86处理器是个人计算机以及服务器上最常见的架构，同时也是大多数Linux发行版支持的架构，deepin提供x86_64（AMD64）镜像下载

ARM处理器主要用于移动端，网络设施以及低功耗设备。几乎所以的手机和路由器都是ARM处理器，目前，部分Linux发行版提供ARM镜像下载

LoongArch处理器是我国自主研发的CPU架构，目前已经得到很多主流开源社区的支持，Linux kernel 5.19已经初步支持龙芯架构

## CPU中断
中断其实就是由硬件或软件所发送的一种称为IRQ（中断请求）的信号。 中断允许让设备，如键盘，串口卡，并口等设备表明它们需要CPU。 一旦CPU接收了中断请求，CPU就会暂时停止执行正在运行的程序，并且调用一个称为中断处理器或中断服务程序（interrupt service routine）的特定程序

## CPU参数
CPU参数一般是由处理器厂商对用户提供的有关于处理器的详细信息，如核心数、主频、电压等