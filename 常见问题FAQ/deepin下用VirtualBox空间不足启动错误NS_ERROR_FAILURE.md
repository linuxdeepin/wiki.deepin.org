---
title: deepin下用VirtualBox空间不足启动错误NS_ERROR_FAILURE
description: 
published: true
date: 2022-06-27T03:04:02.583Z
tags: virtualbox
editor: markdown
dateCreated: 2022-06-27T03:00:57.617Z
---

# ***deepin下用VirtualBox空间不足启动错误NS_ERROR_FAILURE***

打开VirtualBox报以下错误

忘记截图了，具体错误信息是 NS_ERROR_FAILURE (0x80004005)

发生原因：
（仅个人而言）某些原因项目的win文件占了满盘（我的本机虚拟机文件夹放在了home,home文件分区应该分小了。留下的后遗症，安了一个centos-7,还备份了一下，想安个win10），虚拟机自动暂停就直提示空间不足，于是强制退出了，强制退出2-3次之后，就出现了0x80004005错误。

解决方法：
找到你的virtualbox，或者桌面上的VirtualBox快捷方式，右键–>属性–>权限修改如图

![1](https://storage.deepin.org/thread/202203221149488047_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220322071309.png)

![2](https://storage.deepin.org/thread/202203221207132888_%E5%BD%95%E5%B1%8F_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220322120616.gif)

![3](https://storage.deepin.org/thread/202203221208373015_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220322071020.png)

![4](https://storage.deepin.org/thread/202203221208373015_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220322071020.png)

![5](https://storage.deepin.org/thread/202203221208565541_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220322071105.png)

![6](https://storage.deepin.org/thread/20220322120906408_%E6%88%AA%E5%9B%BE_%E9%80%89%E6%8B%A9%E5%8C%BA%E5%9F%9F_20220322071657.png)
