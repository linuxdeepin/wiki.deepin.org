---
title: Xrandr常见用法
description: 
published: true
date: 2022-06-16T06:47:17.214Z
tags: 
editor: markdown
dateCreated: 2022-06-16T06:47:15.280Z
---

# Xrandr常见用法
## 概述
xrandr用来设置和查询屏幕相关信息配置

## 查询
xrandr 查看当前所连接的屏幕通用信息
xrandr -q 命令可以查询当前的显示器状态
xrandr --verbose 将会显示更详细的信息
## 设置分辨率
设置分辨率时需要指定设置的 ouput 以及 mode, 如将 eDP1 的分辨率改为 1920x1080:

 xrandr --output eDP1 --mode 1920x1080 （1920x1080此处不可用1920*1080替代）
## 设置屏幕亮度
xrandr --output eDP1 --brightness 0.5 将eDP1的屏幕亮度设置为0.5
## 设置屏幕刷新率
xrandr --output eDP1 --mode 1920x1080 --rate 75 将eDP1的屏幕的刷新率设置为75（注意，不同的分辨率可用刷新率不同，用xrandr可查询）
## 设置屏幕角度
xrandr --output eDP1 --rotate left ( left 向左旋转90度,right 向右旋转90度, inverted 上下翻转,normal 回到正常角度)
## 指定主屏
如现在有两个 output, 分别是 eDP1 和 VGA1.通过指定 --primary 参数来设置主屏, 如设置 eDP1 为主屏:

xrandr --auto --output eDP1 --primary ，--auto 可以自动启用关闭的屏幕.
## 复制模式
复制模式最好使用两个显示器都有的 mode 作为默认的 mode

xrandr --auto --output eDP1 --pos 0x0 --mode 1440x900 --output VGA1 --same-as eDP1
## 扩展模式
 xrandr --auto --output eDP1 --pos 0x0 --mode 1920x1080 --primary --output VGA1 --mode 1440x900 --right-of eDP1
 
将 VGA1 会在 eDP1 的右边, eDP1 为主屏, 另外位置的参数还有 left-of, --above, --below 等.

 xrandr --auto --output eDP1 --pos 0x0 --mode 1920x1080 --primary --output VGA1 --mode 1440x900 --pos 1920x0 eDP1
 
xrandr --output eDP1  --mode 1920x1080 --rate 60 --pos 0x0 --output VGA1 --mode 1440x900 --rate 75 --pos 1000x1080 --brightness 0.5 --auto 
## 单屏模式
这个模式是只显示某一个屏幕, 如只显示 eDP1

 xrandr --output eDP1 --pos 0x0 --mode 1920x1080 --primary --output VGA1 --off