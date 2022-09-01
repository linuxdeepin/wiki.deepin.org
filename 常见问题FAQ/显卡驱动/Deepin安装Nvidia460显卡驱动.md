---
title: Deepin安装Nvidia460显卡驱动
description: 
published: true
date: 2022-09-01T12:34:56.332Z
tags: nvidia, nvidia  显卡驱动
editor: markdown
dateCreated: 2022-06-28T02:47:46.436Z
---

> 警告：教程执行完成前请不要重启，否则重启后可能无法正常开机（无法进入桌面）！
{.is-warning}

如果教程执行过程中出错，可以去[我们的QQ群或者微信群](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45NTk4OC5odG1s)求助。

该教程仅适用于基于*Debian 10*的发行版，比如*Deepin v20*、*UOS 20*。
该教程不适用于*Ubuntu 20.04*。

# 安装步骤

1. 如果你用Nvidia官网的`.run`安装程序安装过驱动，请先卸载。
   **没装过的请跳过这一步。**
   卸载方法如下：

   1. 找到你之前的`.run`安装包，如果删了就再下载一次。**没装过`.run`或者不知道它是什么的，不用下载，请直接进行第2步！！！**

   2. 打开终端，运行如下命令：

   > 这里假设我的 .run 安装包位于主目录的 *Downloads* 文件夹，名为 *NVIDIA-Linux-x86_64-460.27.04.run* 。如果你的不是，请自行调整其中的文件夹和文件名。
   {.is-info}

   ```bash
   cd ~/Downloads
   sudo bash NVIDIA-Linux-x86_64-460.27.04.run --uninstall
   ```

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzlhZDNkZDY2YTdjOTlhYTZjMGY3ODg2NTRmODM1MTNkMjQwNTEucG5n)

   3. 出现的所有选项都选“Yes”和“OK”，不用管其中的警告和错误，把全部选项选完即可。

2. 为了避免出错，通过软件包管理器安装的旧版nvidia驱动也得卸载掉。在终端执行以下命令：

   ```bash
   sudo apt purge --autoremove '*nvidia*'
   ```

   所有选项回复`y`，如果有弹出式对话框，选“Yes”和“OK”。

3. 添加Debian的`buster-backports`（Debian 11 软件包向后移植至 Debian 10）软件源，该软件源提供了460.32驱动，并且和Deepin/UOS 20的依赖关系冲突不是很严重。

   在终端执行以下命令：

   ```bash
   echo 'deb http://mirrors.aliyun.com/debian buster-backports main contrib non-free' | sudo tee /etc/apt/sources.list.d/debian-buster-backports.list
   ```

4. 启用32位架构，然后更新软件包列表。在终端执行以下命令：

   ```bash
   sudo dpkg --add-architecture i386
   sudo apt update
   ```

   如果遇到以下错误：

   > 由于没有公钥，无法验证下列签名： NO_PUBKEY

   执行以下命令导入公钥：

   ```bash
   sudo apt-get update 2>&1 | tee /tmp/apt.tmp; cat /tmp/apt.tmp | grep 'NO_PUBKEY' | awk -F'NO_PUBKEY' '{print $2}' | sort | uniq | xargs sudo apt-key adv --keyserver keyserver.ubuntu.com --recv
   ```

   反复运行上面的命令，直到“**由于没有公钥，无法验证下列签名： NO_PUBKEY**”不再出现为止。

5. 安装`aptitude`，它是一个类似于`apt`的软件包安装器，但是解决依赖关系冲突的能力比`apt`更强大。在终端执行以下命令：

   ```bash
   sudo apt install aptitude
   ```

6. 用`aptitude`安装460驱动。注意这里不是直接回复`y`就能安装好，请仔细看下面的说明。

   还有，我截图里写的包名是 *nvidia-driver=460.32.03-1~bpo10+1* ，后来我觉得这样不好（以后发布新版本教程就得改），就换成了 *nvidia-driver/buster-backports* 。所以命令请以文字为准，不要参考截图中的命令。

   在终端执行以下命令：

   ```bash
   sudo aptitude install 'nvidia-driver/buster-backports'
   ```

   你会得到这样一个输出（可能和我的有细节上的差异）：

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzBhODliNjM1OTM0YTY0Njk5NjAwNTEzYjk0MjBlMGQwMTI1OTEwLnBuZw..)

   > nvidia-driver [未安装的]

   此时如果回复`y`，就不会安装`nvidia-driver`，相当于什么也没有发生。所以我们不能接受该解决方案，回复n，让它再给出一个解决方案。

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzk4MDg3MDU3MTdiNTE2MmY1M2E4ZTgxOGVmNzZhNDgyMTQzMTIzLnBuZw..)

   得到了这样一个解决方案。可以看到`nvidia-driver [未安装的]`不再出现，说明这就是我们要的解决方案。回复`y`。

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzAyZjY1ZjQyZTNhOGFhYzMzMmMyYjRmYWQ2ZjY1NDBhNDk2MTcucG5n)

   然后得到如上结果（你的和我的细节上可能有差异，比如要安装的软件包数量）。注意，这里的“**0 个将被删除**”很重要！**如果被删除的软件包数量大于0**，请务必再三确认，**如果你不认识要删除的软件包，不知道删除会有什么后果，千万不能回复y**。

   确认没问题后回复`y`，开始安装。

   注意：出现这些提示是正常的，这些不是错误。

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzcwMzM3YTFmZTllNTQ1M2ZiZjY0ODFjMzEyZTQzMzZiMTAyNjIwLnBuZw..)

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nL2VjMGExYjY0ZDQ4ZThkYzFlZDE0NWJjOTdmODRmMDZmNzQzMzUucG5n)

   遇到这种弹窗，回车确认即可，不用关心它说的内容，它说的内容总结起来只有一句话，装完驱动后要重启。

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzE3YmFhNDgwYjNiNWVjNzIyYzUxNTNkMjNjYTYwYzczMzY1NzIucG5n)

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzk1MDgyM2JmODg5ZjA3YzFiMGExYmI1M2U1NjhjODFmNjQzMDkucG5n)

   如果最后结束时没有任何错误字眼（`E:`，`Error`，`Failed`等，`W:`警告开头的警告不算）那就说明安装成功了。但是**教程还没结束**，请继续看。

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nL2Q0NjJmMmZiM2M3NmJjZGMzODcxMDI1ZTgyZWIzMjU2MjA1NzMucG5n)

7. 使用`aptitude`安装460驱动的64位附加组件，需要使用步骤6中提到的技巧，首先回复`n`拒绝“未安装的”方案，然后再回复`y`。

   ```bash
   sudo aptitude install 'nvidia-vulkan-icd/buster-backports' 'nvidia-smi/buster-backports' 'nvidia-settings/buster-backports'
   ```

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nL2Q4OTBjYzMzOTU1M2E5NDFiYzcxN2U1NjRkNWUxZTg0MTQ1NzkwLnBuZw..)

   最后结束时没有错误字眼，就说明成功了。注意你可能没有这行字：`当前状态：18 (-2) 可升级`，不用在意，有没有都正常，这属于细节上的差异。

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzdhOWIxNzBjNDA1ZmRkODY5ZGVjOWM5ZmQ5YTE4MjRkMjY0NzcucG5n)

8. 使用`aptitude`安装驱动的32位附加组件，继续使用步骤6中提到的技巧，首先回复`n`拒绝“未安装的”方案，然后再回复`y`。

   ```bash
   sudo aptitude install 'libnvidia-glcore:i386/buster-backports' 'libnvidia-eglcore:i386/buster-backports' 'nvidia-driver-libs:i386/buster-backports' 'libnvidia-glvkspirv:i386/buster-backports' 'libnvidia-ml1:i386/buster-backports'
   ```

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzUxOTMyZWZjYmIwODg3ZTU2ODBiMzg2MzM2MzdkY2FlMTQzMzYyLnBuZw..)

   注意这些都不是错误，是正常现象：

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzdjOTcwYzY1MDUxNDI1NWI0ODc4NjZiZTk5Nzk4YTNlNjg4NDEucG5n)

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzBhNGYyOTNkNDMwMzMyZGMyMjNjYjJmMWY0OWY3YzI2MjQ2OTYucG5n)

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzMyZmZhYTVmOWJkM2YwZjY4N2QxMDQzMTdiOWZkMzZiMzI1OTIucG5n)

   最后顺利结束：

   ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzkzYzFjZWQ1OTBmNjU4NDI1OGEyZjkxN2ZjYjRkZTliMTYwNTYucG5n)

9. 安装Vulkan支持库。在终端执行以下命令：

   ```bash
   sudo aptitude install libvulkan1 libvulkan1:i386 vulkan-tools 'vulkan-utils|vulkan-tools'
   ```

10. 将新装的显卡驱动用于所有内核版本。执行以下命令：

    ```bash
    sudo update-initramfs -k all -u
    ```

    注意这不是错误，而是正常现象：

    ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzIxYjI3M2IyOWE4NjMwOTdkNmRkMjNmYjRmMDFmZDdlNjM0MzAucG5n)

11. 如果你是笔记本，还需要安装双显卡支持包。如果你是台式机，无需安装，但是安装了也没有副作用。

    注意该双显卡支持包只能用于 *LightDM* 显示管理器，如果你用 *GDM* 或者其他显示管理器，则不起作用，独显可能无法驱动。Deepin和UOS默认使用 *LightDM*，如果你安装了其他桌面换了显示管理器，请自行切换回 *LightDM* 。如果你是其他发行版，请自行安装 *lightdm*

    安装方法如下：

    ```bash
    wget https://file.winegame.net/packages/deepin/mgpu/mgpu-prime_0.2.0_amd64.deb
    sudo apt install -y ./mgpu-prime_0.2.0_amd64.deb
    ```

    然后还需要打开Wine游戏助手的“Nvidia Prime渲染卸载”选项，否则某些游戏游戏打不开，提示没有可用的显卡驱动。打开方法如下：

    ![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzkxNDI0YTkxMzRlYmE4ZTM3NmRhYmUxMTJiMTgxNjUzMTE3MjgzLnBuZw..)

    注意左侧边栏Wine旁边的齿轮按钮正常不会显示，需要把鼠标放上去才会显示。

------

全部操作完成后重启即可，祝你好运。如果重启后无法正常开机，可以去[我们的QQ群或者微信群](https://hu60.cn/q.php/link.url.html?url64=aHR0cHM6Ly9odTYwLmNuL3EucGhwL2Jicy50b3BpYy45NTk4OC5odG1s)求助。

重启后打开“NVIDIA X 服务器设置”，可以看到已经是460.32版本了。

![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nL2IzNmUxNjI5MThlM2RkZWQ4OTM4OWZjOTFmYjk5MjNiMTQxMzUxLnBuZw..)

![图片.png](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzY5NTBjNTAyMDgzOTQzZjM0ZGY2NDQwOWY0MGRkOGZhMTIxNzE0LnBuZw..)

------

如果你要安装CUDA功能，可以运行如下命令：

```bash
aptitude install libcuda1/buster-backports nvidia-cuda-dev/buster-backports nvidia-cuda-toolkit/buster-backports
```

和之前一样先回n再回y。

注意安装的版本是 CUDA 11，最新的，不过应该和 CUDA 9/10 应用程序兼容。