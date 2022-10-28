---
title: Linux如何使用nftables作为防火墙
description: 
published: true
date: 2022-10-25T07:15:58.510Z
tags: 防火墙 nftables
editor: markdown
dateCreated: 2022-06-28T05:36:09.915Z
---

# Linux如何使用nftables作为防火墙
更多nftables使用经验，参考ArchWiki：

[https://wiki.archlinux.org/title/Nftables_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly93aWtpLmFyY2hsaW51eC5vcmcvdGl0bGUvTmZ0YWJsZXNfJTI4JUU3JUFFJTgwJUU0JUJEJTkzJUU0JUI4JUFEJUU2JTk2JTg3JTI5)

------

`nftables`是Linux中新的包过滤前端命令行，用于替代经典的包过滤命令行`iptables`。我个人认为它比`iptables`更易学易用，而且功能比`iptables`更加强大。

`nftables`的命令是`nft`（不是全称`nftables`），如果你系统里没有，可以自己安装一下`nftables`软件包，比如：

```undefined
sudo apt install nftables
```

本文介绍使用`nftables`作为防火墙的方法。`nftables`和`iptables`一样也可以用于网络地址转换（`NAT`）和策略路由，这里不做介绍。

------

用`nftables`作为防火墙：

- 创建新的防火墙规则表，名叫`inet`。

  ```sql
   nft add table inet filter
  ```

- 把防火墙规则表`inet`绑定到网络输入（`input`）、转发（`forward`）、输出（`output`）端口。

  - 从`nftables`的视角来看，这实际上是向规则表`inet`中添加了三个名为`input` `forward` `output`的规则链，并分别绑定到三个名为`input` `forward` `output`的挂钩点（`hook`）。

  - 优先级（`priority`）是`0`。如果有多个链，优先级数值越小的链越先执行。可以为负数。

  - 默认规则为

    ```
    accept
    ```

    （接受），也就是说，如果防火墙规则没有匹配，就放行流量。默认规则还可以是

    ```
    drop
    ```

    （丢弃）、

    ```
    reject
    ```

    （拒绝）。

    ```python
    nft add chain inet filter input \{ type filter hook input priority 0 \; policy accept \; \}
    nft add chain inet filter forward \{ type filter hook forward priority 0 \; policy accept \; \}
    nft add chain inet filter output \{ type filter hook output priority 0 \; policy accept \; \}
    ```

- 允许其他设备访问本机SSH服务。

  - `iif wlp3s0`表示流量来自网络接口`wlp3s0`，它是我机器上的WiFi网卡。你的WiFi网卡名称可能不同。

  - `iif`是 **i**nput **i**nter**f**ace（输入接口），相应的“输出接口”为`oif`（**o**utput **i**nter**f**ace） 。

  - 网线接口的命名方式和WiFi不一样，比如我的是`enp2s0`。不过我不用网线，所以没写它。

  - 用`ifconfig`或`ip addr show`命令可以显示所有网络接口名称。你可以根据需要分别允许。

  - 如果你想允许所有人访问SSH，无论来自哪个网络接口，可以把条件

    ```
    iif wlp3s0
    ```

    删掉。

    ```css
    nft add rule inet filter input iif wlp3s0 tcp dport 22 accept
    ```

- 拒绝其他设备访问本机的其他服务。

  - 因为我们先添加了前一条规则，所以局域网设备还是可以访问我们的SSH服务。

  - `ct state new`是这个规则的关键，它表示“新建连接”。在输入（`input`）方向收到新连接，那肯定就是想访问本机的其他服务了，于是我们拒绝（`reject`）

  - `ct`的全称是`connection tracking`（连接追踪），它同时适用于TCP、UDP、ICMP等流量。所以`ct state new`规则对UDP流量也能起到防护作用，不只适用于TCP。

  - 注意，这里必须要写网络接口，不能删掉

    ```
    iif xxxxxx
    ```

    条件，否则会有巨大的副作用，导致本机也无法访问自身的服务，并且容器类程序（

    ```
    docker
    ```

    、安卓模拟器等）可能会无法联网。

    ```sql
    nft add rule inet filter input iif wlp3s0 ct state new reject
    ```

- 阻止其他设备访问本机容器内的服务。

  - 上面那条规则阻止了`input`（输入）方向的连接，但是防护并不完善。`docker`这样的容器会添加网络地址转换（NAT）规则，把流量直接转发到容器所在的虚拟网卡，不会经过`input`链。

  - 所以即使有上面那条规则，也无法阻止其他设备访问本机容器内的服务。要实现阻止，必须在`forward`（转发）方向禁止。

  - 网络接口条件

    ```
    iif xxxxxx
    ```

    不能省略，否则会有巨大副作用：本机也无法访问容器内的服务。

    ```sql
    nft add rule inet filter forward iif wlp3s0 ct state new reject
    ```

------

上述规则对 IPv4 和 IPv6 都能生效。

------

- 列出现有防火墙规则。

  - 除了你自己创建的规则，你也会看到其他程序创建的规则。

  - table `ip` 和 `ip6` 是 `iptables` 和 `ip6tables` 创建的规则。

  - 每条规则后面都有`# handle N`标记，那是规则的序号，用于删除规则。如果不想显示`handle`，可以不加`-a`参数。

  - 建议不要动其他程序创建的规则，也不要把规则放进其他程序创建的table里，防止副作用。如果只向自己创建的table添加规则，出问题时只要把自己的table删除就能解决问题，不需要重启。如果搞乱了其他程序创建的table，出问题就只能重启了。

    ```css
    nft list ruleset -a
    ```

输出示例：

```python
table inet filter { # handle 11
        chain input { # handle 1
                type filter hook input priority 0; policy accept;
                iif "wlp3s0" tcp dport ssh accept # handle 4
                iif "wlp3s0" ct state new reject # handle 5
        }

        chain forward { # handle 2
                type filter hook forward priority 0; policy accept;
                iif "wlp3s0" ct state new reject # handle 6
        }

        chain output { # handle 3
                type filter hook output priority 0; policy accept;
        }
}
```

------

- 删除刚刚添加的防火墙规则

  - 从上面的命令输出可以看到，我们添加的

    ```
    inet
    ```

    表的序号是

    ```
    11
    ```

    ，所以可以这样删除：

    ```sql
    nft delete table handle 11
    ```

- 删除单条防火墙规则

  - 删除序号为4的规则

  - 也就是`if "wlp3s0" tcp dport ssh accept # handle 4`

  - ```
    input
    ```

    是规则所在的链（

    ```
    chain
    ```

    ），也必须写上去。

    ```css
    nft delete rule inet filter input handle 4 
    ```

------

完整脚本：

这是一个每次执行都会刷新现有规则的脚本。你只需要在脚本里修改规则，然后执行一次脚本就能生效了。

```bash
#!/bin/bash
set -e


# 删除现有规则

handle="$(nft list ruleset -a | grep 'table inet filter' | awk '{print $7}')"
if [ "$handle" != "" ]; then
    nft delete table handle "$handle"
    echo "old inet table deleted"
fi


# 添加新规则

nft add table inet filter

nft add chain inet filter input \{ type filter hook input priority 0 \; policy accept \; \}
nft add chain inet filter forward \{ type filter hook forward priority 0 \; policy accept \; \}
nft add chain inet filter output \{ type filter hook output priority 0 \; policy accept \; \}

nft add rule inet filter input iif wlp3s0 tcp dport 22 accept
nft add rule inet filter input iif wlp3s0 ct state new reject

nft add rule inet filter forward iif wlp3s0 ct state new reject

echo "rule added"


# 显示生效规则

nft list ruleset
```

------

使用`systemd`让上面的脚本开机自启动

1. 创建文件`/usr/local/bin/nftables.sh`，内容如上。
2. 创建`systemd`启动项文件`/etc/systemd/system/nftables.service`，内容如下：

```ini
[Unit]
Description=my nftables firewall
After=network.target

[Service]
Type=simple
Restart=no
User=root
ExecStart=/usr/local/bin/nftables.sh

[Install]
WantedBy=multi-user.target
```

1. 设为开机自启动：

```bash
sudo systemctl enable nftables
```

`systemd`启动项的更多用法详见：[https://hu60.cn/q.php/bbs.topic.101984.html](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy4xMDE5ODQuaHRtbA..)