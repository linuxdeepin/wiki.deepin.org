---
title: python3.10安装
description: deepin v20下安装最新版python3
published: true
date: 2022-10-25T06:03:55.238Z
tags: python
editor: markdown
dateCreated: 2022-09-18T04:11:37.671Z
---

## 前言

当前deepin v20.7 的apt源中只有python 3.7版本，而python官方已经更新到了3.10。虽然3.7和3.10没有太大的差别，但如果想要体验更高版本，本文提供手动编译安装python3.10.7的步骤。

## 步骤

这里假设当前用户名为admin，用户家目录为`/home/admin`，根据实际情况替换。

1. 浏览器访问[官方下载页](https://www.python.org/downloads/), 下载source code的压缩包。
2. 创建安装目录。
```shell
mkdir -p /home/admin/apps/python3.10
```
3. 解压压缩包

```shell
tar xf Python-3.10.7.tar.xz
cd Python-3.10.7
```

4. 预编译。提示缺少依赖的话，使用apt安装依赖。

```shell
./configure --prefix=/home/admin/apps/python3.10 --enable-optimizations
```

5. 编译并安装。CPU若是多核，比如4核，可为`make`指定`-j4`参数加速编译。

```shell
make
make install
```

6. 创建软链接

```shell
sudo ln -s /home/admin/apps/python3.10/bin/python3.10 /usr/bin
sudo ln -s /home/admin/apps/python3.10/bin/python3.10-config /usr/bin
```

7. 验证

```shell
# 这里的输出结果应该还是 python 3.7
python3 --version
# 这里的输出结果应该是 python 3.10.7
python3.10 --version
```


## 补充

- 网上有些文档说直接覆盖python3.7，但笔者以前在centos7上试过，从python3.6直接升到python3.9，部分工具直接无法使用。所以为了避免不可控问题的出现，建议还是不要覆盖原本的python3.7.