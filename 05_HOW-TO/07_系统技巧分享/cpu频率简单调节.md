---
title: cpu频率简单调节
description: cpu频率简单调节
published: true
date: 2024-07-09T03:11:46.173Z
tags: 
editor: markdown
dateCreated: 2024-07-09T03:11:46.173Z
---

> 本文简单分享下如何通过调节cpu频率使电脑在自己满意状态下运行。

## 〇、免责声明

下面内容不同设备个性化设置会比较强，需要小伙伴们仔细阅读并测试，认真比对才能让硬件在最佳状态运行，需要有一定动手能力和知识储备。

## 一、基本原理

开过油车的朋友都知道，有个最佳的油门范围，油门踩的轻了或者踩的重了，走相同的路段用的油都会增多，只有在最合适的范围内油耗才会比较低。

同理，cpu也是如此的，**也有个最佳的功耗比范围**，小马拉大车或者大马拉小车都会比较费电，消耗的电能实际上最终都转换成了**热能，**外在的表现就是**发热、风扇狂转。**

这里有几个小细节，也是跟人理解不太一样：

1. 风扇转速是跟**cpu频率相关**的，**不是跟温度相关**的，低频温度高他可能不怎么转，高频温度低他可能死命转。
2. 有一个核心负载比较高，风扇就狂转，并不是cpu总利用率高了风扇才狂转，因为怕这个核心过热。
3. deepin的负载均衡这块可能考虑到实现负载均衡也会消耗资源，很多时候一核有难n核围观，也就我们看到的明明cpu总占用并不高，风扇也嗷嗷转。

所以很多人想**调节风扇转速**，其实最简单的方法就是**调节频率**。

注意：

- 并不是调低频率，发热一定会降低！有可能又卡又烫(参考某国民品牌手机的调度)！
- 并不是调低频率，发热一定会降低！有可能又卡又烫(参考某国民品牌手机的调度)！
- 并不是调低频率，发热一定会降低！有可能又卡又烫(参考某国民品牌手机的调度)！

所以我们调教cpu的核心思想是，**让他的频率在最佳的能耗范围**。

下面说的方法都需要**反复测试，最终找一个合适的数值，每台电脑硬件不同、使用习惯不同，最终数据也不一样的。**

## 二、用到的工具

### 2.1 命令行工具：cpupower

![image.png](https://storage.deepin.org/thread/202407060906313153_image.png)

1. deepin的源里有这个，直接 `sudo apt install linux-cpupower`
2. 其他发行版的小伙伴如果没有，就去deepin软件源里获取

### 2.2 gui工具：cpupower-gui

![image.png](https://storage.deepin.org/thread/202407060911148081_image.png)

1. ok源里有这个工具，直接 `sudo apt install cpupower-gui`即可
2. deepin小伙伴可以去github或者其他发行版仓库查找

上面两个工具作用是差不多，用一个就行了。

## 三、调教方法

### 3.1 判断调度器使用技术种类

目前市面上还在用的电脑有大的两种技术：

1. cpufreq
2. pstate

这两种技术各自又分intel家的和amd家的，具体有挺大区别，但在频率调节这块区别不是很大，今天就不刻意区分，统一叙述。

比较老的，比如6代以前的intel的cpu可能用的是cpufreq，之后的估计是pstate，查询方法很简单：

如果使用的是命令行工具，运行： `cpupower frequency-info`

![image.png](https://storage.deepin.org/thread/202407060916178574_image.png)

![image.png](https://storage.deepin.org/thread/202407060917543397_image.png)

如果使用的是gui工具，那就直接看调度器种类，像这中只有两个调度器的基本上就是**pstate**：

![image.png](https://storage.deepin.org/thread/202407060920241657_image.png)

不同的技术，我们调教的思路也不一样，下面分别叙述。

### 3.2 cpufreq

在cpufreq下调度器非常多，我认为主要分三类：

1. 省电模式：`powersave`这个调度器会尽可能低在最低频率运行，在某些机器上甚至会表现成**锁死在最低频率**的情况，这就会非常卡。
2. 平衡模式：我嫌麻烦，不想说细节，你们也不愿意看，把 `conservative ondemand userspace powersave schedutil`这几个都归到平衡，大概都是按需调节频率，只不过每种对于这个按需理解不同，侧重点也不同。
3. 性能模式：`performance`这个调度器会尽可能在最高频率运行，在某些机器上甚至会表现成**锁死在最高频率**的情况，理论上讲发热量非常大。

针对这三种模式，就有三种调节思路：

#### 3.2.1 全局省电模式(powersave)

省电模式一般会很凉快的，但他会比较卡，因为是在尽可能低频率运行，甚至锁死，所以我们需要调节最低频率，不让他过低。

如果使用的是命令行工具，运行：

1. 使用powersave调度：`sudo cpupower frequency-set -g powersave`
2. 设置最低频率： `sudo cpupower frequency-set -d 800MHz`

这样即可启动省电调度，并且设置最低频率为800MHz，不用管最高，因为此调度会尽可能在最低频率运行，甚至会锁死最低频率，**请大家反复测试，找一个温度合适、风扇不那么吵、还不卡的频率。**

![image.png](https://storage.deepin.org/thread/202407060935478081_image.png)

如果使用的是gui工具，那很简单，直接拖动进度条，然后应用即可(不要忘了选all cpu)：

![image.png](https://storage.deepin.org/thread/202407060938197887_image.png)

#### 3.2.2 全局平衡模式

平衡模式下cpu频率会按需跳动，一会高一会低，如果频率和需求匹配，理论上讲是最好的，该快的时候快，该慢的时候慢，但实际上经常碰到的是错配，还没来得及频率升上来结果任务完成了，或者频率刚降下去负载又起来了。

这里的调度实际上也分三类：

1. 平衡中的保守：有了负载，cpu频率慢慢升上去，负载降下来再慢慢掉下来，有可能造成卡顿，负载上来半天频率才上来。
2. 平衡中的平衡：比保守调节快点，比激进调节慢点。
3. 平衡中的激进：有了负载，cpu频率快速升上去，负载降下来再快速掉下来，有可能造成发热，算个 很简单的人物，cpu一下升上去，并且过多资源消耗在cpu自身频率调节上。

个人比较推荐 `schedutil`这个调度，相对比较平衡。
因为这个调度是平衡，cpu的频率会上下跳，所以你不但得设置下限防止卡顿，还得设置上限防止发热：

1. 使用 `schedutil`调度：`sudo cpupower frequency-set -g schedutil`
2. 设置最低频率： `sudo cpupower frequency-set -d 800MHz`
3. 设置最高频率： `sudo cpupower frequency-set -u 2000MHz`

这样你就可以让cpu在800～2000之间运行了，具体的数值需要同学们自己摸索。

如果使用的是gui工具，同时设置调度器和上下限即可：

![image.png](https://storage.deepin.org/thread/202407060953049416_image.png)

#### 3.2.3 全局性能模式

这个模式下cpu会尽可能在最高频率运行，甚至会锁频在频率上限，这就造成了大量发热，但性能不会有问题，非常流畅，这里我们的调节思路是 **适当降低最大频率，减少发热。**

注意，降低最大频率不一定会降低性能，发热了本身就会碰到功耗墙降频，甚至适当降低最大频率跑分还会增加，**三秒真男人**原理大家都懂得吧。

所以大家不要担心会卡顿，据我观察，**降频到90%或者95%，你根本感知不到性能上有啥变化，但能大幅度降低温度和风扇转速**，有时候甚至跑分都在跑分误差内。

这调度下，大家不用管下限，只调整上限即可：

1. 使用 `performance`调度：`sudo cpupower frequency-set -g performance`
2. 设置最高频率： `sudo cpupower frequency-set -u 4000MHz`

如果是gui工具，也一样：

![image.png](https://storage.deepin.org/thread/202407061000197695_image.png)

#### 3.2.4 小结

cpufreq技术下，省电模式和性能模式其实类似于定频运行，只有平衡模式频率才会按需运行，所以设置思路就有：

1. 省电模式设置下限放置卡顿，定频到最低频率
2. 平衡模式设置上下限防止卡顿和过热，按需调节频率
3. 性能模式设置上限放置过热，定频到最高频率

### 3.3 pstate

以前的cpufreq技术效果不佳，定频的能效比不好，按需容易产生错配，所以才有了现在的pstate技术。
他有下面几个特点(不全)：

1. 减少了调度器的种类，让我们在选择调度器的时候不再懵逼。
2. 不管是powersave还是performance，cpu均是按需调节频率。
3. 大幅度提高cpu频率调节速度，响应更快，减少了错配情况的发生。
4. ……

目前主流发行版均已很好支持，如果你cpu用的是这种技术，请继续往下看。

pstate技术下的**节能模式**cpu升频没那么激进，更加注重能效比，有些发行版和操作系统就直接翻译成：**效能模式**

pstate技术下的**性能模式**cpu升频会很激进，更加注重性能，有点负载cpu就拉满，之前deepin的平衡模式就用过性能模式，所以上来感觉发热就比较凶，各种发行版一般都翻译成：**性能模式**

由于在pstate技术下，cpu频率都是按需变化的，实测 `powersave`和 `powermance`在跑分上区别很小，甚至有些散热不好的机器反倒是 `powersave`跑分较高。

deepin下调度器和电源模式对应关系如下：

![image.png](https://storage.deepin.org/thread/202407061019344846_image.png)



这种技术下，由于响应够快，频率拉升速度很快，错配大幅减少，所以我们主要防止过热即可，一般情况下使用 `powersave`调度和调节最高频率即可：

1. 使用 `powersave `调度：`sudo cpupower frequency-set -g powersave`

* 设置最高频率： `sudo cpupower frequency-set -u 4000MHz`

gui工具也是如此：

![image.png](https://storage.deepin.org/thread/202407061021575893_image.png)

## 四、总结

以上设置**重启后均会失效**，需要重新设置，大家日常使用可以**如果不烫就不用管，发现风扇太吵或者电脑太烫就配置一下，手动降温。**

核心思路还是调节cpu的调度和频率，让cpu在最合适的范围内工作，从而避免发热和卡顿情况发生。

具体数值需要同学根据自己电脑情况自行调节和配置，据我个人经验**cpu最大频率设置到最大频率的90%或95%**，不卡风扇也不吵。

不用过多配置，过度优化有时候是负优化。