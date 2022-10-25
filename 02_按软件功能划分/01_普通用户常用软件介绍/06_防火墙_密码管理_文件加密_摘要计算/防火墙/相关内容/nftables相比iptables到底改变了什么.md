---
title: nftables相比iptables到底改变了什么
description: 
published: true
date: 2022-06-28T06:42:43.364Z
tags: 
editor: markdown
dateCreated: 2022-06-28T06:38:47.980Z
---

# nftables相比iptables到底改变了什么  

这不是一篇教你怎么可以配置nftables实现一个哪怕最简单防火墙的文章，我从来不写这种Howto，因为我觉得如果一项新技术，一个人连其本身的文档都懒得看，即便没有文档如果没有一点钻研精神将其搞懂，只靠看别人写好的Step by step的话，那真是太失败了。相反，这篇文章是一篇檄文，只为吹擂打鼓，目的是让你在无感于iptables的前提下爱上nftables。


关于nftables，请参见以下几篇文字：
1.同样一篇檄文，翻译过来的，它是《Linux 首次引入 nftables，你可能会喜欢 nftables 的理由》
2.nftables的文档，这个最具有权威性，它是《Nftables HOWTO 中文翻译》，我跟朋友说，这个文档就像当年的iptables文档一样好，我给出个中文链接，喜欢英文的请自行搜索。
3.详细剖析nftables语法以及内部结构的一篇文章，它是《What comes after 'iptables'? Its successor, of course: `nftables`》，很遗憾它没有中文翻译，不过看我的应该就够了。
4.我自己在2014年的时候写的一篇关于nftables的文章，它是《继iptables之后的新一代包过滤框架是nftables》，说实话，我确实自己又看了一遍这个。

如果你真的完整看过了以上4篇，我相信下面的文字是很轻松的。而且，你在读完下面文字的时候，应该会形成一种形而上的观点与我展开讨论，热烈欢迎！
---------------------------------

要站在历史的延长线上观看nftables，它是第X代Linux防火墙。它的前驱是iptables，再往前是ipchains，ipfirewall等等...如果你仔细观察它们的名字，就会发现，nftables中完全用nf前缀取代了ip前缀，这是不是意味着，之前类似iptables，ip6tables，arptables，ebtables等等“工具族”，完全被统一在nf-框架之上了呢？答案无疑是肯定的，nftables完全统一了所有这些，正如其名字所示，nf代表Netfilter，而不管是ipchains，iptables，ebtables，arptables，底层所依赖的机制完全就是Netfilter。nftables统一了所有这一切！伴随着这个事实，理所当然会有下面的疑问：






nftables的外部语法和内部架构该如何变化以适应这个大一统的名字呢？

        我们从名字开始，逐步开始向下挖。
---------------------------------

首先我们先从离我们最近的地方，即用户接口看起。这部分侧重表现在命令或者说语法方面。请注意，iptables的规则是没有语法的，它只是一系列命令行参数的组合，而nftables的语法也并不是完善的，起码现在还很弱，不过我们要向前看有所期待。
        有文章说nftables是借鉴了tcpdump的语法，将-a $a或者--abc $abc这种语法改成了类似a $a之类的语法，我不太认同nftables是借鉴了tcpdump，因为这是nftables自然而然的做法。
        如果是iptables时代，我们使用下面的规则：
iptables -A OUTPUT -p tcp --dport 80 -j DROP
到了nftables时代，同样的规则，会变成下面的样子：
nft add rule ip filter output tcp dport 80 drop
虽然长了些，但你会发现，所有的字词都完整地构成了一个个的句子构成元素，比如主语，谓语，宾语，状语，定语等，然后回看iptables的命令行参数，几乎全部都是限定性的，比如出现-p，就意味着后面将要出现的就是协议...在nftables中，没有这种限制了。映射到程序中，iptables可以用getopt这种方式处理“命令行参数”，请注意，iptables将所有的子命令都作为了命令行参数来看待，而nftables则完全不同，它处理子命令的过程完全是语法分析和词法分析的过程，所有你敲打进去的每一个单词都将构成一颗树上的节点，nftables是有“真正的语法”的，所以说，你配置nftables规则的过程，就是一个编程的过程。编程的过程是一个直观且简洁的过程，就像人们自然而然的写下一个句子一样。

        外部语法的升华背后，那是内部结构的调整。
---------------------------------

nftables的内部结构与iptables相比，有哪些变化呢？
        首先，我们先看下规则的布局。
        我们知道，iptables规则的布局是基于连续的大块内存的，我之前曾经称之为数组式布局，写到这里，我想你应该知道我想说什么了，既然iptables规则是数组式布局，那么nftables规则会不会是链表式布局呢？答案无疑是肯定的。那么iptables规则的布局和nftables规则的布局之间的区别，其实就是数组和链表的区别了。
        举个最简单的例子，数组的元素要是被删除了，保证顺序不变的前提下，后面所有的元素都要往前移动，试想10000个元素的数组删除array[1]的情况，链表就不存在这个问题，只需要修改一个或者几个指针即可。当然，数组还是有好处的，内存紧凑，cache命中率高，可以索引定位什么的...
        当然，我并不晓得iptables当初为什么要采用这种数组式的布局，这很值得深挖一下，但却不是本文的主题。离开这个话题前，我必须要说的是，对于我个人来说，在快速实现一个可以测试的功能的编程过程中，如果需要容器，我首选的就是数组，因为在实验阶段它更直接，且不会遇到O(n)问题...最后等真的遇到问题且有替换数组为链表的十足理由了，我才会去重构成链表式的实现，否则它就在那里了。
        看完了规则的布局差异，我们继续往下看。
        如果你熟悉iptables的内核机制(如果不知道，请Stop，本文不是介绍这方面的Howto)，你就会知道它实际上是一个非常规则且简单的“机械装置”，这套装置明令要求所有的规则必须由若干的matches和一个target组成，我们经常在肯德基或者老式工厂(比如老式机床厂)的车间见到类似的这种装置。这类装置的特点是“工序是固定的”！我以煎蛋为例，如果我想煎三个鸡蛋，并且我有一把足够大的平底锅，我会一次性把三个鸡蛋全部打在刷了足够植物油的锅底，这也是一种自然而然的做法，然而如果用类似iptables内核机制那样的煎锅，不管锅有多大，每次只能煎一个鸡蛋，如果你想煎三个鸡蛋，很简单，重复三次且只能重复三次。这种装置是“不可编程的”！
        我们生活的世界不是这样子的！试想你去一趟超市只能买一样东西，并且不能再去别的地方，现在需要你买来做午饭的所有食材，鉴于超市的蔬菜可能不太新鲜，你要去路对面的蔬菜店单独购买蔬菜，怎么办？使用iptables装置是“可以实现的”，但也仅仅是可以实现而已，除非我们是机器人，否则这种世界是不令人舒适的！我们更容易适应的世界应该是这样子：大早上起床先去海鲜市场买条鱼，然后拎着鱼去蔬菜店买蔬菜，然后拎着鱼和蔬菜寄存在超市，进入超市购买各种肉类，酒水饮料以及家里需要的日用品，结完帐取出寄存的鱼和蔬菜，拎回家或者一起放入汽车的后备箱。
        幸运的是，这就是nftables的方式！
        nftables不再固定“机械装置的行为”，行为完全由你自己来确定，它只是实现了几个足够独立的动作，组合这些动作你几乎可以实现任何操作，事实上，这种新的装置解构了行为，将行为分解成了更加基础的动作，你让它执行什么动作它就执行什么动作，怎么组合这些动作以及最终完成什么样的行为，全在你。这装置是什么？这好像跟电脑很像。其实跟我们人本身更像！稍微术语一点，这就是一部虚拟机。nftables竟然在Linux内核协议栈里实现了一个虚拟机！其实不止一个，而是5个！Netfilter的5个HOOK点上均有一部这样的虚拟机。
        和以往固定的机械装置不同，你灌输给虚拟机的不光是数据，还有程序，而固定的机械装置只能灌输给其数据，因为程序是固定的。
        回过头来我们再来看一下nftables的语法特点，一条规则其实就是一个程序，一个简单的程序就是一个描述性的祈使句，谁，在什么地点，什么时候，用什么，做什么事：
nft add rule ip filter output tcp dport 80 drop
如果想看个究竟，那么加上 --debug=netlink，我们看下其输出：
ip filter output
  [ payload load 1b @ network header + 9 => reg 1 ]
  [ cmp eq reg 1 0x00000006 ]
  [ payload load 2b @ transport header + 2 => reg 1 ]
  [ cmp eq reg 1 0x00005000 ]
  [ immediate reg 0 drop ]
非常明确的一个程序，简直就是另一个Arch上的汇编程序代码，用人话描述这个程序，就是：
从数据包IP头开始算，取出数据包的第10个字节开始的1字节并把它放入reg 1，比较reg 1的值是不是0x06，如果是的话，以数据包TCP头开始算，取出第3个字节开始的2个字节放入reg 1，看看它是不是80，如果是，就指示这个数据包应该丢掉。
关于这个论点，我觉得再继续说下去就有点拖沓了，用一下nftables便知，如果不知，看下其内核代码，比iptables的要简单。

        现在，说点关于人的。
---------------------------------

程序员一般都不懂怎么配iptables规则，甚至都不懂网络，而会配iptables规则的或者会配网络的人一般也不会编程，他们可能是运维管理员，我一直都徘徊在两个阵营之间，最终我自己就死皮赖脸刀枪不入了。我就是个墙头草，跟程序员在一起的时候，说那帮运维只懂搬机器，只会键盘上敲命令，根本就无力修改命令的行为，而编程才是最高贵的随心所欲。然而跟运维在一起的时候，我会说，编程者只会写代码，以为网络就是Socket，充其量就是TCP，根本就不懂IP路由协议，更别提硬件特征比如网卡指示灯，电源了，出了问题他们根本就无力定位，只会撸代码。....虽然说这种自嘲其实是一种自我吹捧，以显得自己什么都会，但如果大家都用nftables的话，我便无力吐槽了。
        nftables的运维者成了编程者。
        因为nftables规则的编写过程就是一个编程的过程，你可以对不服者如下呐喊：
汇编语言和C语言写的程序喂的是CPU这个虚拟机；
Java写的程序喂的是JVM这个Java虚拟机；
nftables编写的规则喂的是Netfilter钩子点上内置的虚拟机。
只要想象一下就兴奋，Netfilter钩子点竟然可以内置虚拟机了，那么如此一来，所有的完成特定功能的Netfilter内核模块都可以扔掉了，在用户态写nftables规则即可。这个特性
可以吸引iptables使用者去使用nftables，因为后者可以“随心所有编程”了！我个人也是比较期待nftables在BSD的Netgraph上有所为的。
        但是如此的想法后只能呵呵，其实nftables还远远没有进化到如此地步。不过基本思想在，我们可以期待nftables的进化。
        以前，你是拿着原材料到传统的代工厂生产了一条规则，如今你是用自家的家伙自己“烹饪”了一条规则。
        nftables引入了运算符，变量和数据结构，这意味着nftables规则的设置者从此成了“编程者”，内核仅仅执行nftables管理员编制的程序，不再做任何假定。

        接近尾声，关于性能相关的话题，我再辩护几句。
---------------------------------

iptables曾经饱受诟病，iptables和Netfilter只能为此承担了责任。因为iptables和其依赖的Netfilter内核模块为用户做的太多了！我们来看看这是为什么。
        iptables规则集由N条规则组成，这些规则和Netfilter的5和HOOK点相关联，在每一个HOOK点上，进入的IP数据报文要遍历所有关联与此点的规则，直到某条规则明确返回ACCEPT或者DROP之类。如果有10000条规则，那就要最多去匹配10000次。除此之外，进入内部，我刚才说过，iptables的内核设施明令规定了规则的结构以及执行过程，写规则的“运维人员”根本就无力像“程序员”那般去修改其行为以便优化，一切都是固定的，如果一个iptables内核模块实现的不好，比如数据结构组织的不好，那么iptables规则的编写者只能兴叹。
        正是因为这种无力，除非必须要用，人们一提起Netfilter/iptables就想到它会影响性能，这其实不是Netfilter的问题，这是iptables的问题，nf-HiPAC也是基于Netfilter的，但为什么就不影响性能呢？
        nftables作为iptables的后继者，摆脱了本不该自己承担的责任。
        从此以后随着nftables版本的进化，它的行为和性能只与“nftables编程者”有关，你再也不能抱怨nftables的效率低下了，如果它的性能低下，那只能怪你编程编的不好。nftables作为拥有自己独立语法的“一种编程语言”，和C语言是类似的，如果你的C语言代码性能很低，你会怪C语言本身吗？不光如此，你不能怪C语言，你也不能怪CPU，所以你不能怪nftables，也不能怪Netfilter。

        包分类这个性能攸关的话题一直以来真的是性能攸关，一般情况下，人们不敢用iptables来搞包分类，这也是事实。后来出现了nf-HiPAC，这玩意儿性能非常高，但是其背后却是复杂的内核代码，伴随着nf-HiPAC作者被招安以及其作品的商业化，你能看到的那个性能比iptables高很多的nf-HiPAC版本其实只是个低级版本。使用nftables的话，你甚至可以“烹饪”出一条基于多维树匹配的规则来，在iptables中，你无力放弃线性的逐条matches匹配，但是在nftables中，你却有能力将其由线性匹配优化成一颗多维匹配树。
---------------------------------

如果你能看到这里，那么下面的内容也就不重要了。但是虽不重要有可能还是比较有用的。最后来点Howto。
        和xtables-addons一样，安装nftables需要先安装以下的包：libmnl-1.0.4.tar.bz2，libnftnl-1.0.6.tar.bz2，readline-6.3.tar.gz，gmp-6.1.2.tar.bz2。这些都很容易找到，我就不给链接了。然后就可以编译nftables了。内核方面的升级并没有难度，我在CentOS 6.7(内核版本为2.6.32)上成功编译了4.9版本内核(将关于nftables的编译选项全部打开)并安装，随后成功安装了nftable并顺利运行，难度并不大。

**前提基础：**

当主机收到一个数据包后，数据包先在内核空间中处理，若发现目的地址是自身，则传到用户空间中交给对应的应用程序处理，若发现目的不是自身，则会将包丢弃或进行转发。

iptables实现防火墙功能的原理是：在数据包经过内核的过程中有五处关键地方，分别是PREROUTING、INPUT、OUTPUT、FORWARD、POSTROUTING，称为钩子函数，iptables这款用户空间的软件可以在这5处地方写规则，对经过的数据包进行处理，规则一般的定义为“如果数据包头符合这样的条件，就这样处理数据包”。

iptables中定义有5条链，说白了就是上面说的5个钩子函数，因为每个钩子函数中可以定义多条规则，每当数据包到达一个钩子函数时，iptables就会从钩子函数中第一条规则开始检查，看该数据包是否满足规则所定义的条件。如果满足，系统就会根据该条规则所定义的方法处理该数据包；否则iptables将继续检查下一条规则，如果该数据包不符合钩子函数中任一条规则，iptables就会根据该函数预先定义的默认策略来处理数据包

iptables中定义有表，分别表示提供的功能，有filter表（实现包过滤）、nat表（实现网络地址转换）、mangle表（实现包修改）、raw表（实现数据跟踪），这些表具有一定的优先级：raw-->mangle-->nat-->filter

一条链上可定义不同功能的规则，检查数据包时将根据上面的优先级顺序检查

![wKiom1fD9SHiFulVAAFG31wO9vs466.png](https://www.linuxidc.com/upload/2016_09/160901194975491.png)

（图片来源网络）

小结一下～～～

![wKioL1fD-LzQLXN1AACb9oWWVug429.png](https://www.linuxidc.com/upload/2016_09/160901194975492.png)

数据包先经过PREOUTING，由该链确定数据包的走向：

  1、目的地址是本地，则发送到INPUT，让INPUT决定是否接收下来送到用户空间，流程为①--->②;

  2、若满足PREROUTING的nat表上的转发规则，则发送给FORWARD，然后再经过POSTROUTING发送出去，流程为： ①--->③--->④--->⑥

主机发送数据包时，流程则是⑤--->⑥

**iptables安装配置**

linux一般默认都已经安装iptables，只需要开启服务即可

```
service iptables start
```

**iptables规则书写**

基本语法：iptables [-t 表] [操作命令] [链][规则匹配器][-j 目标动作]

| 表             | 说明                                               | 支持的链                        |
| -------------- | -------------------------------------------------- | ------------------------------- |
| raw            | 一般是为了不再让iptables对数据包进行跟踪，提高性能 | PREROUTING、OUTPUT              |
| mangle         | 对数据包进行修改                                   | 五个链都可以                    |
| nat            | 进行地址转换                                       | PREROUTING、OUTPUT、POSTROUTING |
| filter（默认） | 对包进行过滤                                       | INPUT、FORWARD、OUTPUT          |

| 常用操作命令 | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| -A           | 在指定链尾部添加规则                                         |
| -D           | 删除匹配的规则                                               |
| -R           | 替换匹配的规则                                               |
| -I           | 在指定位置插入规则例：iptables -I INPUT 1 --dport 80 -j ACCEPT（将规则插入到filter表INPUT链中的第一位上） |
| -L/S         | 列出指定链或所有链的规则                                     |
| -F           | 删除指定链或所有链的规则                                     |
| -N           | 创建用户自定义链例：iptables -N allowed                      |
| -X           | 删除指定的用户自定义链                                       |
| -P           | 为指定链设置默认规则策略，对自定义链不起作用例：iptables -P OUTPUT DROP |
| -Z           | 将指定链或所有链的计数器清零                                 |
| -E           | 更改自定义链的名称例：iptables -E allowed disallowed         |
| -n           | ip地址和端口号以数字方式显示例：iptables -Ln                 |

| 常见规则匹配器         | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| -p tcp\|udp\|icmp\|all | 匹配协议，all会匹配所有协议                                  |
| -s addr[/mask]         | 匹配源地址                                                   |
| -d addr[/mask]         | 匹配目标地址                                                 |
| --sport port1[:port2]  | 匹配源端口(可指定连续的端口）                                |
| --dport port1[:port2]  | 匹配目的端口(可指定连续的端口）                              |
| -o interface           | 匹配出口网卡，只适用FORWARD、POSTROUTING、OUTPUT。例：iptables -A FORWARD -o eth0 |
| -i interface           | 匹配入口网卡，只使用PREROUTING、INPUT、FORWARD。             |
| --icmp-type            | 匹配icmp类型（使用iptables -p icmp -h可查看可用的ICMP类型）  |
| --tcp-flags mask comp  | 匹配TCP标记，mask表示检查范围，comp表示匹配mask中的哪些标记。例：iptables -A FORWARD -p tcp --tcp-flags ALL SYN，ACK -j ACCEPT（表示匹配SYN和ACK标记的数据包） |

| 目标动作 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| ACCEPT   | 允许数据包通过                                               |
| DROP     | 丢弃数据包                                                   |
| REJECT   | 丢弃数据包，并且将拒绝信息发送给发送方                       |
| SNAT     | 源地址转换（在nat表上）例：iptables -t nat -A POSTROUTING -d 192.168.0.102 -j SNAT --to 192.168.0.1 |
| DNAT     | 目标地址转换（在nat表上）例：iptables -t nat -A PREROUTING -d 202.202.202.2 -j DNAT --to-destination 192.168.0.102 |
| REDIRECT | 目标端口转换（在nat表上）例：iptables -t nat -D PREROUTING -p tcp --dport 8080 -i eth2.2 -j REDIRECT --to 80 |
| MARK     | 将数据包打上标记例：iptables -t mangle -A PREROUTING -s 192.168.1.3 -j MARK --set-mark 60 |

注意要点：

  1、目标地址转换一般在PREROUTING链上操作

  2、源地址转换一般在POSTROUTING链上操作

 

**保存和恢复iptables规则**

  使用iptables-save可以保存到特定文件中

```
  ``iptables-save >``/etc/sysconfig/iptables_save
```

  使用iptables-restore可以恢复规则

```
  ``iptables-restore<``/etc/sysconfig/iptables_save
```

**iptables的进阶使用**

  1、limit限制流量：

​    -m limit --limit-burst 15    #设置一开始匹配的最大数据包数���

​    -m limit --limit 1000/s      #设置最大平均匹配速率

​    -m limit --limit 5/m --limit-burst 15   #表示一开始能匹配的数据包数量为15个，每匹配到一个，  

​                                  limit-burst的值减1,所以匹配到15个时，该值为0,以后每过  

​                                  12s，limit-burst的值会加1,表示又能匹配1个数据包

例子：

```
iptables -A INPUT -i eth0 -m limit --limit 5``/m` `--limit-burst 15 -j ACCEPT ``iptables -A INPUT -i eth0 -j DROP
```

  注意要点：

​    1、--limit-burst的值要比--limit的大

​    2、limit本身没有丢弃数据包的功能，因此，需要第二条规则一起才能实现限速的功能

  2、time ：在特定时间内匹配

| -m time                 | 说明                                    |
| ----------------------- | --------------------------------------- |
| --monthdays day1[,day2] | 在每个月的特定天匹配                    |
| --timestart hh:mm:ss    | 在每天的指定时间开始匹配                |
| --timestop hh:mm:ss     | 在每天的指定时间停止匹配                |
| --weekdays day1[,day2]  | 在每个星期的指定工作日匹配，值可以是1-7 |

例子：

```
iptables -A INPUT -i eth0 -m ``time` `--weekdays 1,2,3,4 -jACCEPT``iptables -A INPUT -i eth0 -j DROP
```

  3、ttl：匹配符合规则的ttl值的数据包

| 参数         | 说明                     |
| ------------ | ------------------------ |
| --ttl-eq 100 | 匹配TTL值为100的数据包   |
| --ttl-gt 100 | 匹配TTL值大于100的数据包 |
| --ttl-lt 100 | 匹配TTL值小于100的数据包 |

例子：

```
iptables -A OUTPUT -m ttl --ttl-``eq` `100 -j ACCEPT
```

  4、multiport：匹配离散的多个端口

| 参数                         | 说明                 |
| ---------------------------- | -------------------- |
| --sports port1[,port2,port3] | 匹配源端口           |
| --dports port1[,port2,port3] | 匹配目的端口         |
| --ports port1[,port2,port3]  | 匹配源端口或目的端口 |

例子：

```
iptables -A INPUT -m multiport --sports 22，80，8080 -j DROP
```

  

  5、state：匹配指定的状态数据包

| 参数          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| --state value | value可以为NEW、RELATED（有关联的）、ESTABLISHED、INVALID（未知连接） |

例子：

```
iptables -A INPUT -m state --state NEW，ESTABLISHED -j ACCEPT
```

  6、mark：匹配带有指定mark值的数据包

| 参数         | 说明                        |
| ------------ | --------------------------- |
| --mark value | 匹配mark标记为value的数据包 |

例子：

```
iptables -t mangle -A INPUT -m mark --mark 1 -j DROP
```

  7、mac：匹配特定的mac地址

例子：

```
iptables -A FORWARD -m mac --mac-``source` `00:0C:24:FA:19:80 -j DROP
```

更多iptables相关教程见以下内容：

CentOS 7.0关闭默认防火墙启用iptables防火墙  http://www.linuxidc.com/Linux/2015-05/117473.htm

iptables使用范例详解 http://www.linuxidc.com/Linux/2014-03/99159.htm

Linux防火墙iptables详细教程 http://www.linuxidc.com/Linux/2013-07/87045.htm

iptables的备份、恢复及防火墙脚本的基本使用 http://www.linuxidc.com/Linux/2013-08/88535.htm

Linux下防火墙iptables用法规则详解 http://www.linuxidc.com/Linux/2012-08/67952.htm

Linux下iptables防火墙设置 http://www.linuxidc.com/Linux/2015-10/123843.htm

本文永久更新链接地址：http://www.linuxidc.com/Linux/2016-09/134832.htm

