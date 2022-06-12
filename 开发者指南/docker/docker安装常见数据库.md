---
title: docker安装常见数据库
description: 使用docker安装常见数据库
published: true
date: 2022-06-12T12:20:00.051Z
tags: docker
editor: markdown
dateCreated: 2022-06-12T12:20:00.051Z
---

## 前言

本文只记录基本的安装，详细参数没具体配置。
生产高并发情况下一般不建议将MySQL这样的关系型数据库运行在docker容器中。

## MySQL

```shell
# 1. 拉取docker容器（mysql v5.7）
docker pull mysql:5.7

# 2. 创建容器并运行
# 映射宿主机3307端口到容器的3306端口
# 设置mysql的root密码为123456
docker run -it --name mysql -p 3307:3306 \
	-e MYSQL_ROOT_PASSWORD=123456 \
	-d mysql:5.7
```

## Redis

```shell
# 1. 拉取docker容器（redis v6）
docker pull redis:6

# 2. 创建容器并运行
docker run -p 6379:6379 --name redis -d redis:6
```

## MongoDB

```shell
# 1. 拉取docker容器
docker pull mongo

# 2. 创建容器并运行
docker run -p 27017:27017 --name mongodb \
  -e MONGO_INITDB_ROOT_USERNAME=root \
  -e MONGO_INITDB_ROOT_PASSWORD=123456 \
  -d mongo:latest
```

## PostgreSQL

```shell
# 1. 拉取容器
docker pull postgres

# 2. 创建容器并运行
docker run -p 5432:5432 --name postgresql \
  -e POSTGRES_PASSWORD=123456 \
  -d postgres
```

## RabbitMQ

```shell
# 1. 拉取容器（带Web管理界面版本的镜像）
docker pull rabbitmq:management

# 2. 创建容器并运行
# 15672是web管理端访问端口
docker run --name rabbitmq -p 5672:5672 -p 15672:15672 \
  -e RABBITMQ_DEFAULT_USER=rbmq \
  -e RABBITMQ_DEFAULT_PASS=rbmq \
  --hostname=rabbitmqhosta \
  -d rabbitmq:management
```

## Memcache

```shell
# 1. 拉取容器
docker pull memcached

# 2. 创建容器并运行
# 映射11211端口，指定最大容量为128m。不设置的话，最大容量为64m
docker run --name memcache -p 11211:11211 -d memcached -m 128
```