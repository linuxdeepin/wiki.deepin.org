---
title: 根目录扩容
description: 系统在随着日渐使用的过程中，根目录空间会越来越小，可能部分同学需要进行扩容操作，可以参考此思路完成。
published: true
date: 2022-06-01T02:04:37.282Z
tags: 扩容 根目录扩容  空间不足 手动扩容
editor: markdown
dateCreated: 2022-06-01T02:00:55.220Z
---

# 根目录扩容
# 友情提醒

### **数据无价，以下操作请大家提前备份好自己的个人数据，以防操作失误造成不可逆损失，操作前确认自己知晓可能存在的系统损坏或者数据丢失风险！**

# 前言

> 阅读建议：查看图片如果不清晰，可以点击放大后查看，部分动图有操作步骤，可以核对后操作，确保理解操作步骤后执行。

基于一些历史原因，deepin在全盘安装的时候，划分给根分区的空间只有区区15G，随着用户日渐使用过程中，这15G空间总会是捉襟见肘的，况且当下硬盘动不动就是几个T，其他都给那“没用”的data了，但是已经使用这么久了，总不该要我重新安装选择手动分区吧，而且用户也需要AB分区的备份功能怎么办？

**这个时候就需要进行扩容操作了，过程其实非常简单，当前方案操作流程主要涉及如下内容：**

![Untitled Diagram.png](https://storage.deepin.org/thread/202205192200407801_UntitledDiagram.png)

# Step One:进入live系统

![1.gif](https://storage.deepin.org/thread/202205201618318171_1.gif)
![](https://vjxs22vadq.feishu.cn/space/api/box/stream/download/asynccode/?code=YmU4ZjgxNzAxZWNmNjY2MTg2ZDI5YTU0NzVkNDkyNzFfdzJmVDczbGRtOUlmWGlQSmg5MHRCSmQ0TnB1Y1lZMWNfVG9rZW46Ym94Y25qa3Nkak92YjJ3c0ZjMUN1Q3VxbDhnXzE2NTMwMTQ4MzQ6MTY1MzAxODQzNF9WNA)**很多人可能还不知道如何进入deepin的live系统，可以看上面动图的操作，也是非常简单：**

#### 方案一

1. 首先准备好一个装有deepin镜像的启动U盘（推荐使用ventoy）；
2. 直接走装镜像的路子,启动到系统安装界面；
3. 唯一区别是在grub安装界面的时候，不要选择任何选项，而是按一下键盘上的 **“E”** 按键（如果是非EFI启动，可能需要按TAB键）；
4. 按过之后就会出现下图的编辑界面，通过上下左右按键移动到下方红框标识处，删除 **“cd-installer”** 内容；

   ![2.png](https://storage.deepin.org/thread/20220520162047272_2.png)
5. 然后直接按键盘上F10按键，接下来就会直接进入live系统界面了。
6. 进入live系统后是如下界面的样子（下图是V20.6的镜像）：
> 特别提醒：在live系统下长时间也会自动锁屏了，如果你也遇到了锁屏发现没有密码无法进入系统，可能你需要重新来一次，此时可以直接通过ctrl+alt+F2 进入TTY，然后在TTY界面设置密码即可：``sudo passwd uos``,然后再切回来用设置的密码登录即可。
   >

![](https://vjxs22vadq.feishu.cn/space/api/box/stream/download/asynccode/?code=YzA2MjdlZDQ0ZWNjZDM2MjY2NWMwMThmYjk3OTI5OTRfeEg3SXo2OFE3cTR4WlhTRzhkOTh3SmxxY0l3V0doUWJfVG9rZW46Ym94Y25TaHNCQ0kzalI5RjRYOWlFYTF2a1diXzE2NTMwMTYzMzY6MTY1MzAxOTkzNl9WNA)![3.png](https://storage.deepin.org/thread/202205201621386209_3.png)

#### 方案二

可以使用论坛中大佬提供的live系统，电梯直达：[https://bbs.deepin.org/post/236521](https://bbs.deepin.org/post/236521)

# Step Two:连接网络

我们进入live系统后，第一时间就是记得去连接好网络，准备下一步的工具安装，连接好后确认一下你的网络是否正常哦。

# Step Three:新增系统仓库

![](https://vjxs22vadq.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2IyOThjZTYzNjYzMDk1ODM1YmY4ODU1ZmVmNWEyZjVfVTczclV2SzhoOEtjUGo5MjhuWjNCQWlNa2tISkhnQVFfVG9rZW46Ym94Y25pZ1Q1b1JsVGk0eThxTWNmdThmcTVlXzE2NTI5Njc5MDE6MTY1Mjk3MTUwMV9WNA)因为live系统本身是未带系统仓库的，我们需要手动添加系统仓库信息，具体操作可以看下面动图：

```
# 编辑源文件
sudo vim /etc/apt/sources.list
# 然后敲入下面的源地址
deb https://community-packages.deepin.com/deepin/ apricot main contrib non-free

```

![](https://vjxs22vadq.feishu.cn/space/api/box/stream/download/asynccode/?code=M2Y1ZTRlMjg3NDI5MGEzOTUxOTVhZTQwZDk2NTM5N2JfV1REbkxaNjNLSEdVSko1WXA4UmRwNFBrdm9aV0ZTZUtfVG9rZW46Ym94Y25pZ1Q1b1JsVGk0eThxTWNmdThmcTVlXzE2NTMwMTgwMjQ6MTY1MzAyMTYyNF9WNA)![4.gif](https://storage.deepin.org/thread/202205201623177976_4.gif)

# Step Four:安装gparted应用

1. 先通过 ``sudo apt update``更新源信息；
2. 然后通过 ``sudo apt install gparted`` 命令来安装gparted工具。

![](https://vjxs22vadq.feishu.cn/space/api/box/stream/download/asynccode/?code=MmY5NDY3OTVmOTFjYTFmMTZmNWE5MzM2OGZlOWQ5NGFfYXNvdEhwd3l6VnN3dFljV0pLTmZ6UGNZZ1ZSb0F0S05fVG9rZW46Ym94Y25uNGJGWmlzMVZJalczSmdTeGZXeGhiXzE2NTMwMTgyNDQ6MTY1MzAyMTg0NF9WNA)![5.gif](https://storage.deepin.org/thread/202205201623434987_5.gif)
![](https://vjxs22vadq.feishu.cn/space/api/box/stream/download/asynccode/?code=MDMyMTQ3YmY5NjJlMWU0MmM1ODUyNjhjOGU2NjQwMjRfcWh4YlNHRlRqNHBqNDYyT2h1UU9mbU1rczBlYkNydjhfVG9rZW46Ym94Y25uNGJGWmlzMVZJalczSmdTeGZXeGhiXzE2NTI5Njc5MDE6MTY1Mjk3MTUwMV9WNA)

# Step Five 使用gparted进行扩容操作

## 观察当前分区

- 应用打开后，可以观察到如下硬盘分区的情况，其中Roota 和 Rootb 则是我们说的AB镜像分区，他们的大小是一样大，也就是我们根目录的大小；
- 可以看到下图中我的根目录已经被我调整成了30G了（原来只有15G）；
- 那么下面我们在这30G的基础上，再将其调整成40G大小。

![image.png](https://storage.deepin.org/thread/202205201148141907_image.png)

## 压缩data分区

- 我们首先需要压缩data分区的空间，总共压缩20G出来，后面给Roota 和 Rootb一人分配个10G；
- 先选中data分区，然后右键选择【更改大小/移动】选项；
- 然后在“之前的空余空间”设置项中，填写20G大小换算成MB，就是  **20480** ；
- 再然后在“之后的空余空间”输入框中点击一下，确保为0；
- 最后点击一下【调整大小】按钮就完事了。

![](https://vjxs22vadq.feishu.cn/space/api/box/stream/download/asynccode/?code=N2UxZjZhMDE0NGEwY2UxNGI5ZjM3MzIzNGIyMjJiMWJfSmZSM25iUjhCdUNtMEJMWTRiWnEyNXlWcEQydUJicUpfVG9rZW46Ym94Y25ZdHJuejliSThwRHhmTUswcnRacGFoXzE2NTMwMTg2Mjc6MTY1MzAyMjIyN19WNA)![6.gif](https://storage.deepin.org/thread/202205201624567146_6.gif)

压缩后的效果如下图所示，多出一个20G可用的空间了：

![7.png](https://storage.deepin.org/thread/202205201625447933_7.png)

完成上面那步骤后，估计你也知道下面该怎么玩了，跟拼积木一样，先将这20G空间与上面紧挨着的Rootb空间合并；

- 然后再将Rootb的空间压缩个10G出来；
- 最后将这10G空间与Roota合并；
- 最终应用就大功能告成了！

## Rootb分区合并空闲空间

按照下图操作完成合并

![8.gif](https://storage.deepin.org/thread/202205201626106018_8.gif)

## Rootb分区压缩空间

与上面同样的操作，将此时扩容后的Rootb空间再压缩10G给Roota。

![9.gif](https://storage.deepin.org/thread/20220520162711983_9.gif)

压缩完后就像下图一样，这个空间接下来就可以给Roota了：

![10.png](https://storage.deepin.org/thread/202205201627504966_10.png)

## Roota合并空闲空间

老办法，直接按照下图将空余空间直接给Roota拉满即可：

![12.gif](https://storage.deepin.org/thread/202205201629096274_12.gif)

## 核实最终空间分配情况

- 有时候可能在操作的时候没注意，会跑出来个1MB的未分配空间，不用理会也是可以的；
- 不过你要是有强迫症，那按照上面方法，把这些未分配的空间随便合并到哪个空间就行；
- 要学会举一反三不能钻牛角尖哈。

![13.png](https://storage.deepin.org/thread/202205201629343521_13.png)

## 应用最终的数据变更

- 在做这一步之间，还有后悔药，一旦执行了这一步，有可能会存在意外，导致数据不可恢复；
- **请在执行此操作之前，确认你是想好了，出了问题这锅自己背呀。**
- 这一步的操作，根据自己机器性能和数据量大小不同，耐心等待即可，完成后就会有成功的提示。

![15.gif](https://storage.deepin.org/thread/202205201629545073_15.gif)

## 最终核实分区情况：

完成后，即可看到上方的应用按钮是置灰的，此时分区大小也是与你预期是一致的。

![16.png](https://storage.deepin.org/thread/202205201630101848_16.png)

## 验证你的成果

直接重启你的系统，检查下你的系统盘容量吧！

![](https://vjxs22vadq.feishu.cn/space/api/box/stream/download/asynccode/?code=Mjk4NGQ3ZjAxOTYzOTQ1MDY5NDcwYmE1NGI2MWRmMDVfQ2M5S3pJV2kzMmF6aElxcHh1Ym5rOUNtUkZpTTl5YXhfVG9rZW46Ym94Y25jbUM4dWdaUUJIMlBibjdPTjJQZkFlXzE2NTI5Njc5MDE6MTY1Mjk3MTUwMV9WNA)![17.png](https://storage.deepin.org/thread/202205201630304146_17.png)

# 后记

- 基于这个根分区大小的诉求，我们也在新版本的安装器中，增加了在全盘安装时可以自定义调整根分区大小的功能，在V20.6新版本镜像中就可以体验了。
- 此方案不仅仅只是适用于全盘分区的情况，适用于任何分区安装方式的后期调整，只要思路原理清晰，即可顺利完成。