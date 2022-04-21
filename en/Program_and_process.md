---
title: Program_and_process
description: 
published: true
date: 2022-04-21T03:56:49.930Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:56:47.965Z
---

[[zh:程序与进程]]


## Source code and program

Source code refers to the code written using signs from certain programming language. The code generated from source code by compiling or interpreting is called a program.


There are mainly three types of programming language:

* Compiling language: Source code must undergo compilation before they can be execute by computer. Their efficiency are relatively high. Example: C/C++.

* Interpreting language: Source code must undergo interpretation before they can be execute by computer. Their efficiency are relatively low. Example: Python.

* Script language: Created for simplify the procedure of writting-compiling-executing. Their efficiency are low. Example: JavaScript.

## Program and process

The difference between a program and a process is that the former indicates a piece of code used to offer certain functions, leaving the impression that it is a static tool; the latter, however, is dynamic, and it can be seen as a collection of status when program is running, or an instance of program, which servers as the basic unit of task scheduling in Linux.

It is better to imagine the process of executing of code from the first line to the last line.

A process consists of following elements:

* Process context, i.e. current status of running

* Current directory when executing

* Files and directory that are being read or written

* Access permission, such as file opening mode and ownership

* System resource allocated by system, such as memory

In Linux system, process is used by kernel to control the access of CPU and other resources, and to determine which and how should a program be run on CPU.

The scheduler allocates running times of CPU (time slices) among all process, and retrieve controls from processes when they have used up their time slices.

Linux assigns a unique integer to each process as their ID, PID. Besides, each process has an ID of its parent process, PPID. The init process, with PID 1, is the common ancestor of all process. It is first started after Linux kernel is loaded. It is responsible for booting, starting daemons and other necessary programs as well.

The process in Linux is similar to that in Windows, as it has its own (and unique) identifier and data (process variable, external variable, task, heap, etc).


## Process type

There are four kinds of process in Linux: interaction process, batch process, daemon process and zombie process. Each kind of process has its own characters and properties.

* Interaction process: Started from shell, and can be run either in foreground or background

* Batch process: No relation with terminal. It represents a series of process to be run.

* Daemon process: Started when Linux starts, and run in background.

* Zombie process: Processes that have finished their task, but not been removed from memory.

Note: Daemon process are usually active in background. They can be actived by startup script, or by super user manually. For example, we can start http server by defining the runlevel of its script (usually found in /etc/init.d). Daemon processes are for dealing tasks required, so they are always running.

## Properties of process

* Process ID (PID): Unique number for identify the process

* Parent proces ID (PPID)

* ID of the user (UID) that starts this process and ID of its owning group (GID）

* Process status: Running (R), sleeping (S), interrupted (T) and zombie (Z).

* Prioriety of running

* Name of the terminal it connects to

* Resource usage: Such as RAM usage, CPU usage

## Parent and child process

The relationship between is parent and child process is that the manager and the managed. When a parent process is terminated, its child processes is terminated too. However, when a child process ends, its parent process does not necessarily end.

## Reference

[鸟哥私房菜:什么是程序]（http://vbird.dic.ksu.edu.tw/linux_basic/0440processcontrol.php#whatis）

[Linux进程控制]（http://wiki.linuxdeepin.com/index.php/%E8%BF%9B%E7%A8%8B%E6%8E%A7%E5%88%B6）

[haose的博客：Linux进程]（http://xuzhigang921.blog.163.com/blog/static/56199220201123122639986/）