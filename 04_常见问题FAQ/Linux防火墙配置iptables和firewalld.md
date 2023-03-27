---
title: Linux防火墙配置iptables和firewalld
description: 
published: true
date: 2023-03-27T03:12:36.844Z
tags: 
editor: markdown
dateCreated: 2023-03-27T02:31:18.131Z
---

# Linux防火墙配置iptables和firewalld
### 防火墙基本概念

> 防火墙就是根据系统管理员设定的规则来控制数据包的进出，主要是保护内网的安全,目前 Linux 系统的防火墙类型主要有两种：分别是 [iptables] 和 firewalld

**Iptables-静态防火墙**

> 早期的 Linux 系统中默认使用的是 iptables 防火墙，配置文件在 / etc/sysconfig/iptables

#### 主要工作在[网络层]

> 使用链式规则，只可以过滤互联网的数据包，无法过滤从内网到内网的数据包 Iptables 只可以通过命令行进行配置 Iptables 默认是允许所有，需要通过拒绝去做限制 Iptables 在修改了规则之后必须得全部刷新才可以生效，还会丢失连接（无法守护进程）

**Firewalld-动态防火墙**

> 取代了之前的 iptables 防火墙，配置文件在 / usr/lib/firewalld 和 / etc/fiewalld 中,主要工作在网络层,新增区域概念，不仅可以过滤互联网的数据包，也可以过滤内网的数据包,Firewalld 不仅可以通过命令行进行配置，也可以通过图形化界面配置,Firewalld 默认是拒绝所有，需要通过允许去放行,Firewalld 可以动态修改单条规则，动态管理规则集（允许更新规则而不破环现有会话和连接，可以守护进程）

**注意事项**

> iptables 和 firewaldl 都只是 linux 防火墙的管理程序，真正的防火墙执行者是位于内核的 netfilter，只不过 firwalld 和 iptables 的结果以及使用方法不一样 在配置防火墙时，不建议两种配置方法结合使用（建议只使用其中的一种）

#### Iptables 讲解

> Iptables 配置防火墙依靠四个部分实现：表、规则链、规则（匹配条件）、控制类型组成

**Iptables 表**

> 处理优先级由高到低，表与表之间都是独立的

**raw表**

> 是否对某个数据包进行状态追踪（包含 OUTPUT、PREAUTING 两个规则链）

**mangle表**

> 修改数据包内容；可以做流量整形、对数据包设置标记（包含所有规则链）

**nat表**

> 负责地址转换功能；修改数据包中的源目 IP 地址或端口（包含 IN、OU、PR、PO 三个规则链）

**filter表**

> 负责过滤数据包；对数据包时允许放行还是不允许放行（包含 IN、OU、FO 三个规则链） Iptables 规则链

**什么是规则链**

> 很多个规则组成一个规则链,数据包从上往下做匹配，匹配成功就结束匹配，并执行相应的控制类型（建议需要将精准的策略放在上面）