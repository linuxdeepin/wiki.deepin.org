---
title: ML训练框架环境搭建篇
description: 
published: true
date: 2022-10-25T01:58:01.011Z
tags: 
editor: markdown
dateCreated: 2022-08-01T05:15:59.860Z
---

## 前言

在 deepin 社区版 20.6 发布后，社区小伙伴对在 deepin 上如何进行机器学习表现出浓厚的兴趣。为此，咱们趁热打铁的介绍 Pytorch、TensorFlow、PaddlePaddle 三款主流训练框架在 deepin 的环境搭建全过程，其中不乏踩过的坑和收获的经验，包括在Linux上安装显卡驱动、配置 Docker、部署训练框架等。

 

下面是为本次试验准备的软硬件基础环境：
系统 	Deepin 20.6
CPU 	Intel i9-10900k
显卡 	Nvidia RTX3090
内存 	Corsair 32G DDR4 2133MHZ ×2
硬盘 	Micron SSD 512G + Western Digital Corp HDD 2TB

 
## 1. 安装显卡驱动

我们首先从显卡驱动的安装谈起，因为众所周知，在机器学习领域利用显卡对模型进行计算和加速发挥作用举足轻重的作用。这里的显卡驱动安装指的是安装 NVIDIA 的闭源驱动，不使用开源的 nouveau 驱动的原因是它无法调起 NVIDIA 显卡内部的 CUDA 单元，也无法使用 NVIDIA 的 cuDNN 库为整个训练过程进行加速等等。
 
### 1.1 驱动状态检测

首先我们需要在终端中执行命令 nvidia-smi 来检查是否已经安装了 NVIDIA 闭源驱动。nvidia-smi（简称 NVSMI）是一个跨平台工具，主要提供监控 NVIDIA GPU 使用情况和更改 其状态的功能，这个工具是 N 卡闭源驱动附带的，驱动安装好之后就会有这个命令，反之如果没能正确配置 N 卡的闭源驱动，则执行此命令会产生报错。

如果执行该命令发现报错或提示命令不存在，那么就需要去 NVIDIA 官网下载合适的闭源驱动。
 
### 1.2 官网驱动下载

在NVIDIA 官网查询显卡匹配的驱动版本（https://www.nvidia.com/Download/index.aspx)，对于 deepin 而言，Operating System 一栏直接选择 Linux 64 bit 即可，其余选项则根据自己显卡的情况进行选择即可。而本次教程的选择如下图所示：
 

 

选完后点击下方的 SEARCH 按钮即可进入下载界面。

 

 
### 1.3 显卡驱动安装

完成下载后会得到一个 xxx.run 文件，这个文件就是显卡驱动，如果在其它一些最小系统安装它则需要预装一些依赖项，但是在 deepin 中，这些依赖项都是已经集成在了系统里，直接按下面步骤进行即可。

 

第一步：按组合键（<kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F2</kbd>） 打开 tty ，按照提示输入用户名和密码，然后执行 sudo init 3 关闭图形界面；

 

第二步：在 tty 中使用 `sudo sh /YourSavePath/xxx.run` 命令安装显卡驱动，其中 YourSavePath 是你的 xxx.run 文件的保存位置，你也可以像我一样，先把目录切到目标位置，然后执行 `sudo sh xxx.run`。随后就会进入正式的安装过程：
 

 
 

 

第三步：等待安装完成且没有报错后，使用 `reboot` 命令重启系统，然后在终端中运行 `nvidia-smi` 命令查看显卡相关信息，或是使用 `nvidia-settings` 命令可以打开 NVIDIA 图形设置界面。若这两个命令均可运行则说明显卡驱动安装成功。
 

 
 

 

 

 
## 2. 安装配置Docker

显卡驱动安装完成以后，我们需要继续安装 CUDA 和 cuDNN 以发挥显卡的全部性能，然后再编译或者下载我们需要的训练框架。但是这样从头进行配置会非常麻烦且容易出错。所幸的是绝大部分机器学习厂商都提供了可以开箱即用的 Docker 镜像，因此我们将选择使用配置好的 Docker 来加速环境搭建这一过程。接下来我们将继续自底向上，在安装好了显卡驱动的基础上安装和配置训练用的 Docker。
 

 
### 2.1安装 Docker

在 deepin 内部存在 docker 和 docker-ce 两个包，这里我们选择安装 docker-ce 这个包：

 

更新源
```
$ sudo apt update
```
 

安装 Docker
```
$ sudo apt install docker-ce
```
当看到和直接执行 nvidia-smi 命令的输出差不多的界面时，即表示我们的 docker 环境可以正确调起部署在宿主机上的 N 卡。

 

测试 Docker
```
$ sudo docker run hello-world
```

 

输入 `sudo docker run hello-world` 命令测试 Docker，出现下图则说明测试 Docker 安装成功。
 

 
### 2.2安装测试nvidia-container-toolkit

原本在官方版本的 Docker 中不支持调用 N 卡，只有 NVIDIA 自己维护的 Docker 中才能调起 N 卡，后来有了 nvidia-container-toolkit 工具以及 Docker 上游社区将相关特性合入主线，这才支持在官方版本的 Docker 中调起 N 卡。比较凑巧的是，deepin 仓库源里的 docker-ce 恰好支持这些特性，这使得我们直接安装配置 nvidia-container-toolkit 即可：

 

添加源，可手动选择使用 debian 10 的包
```
$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -

$ curl -s -L https://nvidia.github.io/nvidia-docker/debian10/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```
 

安装nvidia-container-toolkit并重启docker
```
$ sudo apt update

$ sudo apt install -y nvidia-container-toolkit

$ sudo systemctl restart docker
```
 

测试nvidia-container-toolkit
```
$ sudo docker run --gpus all --rm nvidia/cuda:11.0-base nvidia-smi
```

 

当看到和直接执行 nvidia-smi 命令的输出差不多的界面时，即表示我们的 docker 环境可以正确调起部署在宿主机上的 N 卡。

在这个过程中还可能遭遇以下报错：

> ldcache error: open failed: /sbin/ldconfig.real: no such file or directory\\n\""": unknown.
{.is-danger}

此时执行 `sudo ln -s /sbin/ldconfig /sbin/ldconfig.real` 即可解除报错。

 

 
## 3. 部署训练框架

在安装了 Docker 和 nvidia-container-toolkit 之后，机器学习的基础环境就准备完成。现在在 Docker 上运行对应训练框架的 Docker 镜像，就可以进行机器学习训练。

首先需要在 Docker 仓库(https://hub.docker.com/) 寻找自己需要的 Docker 容器（比如：Pytorch），然后调用命令获取对应的镜像 。接下来，我们将使用Pytorch、TensorFlow、PaddlePaddle三种训练框架演示，同时使用 MNIST 数据集（美国国家标准与技术研究院收集整理的大型手写数字数据库）进行训练，从而验证我们的训练框架是否成功部署，可以用来训练模型。

 
### 3.1 Pytorch

点开 https://hub.docker.com/ 搜索 pytorch，并下拉找到 Downloads 数量最高的那个，点进去后切到 Tags 一栏，下面就是各个版本的 Pytorch Dockers 了。
 

 

此时需要注意的是它的命名规则：

- latest 表示最后打包的 Docker 镜像
- 其余的按照 版本号-CUDA版本-cuDNN版本-镜像类型 的规则命名
- 版本号意为 Pytorch 项目的版本号；CUDA版本和cuDNN版本则是镜像内部的N卡GPU加速配置，它和宿主机上的驱动相关；最后的镜像类型分为 runtime、devel 两个版本，runtime 为仅有运行环境的版本，适合部署至最终生产环境，devel 内部包含 Pytorch 的开发环境，方便对 Pytorch 进行调试和开发等等，适合开发阶段使用。

这里查询到我们下载的驱动版本 515.48.07 对应的 CUDA 版本是 11.7，但我们希望使用 1.11.0 版本的 Pytorch，同时我们处于开发阶段，有潜在的调试 Pytorch 的需求：因此我们拉到下面经过一番对比，最终选择最接近的 
1.11.0-cuda11.3-cudnn8-devel 这个 tag， 复制它右边的命令到终端内部，并在前面加上 sudo 指令，最后敲下回车：
 

 

当终端提示符再次出现时，即表示 Docker 镜像拉取完成。

当镜像拉取结束后，我们就可以启动 Docker 尝试使用 MNIST 数据集进行训练了。由于一般的数据集都特别大，因此我们需要先将数据集下载至宿主机的硬盘中，再将对应的路径挂载至 Docker 内部。比如本次我们就将数据集和训练的代码打包放在 
MNIST/home/deepin/Desktop/mnist/pytorch 这个路径下，然后我们使用以下命令启动 Docker 并且移动到挂载目录下：

 
```
$ sudo docker run -it --gpus all -v /home/deepin/Desktop/mnist/pytorch:/test pytorch/pytorch:1.11.0-cuda11.3-cudnn8-runtime /bin/bash

$ cd /test
```
接下来将测试整体环境是否可以完成模型训练，由于我们已经切换至挂载目录，因此直接执行我们写好的 Python 脚本即可：python3 train.py
 

 

最后我们从以上结果可以观察到训练设备为 CUDA（即NVIDIA的GPU），此时即证明我们的环境搭建成功，可以进行正式的开发工作了。
 

### 3.2 TensorFlow

同样地，我们在 Docker Hub 上寻找所需要的训练框架 Docker，然后使用相应命令获取Tensorflow Docker。Tensorflow 的 Docker 命名规则和 Pytorch 的有些不一样，有需要的小伙伴可以自行去搜索相关的资料。这里我们直接拉取它最后打包的版本：
```
$ sudo docker pull tensorflow/tensorflow:latest-gpu
```

 

然后重复 3.1 节的步骤：
```
$ sudo docker run -it --gpus all -v /home/deepin/Desktop/mnist/tensorflow:/test tensorflow/tensorflow:latest-gpu /bin/bash

$ cd /test

$ python train.py
```

 

此时可以看到 Docker 检测到 GPU 设备，可以调用 GPU 进行模型训练，并在最终完成 MNIST 数据集的训练，五个 EPOCH 共计用时9.3825秒，最终训练 ACC 达到97.73%，LOSS 收敛至0.756。

 

 
### 3.3 PaddlePaddle

同样的，在Docker Hub 中寻找我们需要的训练框架镜像，并使用命令获取镜像，PaddlePaddle 的命名规则和 Pytorch 比较类似，参考 3.1 节的选型规则即可：
```
$ sudo docker pull paddlepaddle/paddle:2.0.1-gpu-cuda11.0-cudnn8
```

 

然后重复 3.1 节的步骤：
```
$ sudo docker run -it --gpus all -v /home/deepin/Desktop/mnist/paddlepaddle:/test paddlepaddle/paddle:2.0.1-gpu-cuda11.0-cudnn8 /bin/bash
```

 

最终在 PaddlePaddle 训练框架下对 MNIST 数据集进行训练完成，可以看到它成功调起了 cuDNN 库，同时共五个 EPOCH 训练用时为 10.3678 秒，最终 ACC 达到 98.57%，LOSS 收敛至0.0850。

 

##  4. 总结

以上，我们成功的在 deepin 中实现了三个主流训练框架的部署，包括：

- 安装了支持 CUDA 的闭源显卡驱动；
- 使用 Docker CE 和对应的 Docker 镜像完成了训练框架的部署，有效避开了训练框架部署的繁琐流程，简化了 Pytorch、TensorFlow 和 PaddlePaddle 的部署过程；
- 在 Docker 内使用 Pytorch、TensowFlow、PaddlePaddle 成功调用GPU进行训练；

社区小伙伴也可以按照上述的流程在 deepin 上进行不同的尝试，也欢迎在评论区留言提出意见和建议！

 

 
参考资料

1. 深度学习环境搭建：
	https://blog.csdn.net/qq_18625805/article/details/125223467

2. Docker CE 安装：
	https://blog.csdn.net/u013730110/article/details/107691623

3. nvidia-container-toolkit安装：
	https://blog.csdn.net/KingOfMyHeart/article/details/122351152

4. Pytorch官网：https://pytorch.org/

5. PytorchGitHub：https://github.com/pytorch/pytorch/

6. 文中Pytorch-Docker地址：
	https://hub.docker.com/r/pytorch/pytorch

7. 文中Tensorflow-Docker地址：
	https://hub.docker.com/r/tensorflow/tensorflow

8. 文中Paddlepaddle-Docker地址：
	https://hub.docker.com/r/paddlepaddle/paddle
