---
title: Deepin1541安装Caffe
description: 
published: true
date: 2022-05-07T07:47:22.069Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:31:31.568Z
---

## 简介

本文有深度论坛用户（deepin207）分享，
[原文地址](https://bbs.deepin.org/forum.php?mod=viewthread&tid=145765)

此安装仅仅针对intel核显及NVIDIA独显用户

cpu：intel core i7 6700Q

显卡：NVIDIA gt960m

操作系统： deepin15.4.1 x64

## 步骤

### cuda及cudnn安装

#### 安装nvidia-bumblebee，实现双显卡切换

对于笔记本用户来说，一直开着独显的话发热量会明显增大，并且耗电也会变快，所以需要安装bumblebee来切换显卡，平时只用核显就足够了，需要运行cuda或者玩游戏的话才开启独显。

#### 安装cuda开发工具

cuda在linux下的开发工具基本上够用了，有基于eclipse 的nsight，有visual profiler性能分析工具，还有pycuda库实现对python运算的加速。但是我以前在deepin上面尝试安装官方的.run包，均以失败告终，很容易把电脑搞崩溃。最近终于找到了从软件源直接安装cuda的方法。

具体安装方法：

1. 安装nvidia-bumblebee

```
sudo apt update
sudo apt install bumblebee bumblebee-nvidia nvidia-smi
```

一行命令搞定nvidia驱动、bumblebee切换程序、和显卡状态监控程序。

不用管nouveau驱动，系统会自己屏蔽掉。

2. 重启

```sudo reboot```

重启后测试

```nvidia-smi```

和

```optirun nvidia-smi```

![](http://img.blog.csdn.net/20170920014959449?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHV6aGFuYm8yMDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```optirun -b none nvidia-settings -c :8```

（默认情况下，使用Intel显卡，如果要使用nvidia显卡，需要在命令前加optirun）

3. 安装配置g++,gcc

因为cuda版本原因，cuda8之前都只支持g++-4.8,gcc-4.8

所以gcc需要降级

```sudo apt install g++-4.8 gcc-4.8```

4. 更改软连接

```
cd /usr/bin
sudo rm gcc g++
sudo ln -s g++-4.8 g++
sudo ln -s gcc-4.8 gcc
```

5. 然后下载开发工具

```sudo apt install nvidia-cuda-dev nvidia-cuda-toolkit nvidia-nsight nvidia-visual-profiler```

6. 使用nsight的方法为:在终端下输入

```optirun nsight```

![](http://img.blog.csdn.net/20170920015015566?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHV6aGFuYm8yMDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 安装pycuda扩展

有的同学有使用python 写cuda的需求，推荐pycuda，能够自定义核函数，并且里面带的GpuArray这个数据结构很好用安装方法为：

```sudo apt install python-pycuda```

写一个简单的测试代码

```python
from __future__ import print_function
from __future__ import absolute_import
import pycuda.autoinit
import pycuda.gpuarray as gpuarray
import pycuda.driver as cuda
import numpy
import time

free_bytes, total_bytes = cuda.mem_get_info()
exp = 10
while True:
   fill_floats = free_bytes / 4 - (1<<exp)
   if fill_floats < 0:
       raise RuntimeError("couldn't find allocatable size")
   try:
       print("alloc", fill_floats)
       ary = gpuarray.empty((fill_floats,), dtype=numpy.float32)
       break
   except:
       pass

   exp += 1

ary.fill(float("nan"))

print("filled %d out of %d bytes with NaNs" % (fill_floats*4, free_bytes))

time.sleep(10)
```

另存为cuda_test.py

然后

```optirun python python cuda_test.py```

![](http://img.blog.csdn.net/20170920015046578?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHV6aGFuYm8yMDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

至此cuda的安装完成，为了用NVIDIA显卡加速深度学习，还需要安装cudnn

#### cudnn详细安装步骤

1. 从官网上下载cudnn的安装包。

2. 将安装包解压,将此安装包放在常用的放源码路径下即可，并在当前路径下进行解压,解压后的文件夹名为cuda。

3. 在终端上编辑如下代码：

```
cd cuda/include
sudo cp *.h /usr/include (这里是你安装cuda的地址，由于使用apt安装，系统讲cuda直接安装在/usr目录)
cd cuda/lib64
sudo cp libcudnn.so.7.0.1 /usr/lib
sudo cp libcudnn_static.a /usr/lib
sudo ln -s /usr/libcudnn.so.7.0.1 /usr/libcudnn.so.7
sudo ln -s /usr/libcudnn.so.7 /usr/libcudnn.so
```

至此cudnn安装完毕，第一步完成。

### 安装opencv编译器

opencv的编译安装过程用到cmake，cmake命令行参数配置太复杂，这里采用他的图形化工具

1. 安装OpenCV所需要的库

```
sudo apt-get install build-essential （如果按照之前的流程一路走下来，此步骤省略，因为会造成gcc/g++从4.8升级到当前最新版本，后面的cuda及cudnn加速程序编译报错）
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
sudo apt-get install libglu1-mesa-dev （OpenGL相关的依赖项）
```

2. 从github上下载OpenCV源代码

```git clone https://github.com/opencv/opencv.git```

3. 安装cmake的GUI工具，方便配置

```sudo apt-get install cmake-gui```

然后打开该软件，输入OpenCV源码在本地的路径，已经编译配置过程产生的中间文件保存在本地的路径（一般是建一个build目录）

![](http://img.blog.csdn.net/20170920015110602?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHV6aGFuYm8yMDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

- 路径配置完之后，点击左下角的Configure按钮，开始执行依赖检查等环境配置

- 成功之后，会在上面产生一个列表，是将来编译时需要的一些环境配置，一般默认即可，这里我们稍作修改，修改CMAKE_BUILD_TYPE为Release，修改CMAKE_INSTALL_PREFIX为/usr/local，选中WITH-OPENGL

- 完了之后，点击左下角的Generate按钮，将会根据上面的配置生成Makefile文件

4. 在build路径下执行命令

make -j8 (多线程编译，注意散热，这里的数字8根据自己计算机cpu核心的个数自行调整)
编译过程耗时很久，大概一个半小时左右，编译完成之后安装即可

```
sudo make install
sudo ldconfig
```

测试opencv安装是否成功，opencv完全编译安装会安装对应的python接口，在终端中简单测试即可

![](http://img.blog.csdn.net/20170920015216074?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHV6aGFuYm8yMDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

至此opencv3.2安装完成

### caffe的安装

1. 依赖库安装

```
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libhdf5-serial-dev protobuf-compiler  
sudo apt-get install --no-install-recommends libboost-all-dev 
```

安装ATLAS，输入下述命令：

```sudo apt-get install libatlas-base-dev```

安装剩余依赖项

```sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev```

2. 删除deepin软件源中太新的库，这些库采用比gcc/g++4.8.5更新的编译器编译，有不兼容部分，后续的caffe编译中会出现找不到对应函数的情况

```
sudo apt-get remove libleveldb-dev
sudo apt-get remove libprotobuf-dev
sudo apt-get remove libgflags-dev
sudo apt-get remove libgoogle-glog-dev
```

3. 重新编译安装上述库

leveldb

```
git clone https://github.com/google/leveldb.git
cd leveldb/
make -j8
```

安装leveldb

```
sudo cp -r leveldb/include/leveldb/ /usr/include/
sudo cp leveldb/out-shared/libleveldb.so.1.20 /usr/lib/
sudo ln -s /usr/lib/libleveldb.so.1.20 /usr/lib/libleveldb.so.1
sudo ln -s /usr/lib/libleveldb.so.1 /usr/lib/libleveldb.so
sudo ldconfig
```

查看安装是否成功

```
    ls /usr/lib/libleveldb.so*
    #显示下面 3 个文件即安装成功
    /usr/lib/libleveldb.so.1.20
    /usr/lib/libleveldb.so.1
    /usr/lib/libleveldb.so
```

protobuf编译安装

deepin库中默认采用的是protobuf-3.0x，而caffe的github代码中兼容protobuf2.5

```
git clone https://github.com/google/protobuf.git
cd protobuf/
git checkout v2.5.0
# 依赖gtest 1.5
git clone https://github.com/kgcd/gtest.git
./autogen.sh
./configure --prefix=/usr/
make -j8
make check -j8
sudo make install
sudo ldconfig
```

![](http://img.blog.csdn.net/20170920015251627?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHV6aGFuYm8yMDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

gflags安装

```
git clone https://github.com/gflags/gflags.git
cd gflags
mkdir build
cd build
export CXXFLAGS="-fPIC" && cmake .. && make VERBOSE=1
make -j8
sudo make install
```

glog安装

```
git clone https://github.com/google/glog.git
cd glog
./autogen.sh
./configure
make
sudo make install
```

下载caffe源码

```git clone https://github.com/BVLC/caffe.git```

修改Makefile.config

```
cp Makefile.config.example Makefile.config  
vim Makefile.config 
```

根据个人情况修改

```
    #USE_CUDNN := 1
    改为
    USE_CUDNN := 1
```

opencv3版本

```
    #OPENCV_VERSION := 3 
    改为
    OPENCV_VERSION := 3 
```

使用python layer

```
    #WITH_PYTHON_LAYER := 1 
    改为
    WITH_PYTHON_LAYER := 1 
```

修改CUDA_DIR

```
# CUDA directory contains bin/ and lib/ directories that we need.
CUDA_DIR := /usr/local/cuda
 # On Ubuntu 14.04, if cuda tools are installed via
# "sudo apt-get install nvidia-cuda-toolkit" then use this instead:
# CUDA_DIR := /usr
```

改为：

```
# CUDA directory contains bin/ and lib/ directories that we need.
# CUDA_DIR := /usr/local/cuda
# On Ubuntu 14.04, if cuda tools are installed via
# "sudo apt-get install nvidia-cuda-toolkit" then use this instead:
CUDA_DIR := /usr
```

最重要的一项

```
1 INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include
2 LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib
```

修改为

```
1 INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial /usr/liclude /usr/include/leveldb /usr/local/include/gflags /usr/include/google/protobuf
2 LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr /usr/lib/x86_64-linux-gnu/hdf5/serial
```

修改Makefile

```LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_hl hdf5```

修改为
```LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_serial_hl hdf5_serial```

至此准备工作结束，开始编译

```
make all -j8
make test -j8
```

编译完成，以最简单的MNIST手写体数据集做一个简单测测试

下载MNIST数据

```data/mnist/get_mnist.sh```

![](http://img.blog.csdn.net/20170920015317945?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHV6aGFuYm8yMDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

数据格式转换

```optirun examples/mnist/create_mnist.sh```

![](http://img.blog.csdn.net/20170920015330111?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHV6aGFuYm8yMDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

训练LeNet-5超参数(hyperparameters)

```optirun examples/mnist/train_lenet.sh```

![](http://img.blog.csdn.net/20170920015341967?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHV6aGFuYm8yMDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![](http://img.blog.csdn.net/20170920015351626?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHV6aGFuYm8yMDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

测试集上做预测测试

```
optirun build/tools/caffe.bin test \
-model examples/mnist/lenet_train_test.prototxt \
-weights examples/mnist/lenet_iter_10000.caffemodel \
-iterations 100
```

![](http://img.blog.csdn.net/20170920015403052?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHV6aGFuYm8yMDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![](http://img.blog.csdn.net/20170920015412404?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHV6aGFuYm8yMDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

至此，全部教程结束。
