---
title: Linux防火墙配置iptables和firewalld
description: 
published: true
date: 2023-03-27T03:16:32.355Z
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

**规则链类型**

> INPUT     处理入站的数据包（处理目标是本机的数据包） OUTPUT    处理出站的数据包（处理源是本机的数据包，一般不在此链上做规则） PREROUTING   在进行路由选择前处理数据包（一般用来做 NAT Server） POSTROUTING   在进行路由选择后处理数据包（一般用来做源 NAT） FORWARD     处理转发的数据包（处理经过本机的数据包）

**Iptables 控制类型**

> ACCEPT       允许数据包通过 DROP          丢弃数据包（不给对方回应，一般工作时用这个） REJCET       拒绝数据包通过（会给对方回应，对方知道自己被拒绝） SNAT       修改数据包的源地址 DNAT       修改数据包的目的地址 MASQUERADE   伪装程一个非固定的公网 IP 地址 LOG       在 / var/log/messages 文件中记录日志信息，然后将数据包传递给下一条规则

**数据包到达防火墙根据下图进行匹配**

> iptables 进行数据处理关心的是四表五链以及流量的进出

![2023-3-27_20599.png](/2023-3-27_20599.png)

**Iptables 命令配置**

**配置 iptables 防火墙时需要将防火墙服务开启**

> systemctl start firewalld  开启防火墙 systemctl status firewalld 查看防火墙状态

![2023-3-27_12558.png](/2023-3-27_12558.png)

**Iptables命令查看防火墙**

> iptables -nL -t nat  查看 nat 表的规则链 -n 使用数字形式显示输出结果（如：通过 IP 地址） -L 查看当前防火墙有哪些策略 -t 指定查看 iptables 的哪个表（默认是 filter 表）

![2023-3-27_39963.png](/2023-3-27_39963.png)

**Iptables命令配置防火墙**

```
> iptables -P INPUT DROP将 INPUT 规则链的默认流量更改为拒绝
> -P 设置 / 修改默认策略
> iptables -t filter -I INPUT -s 192.168.10.0/24 -j ACCEPT在 filter 表下的 INPUT 规则链中配置规则
> -I num 插入规则（大写 i，默认在链的开头加入规则，可以指定序号）
> -i  从这块网卡流入的数据
> -o 从这块网卡流出的数据
> -s  源地址（加！表示取反）
> -d 目的地址
> -j  限制动作
> iptables -A INPUT -p tcp --dport 1000:1024 -j REJECT拒绝 tcp 端口号为 1000~1024 的数据包
> -A 在链的末尾加入规则
> -p 指定协议类型
> --sport 源端口
> --dport 目的端口

注意事项
当创建的规则内容与已有规则一致时，不会覆盖原先的规则，会直接加入到现有规则链中（即此时此规则链下有两条一摸一样的规则，只是顺序不同） 以上关于防火墙的配置是 runtime 模式，即配置成功后立即生效，但是重启后会失效（需要将配置保存，重启后才不会失效）
firewalld 讲解
frewalld 是服务名称，firewall-cmd 和 firewall-config 是配置工具名称
firewall-cmd  基于命令行配置
firewall-config 基于图形化界面配置（这两个配置方式实时同步）
Firewalld 区域概念
默认所有网卡都是 public 区域，可以根据需要将网卡设置为不同的区域


> iptables -D INPUT 1删除 INPUT 规则链的第一条规则
> -D num 删除规则链
> -R 修改规则
> iptables -F清空已有的策略
> iptables-save来保存防火墙策略
```

```
> Trust信任区域
> 允许所有流量（所有的网络连接都可以接受）
> Public公共区域
> 仅接受 ssh、dhcpv6-client 服务连接（默认区域）
> External外部区域
> 仅接收 ssh 服务连接（默认通过此区域转发的 IPv4 流量将会进行地址伪装）
> Home家庭区域
> 仅接受 ssh、msdns、ipp-client、samba-client、dhcpv6-client 服务网络连接
> Internal内部区域
> 同 home 区域
> Work工作区域
> 仅接受 ssh、ipp-client、dhcpv6-client 服务连接
> Dmz隔离区域（非军事区域）
> 仅接收 ssh 服务连接
> Block限制区域
> 拒绝所有传入流量（有回应）
> Drop丢弃区域
> 丢弃所有传入流量（无回应）
```

> **数据包到达防火墙匹配规则**
>
> **firewall 进行数据处理只关心区域**
>
> - 1.根据数据包的源 IP 地址匹配，根据源地址绑定区域的区域规则进行匹配（如果没有绑定区域则匹配默认区域的规则）
> - 2.根据传入的网络接口匹配，进入此接口绑定区域的区域规则进行匹配（如果没有绑定区域则匹配默认区域的规则）**绑定源地址的区域规则＞网卡绑定的区域规则＞默认区域的规则**
>
> **Firewalld 两种配置方法**
>
> **临时配置（runtime 当前生效表）**
>
> > 立即生效，重启后失效,不中断现有连接,无法修改服务配置**永久配置（permanent 永久生效表）**
>
> > 不立即生效，重启后生效，或者立即同步后生效 会终端现有连接 可以修改服务配置
>
> firewall-cmd 命令行基础配置
>
> **如何实现永久配置**
>
> > --permanent表示此配置加入到永久生效（默认临时生效） 或者在配置结束后执行此命令 firewall-cmd --runtime-to-permanent 将临时更改为永久 永久配置完成后需要立即同步 firewall-cmd --reload立即同步永久配置
>
> **查看默认区域并进行更改**

> firewall-cmd --get-zones               查询可用的区域
> firewall-cmd --get-default-zone          查询默认区域的名称
> firewall-cmd --get-active-zone           显示当前正在使用的区域与网卡名称
> firewall-cmd --set-default-zone=trusted    设置默认区域为 trusted 区域