---
title: 如何使用虚拟机VMWare Workstation安装deepin
description: VMWare Workstation安装deepin
published: true
date: 2022-06-15T02:52:52.089Z
tags: deepin, vmware, 虚拟机
editor: markdown
dateCreated: 2022-06-15T02:18:22.869Z
---

# VMWare Workstation上安装deepin

## 简介
本篇文档来自论坛用户（木一明）的分享-[《使用VMware workstation 安装和管理deepin》](https://bbs.deepin.org/zh/post/232566)。
若为windows系统，下载.exe版本VMWare Workstation，双击运行安装包即可安装；若为linux系统，下载.bunble版本VMWare Workststion，使用命令`sudo ./VMWare安装包`即可安装。
VMware workstation 提供主机运行的硬件基础，deepin提供主机运行的软件基础。
## 新建虚拟机
> *本节主要内容：使用VMware workstation创建一台配置自定义的虚拟机。简单来说，就是定制一台PC的硬件。*

首先打开VMware workstation，点击【创建新的虚拟机】，如下图：
![alt 创建新的虚拟机](https://storage.deepin.org/thread/202203060843451445_image.png)
在出现的对话框中，选择【自定义(高级)】，如下图：
![alt 自定义(高级)](https://storage.deepin.org/thread/202203060844583237_image.png)
在出现的对话框中，是VMware根据物理机PC 给的默认配置选项，用来保持虚拟机的兼容性，直接默认下一步即可，如下图：
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060846149106_image.png)
在出现的对话框中，是VMware询问是否安装操作系统，从哪里安装操作系统，我们暂时不安装操作系统，选择如下图：
![alt 新建虚拟机向导](https://storage.deepin.org/thread/20220306084714495_image.png)
在出现的对话框中，是VMware 询问要安装的虚拟机的操作系统的类型。deepin当前版本是基于debian的发型版本，所以我们选择如下图：
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060848225466_image.png)
在出现的对话框中，是VMware给创建的虚拟机起个名字，放在哪里。deepinV20系列我们叫做deepin20，位置默认即可。以后有deepinV23的话，可以叫做deepin23。
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060849411528_image.png)
接下来就是定制硬件的过程了。首先是处理器。我们知道现在电脑配置都很高，一般都是几个处理器，每个处理器几个核心，我们根据自己的物理机配置做选择，选错没关系，VMware会提示的。我根据自己的配置，为虚拟机创建如下配置
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060851416258_image.png)
接下来是内存的选择。我物理机16G内存，为了得到更好的体验，这里分配虚拟机8G，如下图：
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060852468047_image.png)
接下来的【网络类型】，无特殊配置需求 ，直接默认网络地址转换即可，这种配置的意思就是，把物理机作为路由器，虚拟机相当于连接在路由器上的一个内网机器。如下图：
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060853339947_image.png)
然后是【I/O控制器类型】，直接默认VMware给的推荐即可
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060854428287_image.png)
【磁盘类型】也是，直接默认
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060855092888_image.png)
接下来VMware询问如何使用磁盘？是使用物理盘，还是使用虚拟盘。我们选择默认，即创建一个虚拟硬盘
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060856032790_image.png)
接下来是配置虚拟硬盘的大小和存放模式，我们选择分配60G大小，不立即分配这么多空间，将虚拟盘作为一个独立文件存在。

这样做的意义：
- 60G大小足够安装和使用deepin。过小的话可能出现其他问题
- 不立即分配所有磁盘空间，主要是虚拟磁盘会占用物理机的磁盘空间。我们珍惜一下物理机的磁盘空间
- 存储为单个文件，便于以后成为高级玩家以后，可以把虚拟机到处拷贝，导入到其他物理机
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060856403015_image.png)

然后选择磁盘的格式，默认即可
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060858555541_image.png)
接下来是配置汇总清单
![alt 新建虚拟机向导](	https://storage.deepin.org/thread/20220306085920408_image.png)
到这里，我们就组装好了一台电脑，点击选择【完成】即可。
以下内容是我个人习惯配置，仅供参考，与本节内容无关：
点击【自定义硬件】，我们还可以对配置好的硬件，进行各种自定义和修改
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060901037387_image.png)
假如你想在deepin里面体验虚拟化，比如配置kvm环境，那么需要点击【处理器】勾选【虚拟化引擎】功能，如下图：
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060902146831_image.png)
如果你想给虚拟机固定MAC地址（网卡ID），在【网络适配器】选择高级，然后配置MAC地址，如下图：

> 本篇把deepin20的虚拟机地址配置为：00:00:00:80:00:20。以后deepinV23的话，可以设置为：00:00:00:80:00:23，便于区分对比

![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060903295429_image.png)
最后是【显示器】选择，如下，禁用3D加速即可
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060905065356_image.png)
最后，如果还希望deepin里面能够模拟更多的使用场景，比如：
- 多网卡
- 双系统
- 等

还可以选择【添加】，来配置更多的硬件环境。如下图：
![alt 新建虚拟机向导](https://storage.deepin.org/thread/20220306084714495_image.png)
## 安装操作系统deepin
> 在第二节【创建虚拟机】里面，我们已经通过VMware workstation 创建一个名为:deepin20的裸机(没有操作系统的电脑)，本节我们就开始来为此虚拟机安装deepin操作系统

首先要做的就是，为虚拟机deepin20的CD插入一个光盘，操作如下图：
![alt 新建虚拟机向导](https://storage.deepin.org/thread/20220306090916631_image.png)
点击确定之后，如下图。意思即是说，我们准备使用这个iso给deepin20安装操作系统了。那么开始吧！
![alt 新建虚拟机向导](	https://storage.deepin.org/thread/202203060909501485_image.png)
点击【开启虚拟机】，不一会儿，就出现了我们熟悉的界面，如下图。
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060911355026_image.png)
上图等待一会儿，就出现了配置界面。首先进行语言配置，勾选以后，点击下一步即可
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060912386413_image.png)
下一步之后，出现了友情提示，说我们正在使用虚拟机体验deepin，这会导致性能下降，体验不好。

不过没关系，我们就使用虚拟机体验是有道理的，谢谢提示哦！
![alt 新建虚拟机向导](	https://storage.deepin.org/thread/202203060913063090_image.png)
然后下一步，如下图：
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060914485194_image.png)
这一步即使关键的一步了，出现了比较高级的选择了【硬盘分区】。这也是困扰很多新手用户的关键问题之一。该如何选择硬盘分区呢？还有要不要对硬盘进行加密呢？保留用户数据又是什么东东？

没关系，不要忘了我们正在使用VMware，而且我们玩的是虚拟机。那么这些问题重要吗？不重要，为啥？大不了重新来呗。虚拟机不就是用来体验摸索的嘛，摸索熟了，自然就知道了对吧。

**那么首先来一个重磅功能：快照！**

快照很好理解。照相知道吧，照相能保留快门按下的那一瞬间，你的动作，表情，神态。VMware 快照就是保留这一瞬间虚拟机的状态。虽然照片没有办法让你回到过去某个时间点重新来过，但是虚拟机管理的快照却可以让虚拟机回到那个状态。回去干什么？重新来过啊，你即便是把虚拟机玩坏了，没事，一个菠萝菠萝蜜咒语直接回去，重新搞！

使用VMware workstation的快照功能，我们可以兵分两路，分别体验！不信？来看！

如下图，我们做 如下操作，创建一个快照。
![alt 新建虚拟机向导](https://storage.deepin.org/thread/20220306092006563_image.png)
上述操作以后，出现对话框，要给快照命名，然后简单描述一下。明白了，就是怕我忘记了这是哪个时间点嘛。如下图，看！够清楚了吧
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060924472433_image.png)
然后拍摄快照就可以了。通过快照管理器，我们可以看到保存的快照，如下图
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060925424147_image.png)
![alt 新建虚拟机向导](	https://storage.deepin.org/thread/202203060925584078_image.png)
好了，保留快照，主要是想日后再回到这个地方，重新开始，体验不同的分区方式。

当然重新创建虚拟机来设置不可以吗？当然可以，不过创建虚拟机相对于恢复快照来说，那就是公交车跟高铁的区别了。要高效！要优雅！

保留了分区选择的状态的快照，那我就开始分区吧。我们这里选择手动分区。为啥手动？因为根据经验，自动分区不是特别理想，很多人最后都把用户主分区玩满了，但是又没有后悔药可吃。

其实要相信科学！分区在目前看来，只是一个无用的操作。为啥？当一个磁盘出现分区故障时，你还会用吗？当一个硬盘故障时，你分区还有用吗？不能总是用那高大上的概念，吓唬自己的小心肝。大胆玩吧，玩坏了再买一个呗。只要平时注意备份自己的私房照就可以了。

选择手动分区后，如下图。
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060930514324_image.png)
那到底该如何手动分？其实deepin早就为你做好了一切，你只需选择就可以了。如下：
当我们把鼠标放到磁盘是，主要观察磁盘右下角

![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060932066159_image.png)
没错，有个+号，先点点看，出现一个对话框
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060932461353_image.png)
这是干啥的？这就是要如何分区。其实看不懂，尤其是没有Linux磁盘管理和文件系统基础的人。

所以我感觉deepin在这方面设计的不够好，或者足够好。手动分区的人基本都是中级玩家了。因为初级玩家，可能直接选择【全盘安装】了。不然不会把【全盘安装】作为默认

那么既然是中级玩家。这里就不详述了吧。简述是可以的：

这里主要选择文件系统格式和挂载点。文件系统默认ext4，没问题。挂载点吧，选择/boot分区，然后点击新建
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060936211957_image.png)
然后出现了如下界面：
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060936533721_image.png)
继续按照上面的方式操作，这次选择/根分区，如下图：
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060937387189_image.png)
到这里，直接下一步即可。

> 注意：出现任何友情提示，都直接点击确定。老司机告诉你，不会有问题。

然后就是这个界面了
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060938472199_image.png)
等什么，继续安装呗。点击之后 就是一杯咖啡不要的时间，就安装好了。如下图
![alt 新建虚拟机向导](	https://storage.deepin.org/thread/202203060946284538_image.png)
然后点击【立即重启】，接着就会重启进入初始化配置阶段
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060947029703_image.png)
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060947109355_image.png)
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060947212451_image.png)

上面都没有啥，下图有点意思
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060947538510_image.png)

主要是配置用户的【图像】，用户名，计算机名，密码等，都是基本的配置，点击之后进入【优化系统配置】
再等等，就是登录界面了
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060948552605_image.png)
此时输入你刚才设置的用户密码，就可以进入桌面玩耍了
![alt 新建虚拟机向导](https://storage.deepin.org/thread/20220306094924156_image.png)
进入之前，要选择模式？当然是特效模式啦，没有特效还玩啥
![alt 新建虚拟机向导](	https://storage.deepin.org/thread/202203060950068266_image.png)
终于啊，见到庐山真面目了！

###### 可别急，先撸个快照吧，以便日后折腾到面目全非，我可以再穿越回来！
![alt 新建虚拟机向导](https://storage.deepin.org/thread/202203060951159828_image.png)
好了，尽情折腾吧！

折腾坏了，也不是大不了重装，而是大不了恢复快照！