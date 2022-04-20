## 前言

笔者的系统与硬件配置：

- 系统：Deepin 15.5
- CPU：Intel i7 6700HQ
- 显卡：NVIDIA GTX 950M
- 电脑型号：惠普(HP)WASD 暗影精灵

![](http://wx1.sinaimg.cn/mw690/865d961cly1fm1rq7d95qj20mn0jpany.jpg)

## 安装NVIDIA驱动

为了兼容性，Linux系统默认使用开源显卡驱动。对于Debian系，双显卡笔记本采用bumblebee（大黄蜂）进行切换（**默认使用核显以省电，在命令前加optirun则会使用独显**）

Deepin应用商店有深度显卡驱动管理器，大家可以尝试使用这个工具进行显卡驱动的安装，还有自带的驱动管理器也可以尝试下。如果这两个工具都不行，可以使用命令安装：

```bash
sudo apt install bumblebee bumblebee-nvidia
```

之后重启即可。

![](http://wx3.sinaimg.cn/mw690/865d961cly1fm1rq0a1o4j20yb0jkqh2.jpg)

重启之后，启动器里会多出一个NVIDIA X服务器设置，点开会报错，提示让以root运行nvidia-xconfig。不要真的这样做，这是因为我们没有在前面使用optirun ，所以独显并没有启用。若如此做，会在/etc/X11/下生成xorg.conf文件，重启后进系统会黑屏，无法进入桌面，只需Ctrl+Alt+F2进tty，然后删掉这个文件重启即可。

打开NVIDIA X服务器设置可以使用命令：

```bash
optirun -b none nvidia-settings -c :8
```


## 安装cuda和cudnn

安装cuda：

```bash
sudo apt install nvidia-cuda-dev nvidia-cuda-toolkit
```

安装cudnn：

到官网下载cudnn-8.0-linux-x64-v6.0.tgz，解压，内容如下：

![](http://wx3.sinaimg.cn/mw690/865d961cly1fm1s41pnaaj20ch05maa8.jpg)

libcudnn.so和libcudnn.so.6是两个链接文件。

然后把cudnn.h复制到/usr/include/目录下，lib64下面的这些文件复制到/usr/lib/目录下。（由于使用apt安装，系统将cuda直接安装在/usr目录）

## 安装pyenv和Python

可选。也可以使用anaconda这个Python发行版，或使用系统自带的Python。

pyenv是Python版本管理工具。

安装依赖：

```bash
sudo apt install make git build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev
sudo apt install libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev
```

安装pyenv：

```
curl -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash
```

修改.bashrc：

```bash
vi ~/.bashrc
# 加上如下部分
export PATH="/home/"$USER"/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
# 保存并运行source使得.bashrc生效
source ~/.bashrc
```

安装Python（推荐3.5版本）

```bash
# 安装python 3.5.4
pyenv install 3.5.4
# 将python 3.5.4设为默认版本
pyenv global 3.5.4
```

## 安装TensorFlow

```bash
pip install tf-nightly-gpu
```

这样，基于Deepin 15.5的GPU版TensorFlow环境就搭建好了。