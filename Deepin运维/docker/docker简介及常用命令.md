---
title: docker简介及常用命令
description: 
published: true
date: 2022-10-25T02:04:02.469Z
tags: 
editor: markdown
dateCreated: 2022-06-23T08:53:16.224Z
---

# docker简介及常用命令

## 一． Docker介绍及基本命令

Docker是一种遵从Apache2.0协议开源的Linux容器管理解决方案，它通过进程和进程通信技术对操作系统的文件资源和网络的进行隔离，实现了包含文件资源、系统资源(shell环境等)以及网络资源的容器创建和管理。每一个容器都有一个唯一的进程，当该进程结束的时候，容器也会完全的停止。 适用于Linux平台（仅适用）

Docker 官网：http://www.docker.com
Github Docker 源码：https://github.com/docker/docker
帮助文档：https://docs.docker.com

## 二．Docker和虚拟机的区别

虚拟机实现资源隔离和环境隔离的方法是利用独立的OS，并利用Hypervisor（Hypervisor是一种运行在物理服务器和操作系统之间的中间软件层,可允许多个操作系统和应用共享一套基础物理硬件。当服务器启动并执行Hypervisor时，它会给每一台虚拟机分配适量的内存、CPU、网络和磁盘，并加载所有虚拟机的客户操作系统。）虚拟化CPU、内存、IO设备等实现的。 Docker容器是共享一个操作系统内核（kernel）的，这些容器通过命名空间相互独立。Docker有着比虚拟机更少的抽象层。由于Docker不需要Hypervisor实现硬件资源虚拟化，运行在Docker容器上的程序直接使用的都是实际物理机的硬件资源。

## 三．Docker的应用场景

Web 应用的自动化打包和发布。 自动化测试和持续集成、发布。 多环境的部署切换。 复杂环境一键配置。 在服务型环境中部署和调整数据库或其他的后台应用。 在openstack等iaas云平台上搭建自己的PaaS环境。 从头编译或者扩展现有的OpenShift或Cloud Foundry平台来搭建自己的PaaS环 境。 同应用多版本隔离、文件隔离。

## 四．Docker的优缺点

优点：

1. Docker提供隔离的运行环境 文件系统隔离 网络隔离 进程号隔离 进程间通信隔离
2. 容器性能开销极低 Docker技术虽然是虚拟化技术，却几乎不消耗除容器中的应用程序外的其他资源， 可以达到近乎裸机的运行能力，达到秒级/微秒级的部署，一台实体机可以运行 几百甚至上千个docker容器。
3. 容易移植 有很高的移植性，可以在任何平台运行（包括物理机、虚拟机、云平台）。 过去需要用数天乃至数周的环境移植任务，在Docker容器的处理下，只需要数秒 就能完成。

缺点：

1. Docker容器受到的资源限制 CPU计算资源 内存资源 磁盘I/O资源等

## 五．Docker基本命令 docker load -i xxx 加载docker镜像
```
docker rmi IMAGE_ID # 卸载docker镜像
docker images # 查看当前有那些以被加载的镜像（images）
docker ps -a # 查看当前运行的所有容器
docker stop CONTAINER_ID # 停止容器
docker rm CONTAINER_ID # 删除容器
docker run -it IMAGE_ID /bin/bash # 运行docker容器
docker exec -it CONTAINER_ID /bin/bash # 容器运行多个终端
docker cp 文件路径 NAMES   # 要拷贝到容器里面对应的路径
docker cp NAMES  文件路径  # 要拷贝的文件路径
```