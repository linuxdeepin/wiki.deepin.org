---
title: Podman及容器技术介绍
description: 
published: true
date: 2023-06-15T09:33:07.835Z
tags: 
editor: markdown
dateCreated: 2023-06-15T08:58:13.916Z
---

此帖来自于论坛用户：donaldsebleung的分享

# 教程对象
对电脑及 Linux 技术有兴趣的坛友，本教程假设您对虚拟化的概念有基本的认知，最好之前接触过 VirtualBox 或相关技术

# 容器概述
容器是一种轻量的虚拟化技术；相对于虚拟机，容器一般并不尝试虚拟化硬件及操作系统内核，只虚拟化文件系统、进程、网络等系统组件（俗称用户空间）；因此容器的镜像大小及启动时间大大缩短，启动时间从几秒到几十秒不等缩短到约一秒甚至以毫秒计
![2023-6-15_23651.png](/2023-6-15_23651.png)
（以上图片取自 Docker 官网： https://www.docker.com/wp-content/uploads/2021/11/docker-containerized-and-vm-transparent-bg.png ）
当然，容器也有其弊端：由于容器一般不选择虚拟化硬件、操作系统内核，因此跟宿主系统及其他容器的隔离不及虚拟机；故容器遭遇入侵攻击时，黑客从容器跳到宿主系统继续攻击的难度相对较易

容器技术在 Linux 上最为活跃及成熟，但容器概念本身并不限于 Linux，其他操作系统也有类似的概念与技术：

- 特定 Windows Server 版本支持 Windows 容器
- FreeBSD 有 jails 概念，可视为强化版的容器
- Oracle Solaris 及其近亲如 SmartOS、illumos 有 zone 的概念，据宣传相比 jails、容器 更为安全
# Podman 概述

Podman 是一个用于创建及管理个别容器的引擎及客户端，一般适合个人用户及开发者使用。另外一个常用的容器引擎是 Docker，在此就不详细阐述了

相对于 Docker，Podman 拥有以下优势：

- Docker 依靠长期运行的 Docker Daemon 服务，该服务以 root 用户运行，故管理容器需要使用 sudo 提权或把相关用户加到 docker 群组，而且容器里的 root 尽管受到各种限制也是宿主系统的 root ，故有安全隐患；Podman 则不依靠 root 运行的服务，故一般用户能直接创建管理容器，而且非 root 运行的容器里，容器里的 root 用户在宿主系统上只是一般用户（例如 nobody ），故黑客倘若控制了容器里的 root 再逃到宿主系统后也只是普通用户，故不能轻易对宿主系统进行破坏，这样更安全；
- Docker 引擎依靠 Docker 运行时，该运行时由于历史原因并不完全兼容后期定制的统一 容器运行时（CRI） 标准，故若需要用于 Kubernetes 容器编排工具 等生产工具，则需要额外的兼容层与配置；Podman 引擎则依靠 CRI-O 运行时，完全兼容 CRI 标准，没有 Docker 在这方面的限制
从用户及开发者的角度看，Podman 客户端跟 Docker 客户端大体上一样，大部分简单情况下把命令里的 docker 替换为 podman 即可，但未必适用于一些更复杂的情况
# 初尝 Podman
本教程假设您在 deepin 23 上操作，若您在使用 deepin 20 或其他 Linux 发行版，则可能需要修改相应操作细节与命令

本教程不适用于非 Linux 操作系统，例如：Windows，但 Windows （或苹果等等）的用户也可以透过创建 deepin 23 虚拟机，在虚拟机内跟随教程操作

# 安装 Podman
deepin 23 源里有 Podman 软件包，名称为 podman ，调用 APT 安装即可：
```
sudo apt update && sudo apt install -y podman
```
安装后可验证 podman 命令的存在及查询版本：
```
which podman
podman --version
```
输出：
```
/usr/bin/podman
podman version 4.3.1
```
# Podman 您好
让我们快速创建及运行一个 Hello World 容器吧：
```
podman run --rm --name hello-podman docker.io/library/hello-world:latest
```
输出：
```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
从输出里可以看到，Podman 做了以下事情：

1.Podman 创建容器时需要使用容器镜像，但相关镜像 docker.io/library/hello-world:latest 不在本地仓库里，所以 2.Podman 根据镜像全名从网上下载了相关镜像
3.Podman 从刚下载的镜像创建了一个名为 hello-podman （命令里 --name hello-podman 选项）的容器并运行了它
4.Podman 把容器的输出（STDOUT）转发到终端里显示
由于命令里使用了 --rm 选项，该容器运行完毕后 Podman 会把它自动删掉（但镜像会保留）
如果您细心阅读以上的英文输出，会发现输出提及 Docker Daemon，但其实使用 Podman 时 Docker Daemon 并不存在，所以可以忽略
容器镜像是创建运行容器的基础，其功能类似于 Word 模板，像容器这么复杂的东西总不能从零开始吧 ～

查询本地已下载的镜像使用 podman images 命令，输出如下：
```
REPOSITORY                     TAG               IMAGE ID      CREATED      SIZE
localhost/linuxdeepin/apricot  v20.8-compatible  6b03b24c2d82  12 days ago  3.3 GB
docker.io/library/hello-world  latest            9c7a54a9a43c  2 weeks ago  19.9 kB
```
您的输出或许不完全一样，但是倘若在 deepin 23 上安装启动过 deepin 20 兼容方案并运行了刚才的 Hello World 容器，则应该起码有以上三行输出
让我们看看每个列的意义吧：

- REPOSITORY：指镜像的全名称，repository 直译是「仓库」的意思。镜像全名一般分为三个部分，如 - -- docker.io/library/hello-world 可拆分为以下三个组件：
1. docker.io 是镜像仓库的名称，这里指的是 Docker Hub，一些其他的例子有 GitHub 仓库 ghcr.io 、红帽的 quay.io 仓库，很多公有云如阿里云也有自己的镜像仓库，或者您也可以考虑搭建自己的镜像仓库
1. library 是用户或机构的名称，例如：library 、donaldsebleung 、tetratelabs
1. hello-world 是镜像的名称，例如：hello-world 、 nginx 、busybox
- TAG 是镜像的版本，例如：v20.8-compatible 、latest 、22.04
- IMAGE ID 是镜像的哈希，可用于验证镜像下载后的完整
- CREATED 是镜像创建的时间
- SIZE 是镜像的总大小
我们也可以下载、删除、创建及上传镜像，其中下载及删除操作相对较简单，这里就先看这两个操作吧 ～
下载镜像
下载镜像可用 podman pull <镜像名称>:<镜像版本> 命令，例如下载 docker.io/library/nginx 镜像的 1.24 版本，命令如下：
```
podman pull docker.io/library/nginx:1.24
```
输出：
```
Trying to pull docker.io/library/nginx:1.24...
Getting image source signatures
Copying blob 2246bda193a8 done  
Copying blob ee7feb8b89d4 done  
Copying blob 4b5188c33a72 done  
Copying blob 9e3ea8720c6d done  
Copying blob 3726de4affbb done  
Copying blob 3c90f9dd7b85 done  
Copying config 8409c6410d done  
Writing manifest to image destination
Storing signatures
8409c6410d365fe098e23426b7617ad45637250df344cd91c4c2f1917f6cd85b
```
再查看本地的镜像，可以看到刚才的下载成功了：
```
podman images
REPOSITORY                     TAG               IMAGE ID      CREATED      SIZE
localhost/linuxdeepin/apricot  v20.8-compatible  6b03b24c2d82  12 days ago  3.3 GB
docker.io/library/hello-world  latest            9c7a54a9a43c  2 weeks ago  19.9 kB
docker.io/library/nginx        1.24              8409c6410d36  2 weeks ago  147 MB
```
# 删除镜像
删除镜像使用 podman image rm <镜像名称>:<镜像版本> 命令，让我们删除刚才的 hello-world 镜像吧：
```
podman image rm docker.io/library/hello-world:latest
Untagged: docker.io/library/hello-world:latest
Deleted: 9c7a54a9a43cca047013b82af109fe963fde787f63f9e016fdc3384500c2823d
```
再查看本地镜像，发现 hello-world 消失了，如我们预期一样：
```
podman images
REPOSITORY                     TAG               IMAGE ID      CREATED      SIZE
localhost/linuxdeepin/apricot  v20.8-compatible  6b03b24c2d82  12 days ago  3.3 GB
docker.io/library/nginx        1.24              8409c6410d36  2 weeks ago  147 MB
```
# 运行容器
学习如何创建、上传镜像前，让我们先学习如何运行容器吧 ～

运行容器基本方法使用 podman run <镜像名称>:<镜像版本> 命令，也建议添加 --name <容器名称> 选项给容器起个名字，不然 Podman 会自动给容器起个 奇怪 中规中矩的名字，例如创建一个名为 hello-podman 的容器，使用刚才的 docker.io/library/hello-world 镜像，latest 版本：
```
podman run --name hello-podman docker.io/library/hello-world:latest
```
输出还是刚才 Hello World 的一大堆输出

然后用 podman ps 命令查看正在运行的容器（这里 ps 是英文 processes 的缩写，指「进程」），返回结果为空：
```
CONTAINER ID  IMAGE       COMMAND     CREATED     STATUS      PORTS       NAMES
```
添加 -a 选项后能看到运行及已停止的容器：
```
podman ps -a
CONTAINER ID  IMAGE                                           COMMAND               CREATED        STATUS                    PORTS       NAMES
a0e067a9ead4  localhost/linuxdeepin/apricot:v20.8-compatible  -v --name donalds...  2 days ago     Created                               deepin-v20
4699b68a279b  docker.io/library/hello-world:latest            /hello                5 seconds ago  Exited (0) 5 seconds ago              hello-podman
```
这里最后一行输出是刚才名为 hello-podman 的容器，看到它状态 STATUS 为 Exited ，即它已经运行完毕，运行的命令 COMMAND 路径为 /hello

删掉容器使用 podman rm <容器名称> 命令：
```
podman rm hello-podman
```
# 运行 NGINX Web 服务器
刚才学习下载镜像时弄了个 NGINX 镜像，让我们基于这个镜像跑一个 Web 服务器吧 ～

跟刚才的 Hello World 程序不同，Web 服务器是长期后台运行的而且需要调用到网络

科普一下，Web 服务器一般为 HTTP 或 HTTPS 服务器，HTTP 服务的默认端口是 80，使用 TCP 协议

可是容器里的网络跟宿主的网络不同，而且宿主的网络打开 80/tcp 端口需要管理员权限，这样就抵消了 Podman 的安全性了

让我们创建一个名为 my-web-server 的 NGINX 容器，并添加以下选项：

- -d 选项让 Podman 在后台运行该容器，不要占用终端
- -p 8080:80 是端口映射，意思是打开宿主的 8080/tcp 端口（不需要管理员权限），然后把访问该端口的请求转发到容器里的 80/tcp 端口，则 HTTP 服务端口
```
podman run --name my-web-server -d -p 8080:80 docker.io/library/nginx:1.24
```
您可以用 podman ps 命令确认该容器正在运行，然后打开浏览器访问 http://localhost:8080/ 验证结果：

![2023-6-15_19635.png](/2023-6-15_19635.png)
验证后，用 podman stop <容器名称> 命令停掉服务器：
```
podman stop my-web-server
```
然后再移除该容器：
```
podman rm my-web-server
```
#创建并上传自己的容器镜像
既然运行容器需要用到镜像，那镜像是怎么来呢？让我们创建并上传自己的镜像以回答这个问题吧 ～

首先创建一个 hello-deepin23-podman 目录并 cd 进去：
```
mkdir -p hello-deepin23-podman/
cd hello-deepin23-podman/
```
然后在该目录创建个 index.html 文档并输入以下 HTML 代码：
```
<!DOCTYPE HTML>
<html>
  <head>
    <meta charset="utf-8" />
    <title>deepin 23 您好！ Podman 您好！</title>
  </head>
  <body>
    <h1>deepin 23 您好！</h1>
    <h2>Podman 您好！</h2>
  </body>
</html>
```
创建容器镜像主要有两个部分：

您的代码，例如刚才的 index.html
1.一个指示 Podman 如何创建镜像的 Dockerfile
2.让我们写个 Dockerfile 吧，稍后解释：
```
FROM docker.io/donaldsebleung/linuxdeepin-apricot:v20.8-compatible
WORKDIR /app
COPY index.html .
CMD ["python3", "-m", "http.server", "80"]
```
这个 Dockerfile 一共四行：

- FROM docker.io/donaldsebleung/linuxdeepin-apricot:v20.8-compatible 指示 Podman 从 docker.io/donaldsebleung/linuxdeepin-apricot 的 v20.8-compatible 版本底层镜像开始创建我们的新镜像 —— 容器镜像一般是一层层构建起来的，就像一颗洋葱
- WORKDIR /app 指示 Podman 往上加一层，并把新一层镜像的工作目录设置为 /app
- COPY index.html . 指示 Podman 再添加一层，并把 index.html 文档拷到镜像里的工作目录 . 下
- CMD ["python3", "-m", "http.server", "80"] 指示 Podman 往上再添加一层，并把基于该镜像创建的容器的默认命令设置为 python3 -m http.server 80
然后用 podman build -t <镜像名称>:<镜像版本> <项目根目录> 命令基于源代码及 Dockerfile 创建我们的镜像:
```
podman build -t hello-deepin23-podman:0.1.0 .
```
这里我们选择了给镜像一个简单的名称 hello-deepin23-podman ，版本 0.1.0 ，项目根目录是工作目录 . ；当镜像只供本地使用，不打算上传时，镜像名称不一定要严格遵循 <仓库>/<个人或机构名称>/<镜像名称> 格式

该镜像跟 NGINX 类似，两者都是后台长期运行的 Web 服务器，让我们运行这个服务器吧：
```
podman run --name my-hello-server -d -p 8080:80 hello-deepin23-podman:0.1.0
```
如同刚才 NGINX 的做法，我们打开浏览器造访 http://localhost:8080/ 看看：
![2023-6-15_30566.png](/2023-6-15_30566.png)
验证后把容器停掉、删除，我们接着学习如何把镜像上传到仓库里：
```
podman stop my-hello-server
podman rm my-hello-server
```
# 上传容器镜像
把镜像上传前，我们要选择上传到哪个仓库以及确认对该仓库有上传权限。由于 Docker Hub 是免费的，我们就把镜像上传到 Docker Hub 吧 ～

Docker Hub 的仓库地址是 docker.io ，要取得上传权限得先到 https://hub.docker.com/ 创建账号

假设您刚创建账号，账号名称是 student ，让我们把账号名称存到环境变量中吧，例如：
```
export DOCKERHUB_USERNAME="student"
```
以上把 student 换成您的 Docker Hub 用户名称

把自创建镜像上传到仓库前，镜像名称必须严格遵守 <仓库>/<个人或机构名称>/<镜像名称> 格式，用 podman tag <镜像旧名称>:<镜像旧版本> <镜像新名称>:<镜像新版本> 命令重新命名镜像（其实是给镜像起个新名称）：
```
podman tag hello-deepin23-podman:0.1.0 docker.io/"$DOCKERHUB_USERNAME"/hello-deepin23-podman:0.1.0

```
然后上传镜像前需要验证身份，用 podman login --username <用户名称> <仓库> 命令登录并输入密码：
```
podman login --username $DOCKERHUB_USERNAME docker.io
```
看到 Login Succeeded! 输出则代表登录成功

然后用 podman push <镜像名称>:<镜像版本> 命令把我们的镜像推到 Docker Hub 上：
```
podman push docker.io/"$DOCKERHUB_USERNAME"/hello-deepin23-podman:0.1.0
```
恭喜您！您成功把自己创建的镜像上传到 Docker Hub 分享出去！

最后记得用 podman logout <仓库> 命令注销 Docker Hub 账号：
```
podman logout docker.io
```
# 继续探索
刚才看到了，直接调用 Podman 客户端运行单一个容器也相当不简单。如果要运行更复杂的程序，例如一个依赖 MySQL 数据库的 Web 服务器，则您需要考虑以下的问题：

- 如何运行 Web 服务器？端口转发该怎么做？
- 如何运行 MySQL 数据库？数据库要跟 Web 服务器共享一个容器吗（答案：千万不要！），还是分开运行在另一个容器上？
- 如果数据库跟 Web 服务器分开两个容器运行，则如何让 Web 服务器安全访问数据库？
- 当数据库的容器因为异常等不可控因素终止运行，如何确保库里的数据能恢复？
- 当访问 Web 服务器的流量上来了，如何确保足够算力与资源支撑该服务？
因此在生产环境中，光一个容器引擎是远远不足的 —— 我们需要一个更强大的容器编排工具，以高度弹性、可靠、自动化的方式实时管理大量的容器

目前在容器编排工具市场中占绝对主导地位的是 Kubernetes ，俗称 K8s，也有其他的容器编排工具：

- Docker Swarm：适合个人及开发者部署小型的服务与测试环境
 - Apache Mesos：可用于生产环境，但如今基本上没戏了
若欲深入了解容器技术，进军互联网行业，那培训考取 K8s 认证是必须的，详情就不多说了，有兴趣可自行探索：https://kubernetes.io/training/

