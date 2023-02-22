---
title: 安装openresty
description: deepin下编译安装openresty
published: true
date: 2022-10-25T02:52:42.485Z
tags: nginx, openresty
editor: markdown
dateCreated: 2022-09-18T03:32:24.159Z
---

## 简介

OpenResty是一个基于Nginx和Lua的高性能Web平台，其内部集成了大量精良的Lua库、第三方模块以及大多数的依赖项。用于方便地搭建能够处理超高并发、扩展性极高的动态 Web 应用、Web 服务和动态网关。

可以简单理解成集成了lua插件的nginx。

## 环境

- 操作系统：deepin v20.7 x86_64
- openresty版本：1.21.4.1

## 步骤

假设用户名为admin, 拥有sudo管理权限。

1. 下载、解压、创建目录

```shell
mkdir -p /home/admin/apps/openresty
wget https://openresty.org/download/openresty-1.21.4.1.tar.gz
tar xf openresty-1.21.4.1.tar.gz -C /home/admin/apps
cd /home/admin/apps/openresty-1.21.4.1
```

2. 安装依赖

```shell
sudo apt install -y libpcre3-dev openssl libssl-dev libxml2-dev libgd-dev libxml2 libgeoip-dev libxslt-dev
```

3. 预编译。这里将默认不编译的部分参数加上了，另外还需注意 `--prefix` 指的是实际安装目录

```shell
./configure --prefix=/home/admin/apps/openresty \
--with-threads \
--with-file-aio \
--with-http_ssl_module \
--with-http_v2_module \
--with-http_realip_module \
--with-http_addition_module \
--with-http_xslt_module=dynamic \
--with-http_image_filter_module=dynamic \
--with-http_geoip_module=dynamic \
--with-http_sub_module \
--with-http_dav_module \
--with-http_flv_module \
--with-http_mp4_module \
--with-http_gunzip_module \
--with-http_gzip_static_module \
--with-http_auth_request_module \
--with-http_random_index_module \
--with-http_secure_link_module \
--with-http_degradation_module \
--with-http_slice_module \
--with-http_stub_status_module \
--with-stream_ssl_module \
--with-stream_realip_module \
--with-stream_geoip_module=dynamic \
--with-stream_ssl_preread_module \
--with-compat \
--with-pcre-jit
```

4. 编译并安装。如果CPU为多核，比如4核，可以为`make`添加 `-j4` 参数加快编译速度

```shell
make
make install
```

5. 执行完以上操作后, 在`/home/admin/apps/openresty`目录下应该多了不少东西。
6. 验证。

   1. 编辑 `/home/admin/apps/openresty/nginx/conf/nginx.conf` , 将 `listen 80;` 改成 `listen 8000;` 
   2. 切换到 `/home/admin/apps/openresty/bin` 目录, 执行命令 `./openresty` 启动。
   3. 浏览器访问 `http://127.0.0.1:8000`
   4. 出现 openresty 欢迎信息页的话，说明安装成功。
7. (可选)将`/home/admin/apps/openresty/bin`目录添加到环境变量`PATH`中，便于以后直接使用，不需要再手动切换目录。