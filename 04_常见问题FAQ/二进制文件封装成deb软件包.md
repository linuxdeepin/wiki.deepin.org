---
title: 二进制文件封装成deb软件包
description: 
published: true
date: 2023-08-24T07:51:11.557Z
tags: 
editor: markdown
dateCreated: 2023-08-24T07:31:20.156Z
---

  要将一个二进制文件制作成可以通过 apt install 安装的资源，你需要创建一个 Debian 软件包。Debian 软件包是一种标准的软件分发格式，它包含了软件的元数据、安装脚本以及文件等。

以下是制作 Debian 软件包的基本步骤：

## 准备工作环境：
在你的开发环境中，确保已经安装了 dpkg-dev 包，它包含了创建和管理 Debian 软件包所需的工具。
```
sudo apt-get update
sudo apt-get install dpkg-dev
```
## 创建软件包目录结构：
在你的工作目录中，创建一个新的目录，用于组织软件包的内容。
```
mkdir mypackage
cd mypackage
```
## 创建软件包的控制文件：
在软件包目录中，创建一个名为 DEBIAN 的子目录，用于存放软件包的控制文件。在 DEBIAN 目录中，创建一个名为 control 的文件，它包含了软件包的元数据，如软件名称、版本、描述等。

示例 control 文件内容：
```
Package: mypackage
Version: 1.0
Architecture: amd64
Maintainer: Your Name <your@email.com>
Description: A description of your package
```
## 将二进制文件添加到软件包：
将你的二进制文件复制到软件包目录中，通常放在 /usr/bin （即 软件包子目录 mkdir -p ./usr/bin）或类似目录（比如 /usr/local/bin）下。确保你在 control 文件中正确指定了适当的架构。

## 制定安装脚本（可选）：
如果你的软件包需要在安装或卸载时执行特定的操作，你可以在 DEBIAN 目录中创建 postinst（安装后脚本）和 postrm（卸载后脚本）等文件。

## 设置文件权限和所有权：
根据需要，为软件包中的文件设置适当的权限和所有权。

## 构建软件包：
在软件包目录的上层目录，使用以下命令构建软件包：
```
sudo dpkg-deb --build mypackage
```
这将在同级目录下生成一个 .deb 文件，这就是你的 Debian 软件包。

## 安装和测试：
你可以使用 dpkg 命令安装你的软件包进行测试：
```
sudo dpkg -i mypackage.deb
```
然后，你可以运行你的二进制文件，确保它能正常工作。

## 上传到仓库（可选）：
如果你希望其他人也能通过 apt 安装你的软件包，你可以将软件包上传到一个 APT 软件仓库。这需要更多的步骤，包括设置一个仓库服务器和发布软件包。

需要注意的是，创建一个完整的 Debian 软件包可能涉及更多的细节和配置。上述步骤只是一个基本指南。如果你计划将软件包发布到公共仓库，还需要了解如何签名和管理你的软件包以确保安全性。

比如：

先制作一个二进制文件
```
cat > helloworld.c
```
```
#include <stdio.h>

int main() {
    printf("Hello, World!");
    return 0;
}

```
```
按 ctrl + c （注意，要有换行。若输入的内容无换行，需要再按“Enter”回车换行）

#  编译成二进制文件
gcc helloworld.c -o helloworld
# 运行二进制文件
./helloworld
# 显示
Hello, World!
```
将二进制文件复制到 mypackage/usr/bin/ 目录下。
```
cp ./helloworld ./mypackage/usr/bin/mypackage

mypackage tree

├── DEBIAN
│   └── control
└── usr
    └── bin
        └── mypackage

4 directories, 2 files
```
```
/# 构建
dpkg-deb --build mypackage
# 安装
sudo dpkg -i mypackage.deb
# 将会将 ./mypackage/usr/bin/mypackage 文件复制到系统目录 /usr/bin/mypackage 中
which mypackage
/usr/bin/mypackage
```
若想通过 apt install 可安装的方式，需要制作一个 apt 软件仓库。

---感谢坛用户jetsung分享.