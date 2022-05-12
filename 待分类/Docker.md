---
title: Docker
description: 
published: true
date: 2022-05-07T07:47:22.190Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:33:17.655Z
---

## 简介

Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。

## 关于 Deepin 中的 Docker

Deepin 官方的应用仓库已经集成了 docker，但不是类似于 docker-ce 这样的最新版本。由于 Deepin 是基于 debian 的 unstable 版本开发的，通过 `$(lsb_release -cs)` 获取到的版本信息为 **unstable**，而 docker 官方源并没支持 debian 的 **unstable** 版本，因此使用 docker 官方教程是安装不成功的。如果你需要安装 docker-ce，请遵循下面的步骤进行安装：

## 在 Deepin 中安装 Docker 最新版的方法

1. 如果以前安装过老版本，要确保先卸载以前版本

```sudo apt-get remove docker.io docker-engine```

2. 安装密钥管理与下载相关的工具

```
 // 密钥管理（add-apt-repository ca-certificates 等）与下载（curl 等）相关的工具
sudo apt-get install apt-transport-https ca-certificates curl python-software-properties software-properties-common
```

3. 下载并安装密钥

>鉴于国内网络问题，强烈建议使用国内源，官方源请在注释中查看。

国内源可选用[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/)或[中科大开源镜像站](http://mirrors.ustc.edu.cn)，示例选用了中科大的。

为了确认所下载软件包的合法性，需要添加软件源的 GPG 密钥。

```
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/debian/gpg | sudo apt-key add -
// 官方源，能否成功可能需要看运气。
// curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```

4. 查看密钥是否安装成功

```sudo apt-key fingerprint 0EBFCD88```

如果安装成功，会出现如下内容：

```
  pub   4096R/0EBFCD88 2017-02-22              Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88  
  uid     Docker Release (CE deb) <docker@docker.com>  
  sub   4096R/F273FCD8 2017-02-22
```

5. 在 source.list 中添加 docker-ce 软件源（请先查看后面的 **Note**）：

```
sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/debian wheezy stable"
// 官方源
// sudo add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/debian wheezy stable"

// 15.10 会提示  aptsources.distro.NoDistroTemplateException: Error: could not find a distribution template for Deepin/stable
// 这里我们通过编辑 sudo vim /etc/apt/sources.list 添加一行即可，原因未知
sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/debian stretch stable"
```

**Note：** 官方在 `wheezy` 位置使用的是 `$(lsb_release -cs)`，但之前已经解释过，在 deepin 里运行它得到的是 **unstable**，docker 官方不支持 unstable 版本！因此直接使用官方教程的命令会安装失败。

**更改方法：**将上述命令中的版本名称 wheezy，替换成 deepin 基于的 debian 版本对应的代号。查看版本号的命令为：`cat /etc/debian_version`.

举例：

a). 对于 deepin 15.5，我操作上面的命令得到 debain 版本是 8.0，debian 8.0 的代号是 **jessie**，把上面的 wheezy 替换成 jessie，就可以正常安装 docker 了。

b). deepin 15.9.2 基于 debian 9.0 , debian 9.0 的代号为 **stretch**, 所以 deepin 15.9.2 上完整的添加信息为:

```
sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/debian stretch stable"
```

c) 对于Deepin 20，使用的是Debian 10 （Buster），所以对应信息：

```
sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/debian buster stable"
```

6. 更新仓库

```sudo apt-get update```

7. 安装 docker-ce

由于网络不稳定，可能会下载失败。如果下载失败了，可以多试几次或者找个合适的时间继续。

```sudo apt-get install docker-ce```

~~在安装完后启动报错，查看 docker.service 的 unit 文件，路径为 /lib/systemd/system/docker.service，把 ExecStart=/usr/bin/dockerd -H fd:// 修改为 ExecStart=/usr/bin/dockerd，就能正常启动 docker 了~~

**Note：**经测试，在 Deepin15.9 中已不需要做修改可直接启动 docker-ce

8. 启动 docker：

```systemctl start docker```

9. 查看安装的版本信息

```docker version```

10. 验证 docker 是否被正确安装并且能够正常使用

```
sudo docker run hello-world
```

如果能够正常下载，并能够正常执行，则说明 docker 正常安装。

11. 让普通用户也能运行 docker

默认情况下，普通用户运行 docker 会有权限问题，每次运行都得加 sudo，很麻烦。把你的账号加到 docker 用户组后就不用加 sudo 了：

```
sudo usermod -aG docker $USER
```

然后注销用户重新登录即可。

## 更换国内的 docker 加速器

如果使用 docker 官方仓库，速度会很慢，所以更换国内加速器就不可避免了。

方式一：使用阿里云的docker加速器。

1. 在阿里云申请一个账号  

打开连接 <https://cr.console.aliyun.com/#/accelerator> 拷贝您的专属加速器地址。

2. 修改 daemon 配置文件 /etc/docker/daemon.json 来使用加速器（下面是4个命令，分别单独执行）

**Note：** 这里的 <https://jxus37ad.mirror.aliyuncs.com> 是申请者的加速器地址，在此仅仅用于演示，而使用者要个根据自己的使用的情况填写自己申请的加速器地址。

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://jxus37ad.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

方式二：使用 docker-cn 提供的镜像源

1. 编辑 /etc/docker/daemon.json 文件，并输入 docker-cn 镜像源地址

```
sudo nano /etc/docker/daemon.json
```

输入以下内容

```
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

2. 重启 docker 服务

```
sudo service docker restart
```

## 禁止开机自启

默认情况下 docker 是开机自启的，如果我们想禁用开机自启，可以通过安装 chkconfig 命令来管理 Deepin 自启项：

```
# 安装chkconfig
sudo apt-get install chkconfig

# 移除自启
sudo chkconfig --del docker
```

最后：Deepin 的这个文件格式真的是 Markdown 么？改得特别难受，是不是有 bug...
