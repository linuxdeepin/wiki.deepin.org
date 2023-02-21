---
title: aria2下载工具
description: 
published: true
date: 2022-10-25T02:07:02.495Z
tags: 
editor: markdown
dateCreated: 2022-06-28T07:55:53.598Z
---

# aria2 下载工具配置方法

## 前言

linux 没有迅雷（有wine版迅雷），那么 linux 上怎么下载 bt 种子呢？使用 aria2 这个命令行工具可以下载。使用方法比较简单，但是它需要经过配置才有速度。

## 配置方法

1. 需要配置种子服务器
2. 需要配置 dht.dat 文件

### 种子服务器

下载文件：<https://ngosang.github.io/trackerslist/trackers_best_ip.txt>，里面就是服务器的列表

保存到：~/.aria2/aria2.conf 配置文件中，内容如下：

```ini
# 每一个服务器地址用逗号分隔
bt-tracker=网址1,网址2,网址3...
```

但是第一步得到的地址不是以逗号分隔，而是以一个空行分隔。所以自己要修改一下，可以用 kwrite 编辑器，替换"\n\n" 为 ","。

```bash
# 获得正确格式的文本
curl -o- https://ngosang.github.io/trackerslist/trackers_best_ip.txt|sed -ne '1h; 1!H;${g;s/\n\n/,/g;s/\n//g;p}'

# 自动更新脚本
sed 's@bt-tracker=.*@bt-tracker='$(curl -o- https://ngosang.github.io/trackerslist/trackers_best_ip.txt|sed -ne '1h; 1!H;${g;s/\n\n/,/g;s/\n//g;p}')'@g' ~/.aria2/aria2.conf
```

### dht.dat

这是一个很关键的步骤，它的作用就是把可以联通的网络保存起来，那么下次下载的时候用这些网络，自然就提高了速度。否则，你还需要从有限的种子服务器里面获取内容，有个缓慢速度爬坡的过程。

总而言之，将以下内容写入配置文件：~/.aria2/aria2.conf

```ini
# aria2配置文件
# 保存为 ~/.aria2/dht.dat
enable-dht=true
bt-enable-lpd=true
enable-peer-exchange=true
# 以下是 ipv6 支持，保存为 ~/.aria2/dht6.dat
enable-dht6=true
```

关键的一步，你必须要创建一个空的文件，否则它不干活：

```bash
touch ~/.aria2/dht.dat
touch ~/.aria2/dht6.dat
```

之后就可以正常的使用 aria2 下载种子了：

```bash
# 下载指定种子文件
aria2c xxx.torrent 
# 结束之后，可以观察 dht.dat 变大了
ls -hl ~/.aria2
```

