---
title: docker-ce代理
description: 
published: true
date: 2023-07-19T06:39:57.139Z
tags: 
editor: markdown
dateCreated: 2023-07-18T12:46:39.753Z
---

# dockerd代理
在Docker中，`dockerd`是Docker守护进程，负责管理和运行容器。当我们使用`docker pull`命令从Docker镜像仓库中拉取镜像时，实际上是由`dockerd`进程执行的。

要为`dockerd`设置代理，我们需要进行以下步骤：

1. 创建代理配置文件夹：首先，我们需要创建一个名为`/etc/systemd/system/docker.service.d`的文件夹，用于存放Docker服务的配置文件(`.conf`)。

```
sudo mkdir -p /etc/systemd/system/docker.service.d
```

2. 创建代理配置文件：然后，我们需要在上述文件夹中创建一个名为`proxy.conf`的文件，用于配置代理。

```
sudo touch /etc/systemd/system/docker.service.d/proxy.conf
```

3. 编辑代理配置文件：使用文本编辑器（如`nano`或`vi`），打开`proxy.conf`文件，并添加以下内容：

```
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:7890"
Environment="HTTPS_PROXY=http://127.0.0.1:7890"
Environment="NO_PROXY=localhost,127.0.0.1"
```

在上述配置中，我们设置了HTTP和HTTPS代理的地址为`http://127.0.0.1:7890`，并指定了不需要代理的地址为`localhost`和`127.0.0.1`。

4. 生效配置更改：保存并关闭文件后，我们需要重新加载`systemd`守护进程的配置，并重启Docker服务，以使代理配置生效。

```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

5. 要查看`dockerd`的环境变量配置，您可以使用以下命令：

```
sudo systemctl show --property=Environment docker
```

这将显示`dockerd`的环境变量配置，包括代理相关的配置。

现在，`dockerd`将使用指定的代理进行网络请求，包括从Docker镜像仓库中拉取镜像。

# container代理
在容器运行阶段，我们可以在用户级别为容器设置代理。这将影响到在容器内部运行的命令，以及`docker login`等。也可以直接在其运行时通过`-e`注入`http_proxy`等环境变量。

要为容器设置代理，我们需要进行以下步骤：

1. 编辑Docker配置文件：使用文本编辑器，打开`~/.docker/config.json`文件。

```
nano ~/.docker/config.json
```

2. 添加代理配置：在配置文件中，我们需要添加一个`proxies`字段，并在其中指定代理配置。

```
{
 "proxies": {
   "default": {
     "httpProxy": "http://127.0.0.1:7890",
     "httpsProxy": "http://127.0.0.1:7890",
     "noProxy": "127.0.0.0/8"
   }
 }
}
```

在上述配置中，我们设置了HTTP和HTTPS代理的地址为`http://127.0.0.1:7890`，并指定了不需要代理的地址为`127.0.0.0/8`。

3. 保存并关闭文件。

4. 要在容器中查看代理环境变量，您可以使用以下命令：

```
sudo docker run --rm alpine sh -c 'env | grep -i _PROXY'
```

这将在一个临时的Alpine容器中运行命令，并显示与代理相关的环境变量。

现在，在容器中运行的命令将使用指定的代理进行网络请求。

# docker build代理
在构建Docker镜像时，我们也可以设置代理。这对于在构建过程中需要下载依赖包或其他资源的情况非常有用。

要为`docker build`命令设置代理，我们可以使用`--build-arg`参数传递代理配置。以下是示例命令：

```
docker build . 
    --build-arg "HTTP_PROXY=http://127.0.0.1:7890" 
    --build-arg "HTTPS_PROXY=http://127.0.0.1:7890" 
    --build-arg "NO_PROXY=localhost,127.0.0.1" 
    -t your/image:tag
```

在上述命令中，我们使用`--build-arg`参数分别设置了HTTP代理、HTTPS代理和不需要代理的地址。这将在构建过程中生效，并且只对该次构建有效。

请注意，上述示例中的代理地址和端口号是示例，您需要根据实际情况进行相应的更改。

通过上述方法，您可以在Docker中设置代理，以便在网络请求中使用代理服务器。这对于在受限网络环境下使用Docker非常有用，例如在公司内部网络或代理服务器后使用Docker。

# 参考
https://note.qidong.name/2020/05/docker-proxy/
https://docs.docker.com/config/daemon/systemd/
https://docs.docker.com/network/proxy/