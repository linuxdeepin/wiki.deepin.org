---
title: 使用action创建新的git仓库
description: 
published: true
date: 2022-10-25T02:55:17.704Z
tags: 
editor: markdown
dateCreated: 2022-06-22T08:51:47.006Z
---

# 使用 action 创建新的 git 仓库
> ps: 当前自动化构建是基于commit进行触发，每当有一个commit合入则触发action的自动构建并导入仓库中，请注意不要随意在正式环境中使用下面的仓库，仓库的软件包大多没有经过测试验证，我们无法保证仓库的软件包的稳定性。
{.is-warning}

## archlinux

### 添加仓库

在 `/etc/pacman.conf` 中添加以下内容：

```
[deepin]
SigLevel = Never
Server = https://deepin-community.github.io/arch-dde-repo
```

所有构建的包，均属于 `deepin-git` group，可通过安装该 group 所有软件包替换 `deepin` group 的所有软件包。

### 问题修复

有时候项目会需要增加依赖，或者减少依赖，导致 archlinux 平台无法完成构建，那么需要在 [https://packages.archlinux.org/](https://packages.archlinux.org/) 查询相关软件包，并添加到 archlinux/PKGBUILD 文件的 makedepends 中，如果是运行时依赖，还需要同时添加到 depends 中。

### 本地验证

本地验证可以保存以下脚本，在项目的根目录运行，即可完成一次打包测试。

```bash
#!/bin/bash

rm -rf $(basename $PWD)-git
mkdir $(basename $PWD)-git
rsync -a . $(basename $PWD)-git --exclude "$(basename $PWD)-git"
tar -czf archlinux/source.tar.gz $(basename $PWD)-git
version=1.0.0
sed -i "/pkgver=/cpkgver=${version}" archlinux/PKGBUILD
echo "deepin_source_name=$(basename $PWD)-git" >> archlinux/PKGBUILD
cd archlinux
deepin-x86_64-build
```

使用该脚本，需要先添加 `deepin-git` 仓库，安装 `devtools-deepin-git` 软件包。

## debian sid

### 添加仓库

在 `/etc/apt/sources.list.d/deepin-git.list` 中添加以下内容:

```
deb [trusted=yes arch=amd64] https://deepin-community.github.io/debian-sid-dde-deps-repo sid main
deb [trusted=yes arch=amd64] https://deepin-community.github.io/debian-sid-dde-repo sid main
```

debian sid 提供了两个仓库，理论上所有发行版仓库都会提供两个仓库，一个是保存 deepin 构建的软件包，另一个是提供官方仓库的补充。

在 debian sid 仓库中，只要添加该仓库，所有 deepin 的软件包均会升级到最新版本。

### 本地验证

想要在本地验证软件包在 debian 上的构建情况，需要安装 devscripts 和 pbuilder 软件包。

使用以下命令构建 debian sid base 镜像。

```bash
sudo pbuilder create --architecture amd64 \
                     --mirror "https://mirrors.ustc.edu.cn/debian" \
                     --distribution sid \
                     --basetgz ~/sid-base-amd64.tgz \
                     --allow-untrusted \
                     --debootstrapopts --include=ca-certificates
```

保存以下脚本内容：

```bash
#!/bin/bash

mkdir /tmp/hooks
cat << EOF > /tmp/hooks/D50Update
echo "" > /etc/apt/sources.list
echo "deb [trusted=yes] https://deepin-community.github.io/debian-sid-dde-deps-repo sid main" >> /etc/apt/sources.list
echo "deb [trusted=yes] https://deepin-community.github.io/debian-sid-dde-repo sid main" >> /etc/apt/sources.list
echo "deb https://mirrors.ustc.edu.cn/debian sid main contrib non-free" >> /etc/apt/sources.list
apt-get update || true
EOF

chmod +x /tmp/hooks/D50Update

PROJECT_NAME=$(dpkg-parsechangelog -S Source)
tag=$(git describe --tags | cut -f1 -d '-') || echo
num=$(git describe --tags | cut -f2 -d '-') || echo
if [ "$num" = "$tag" ]; then num=$(git rev-list --all --count); fi
num=`echo $num | awk '{printf("%03d",$0)}'`
version=${tag:-0.0.0}+r${num}+g$(git rev-parse --short HEAD)
new_dir=${PROJECT_NAME}-$version
mkdir $new_dir
rsync -a . $new_dir --exclude $new_dir
rm -rf $new_dir/.git
cd $new_dir
	rm -rf $new_dir
  dch -M -bv "${version}-1" "update"
  if [ -f ./debian/source/format ]; then sed -i 's/native/quilt/g' './debian/source/format'; fi
  dh_make --createorig -sy || true
  dpkg-source -b ./
cd ..
sudo DEB_BUILD_OPTIONS=nocheck pbuilder --build \
                            --basetgz ~/sid-base-amd64.tgz \
                            --allow-untrusted \
                            --hookdir /tmp/hooks \
                            --use-network yes \
                            --logfile `uname -m`-build.log \
                            --aptcache "" \
                            --buildresult . ./*.dsc
```

在项目的根目录执行脚本，就可以完整本地的打包验证。

## deepin

### 添加仓库

在 `/etc/apt/sources.list.d/deepin-git.list` 中添加以下内容:

```
deb [trusted=yes arch=amd64] https://deepin-community.github.io/deepin-dde-repo sid main
deb [trusted=yes arch=amd64] https://deepin-community.github.io/deepin-dde-deps-repo apricot main
```

在 deepin 上使用该仓库，需要注意 **不要** 同时使用内测仓库，混用不同的仓库可能会导致依赖关系解析错误，从而破坏系统。

### 本地验证

想要在本地验证软件包在 deepin 上的构建情况，需要安装 devscripts 和 pbuilder 软件包。

使用以下命令构建 deepin base 镜像。

```bash
sudo pbuilder create --architecture amd64 \
                     --mirror "https://mirrors.ustc.edu.cn/deepin" \
                     --distribution apricot \
                     --basetgz ~/apricot-base-amd64.tgz \
                     --allow-untrusted \
                     --debootstrapopts --keyring=/usr/share/keyrings/deepin-archive-camel-keyring.gpg \
                     --debootstrapopts --include=deepin-keyring,ca-certificates
```

保存以下脚本内容：

```bash
#!/bin/bash

mkdir /tmp/hooks
cat << EOF > /tmp/hooks/D50Update
echo "" > /etc/apt/sources.list
echo "deb [trusted=yes] https://deepin-community.github.io/deepin-dde-repo apricot main" >> /etc/apt/sources.list
echo "deb [trusted=yes] https://deepin-community.github.io/deepin-dde-deps-repo apricot main" >> /etc/apt/sources.list
echo "deb [trusted=yes] https://community-packages.deepin.com/deepin/ apricot main contrib non-free" >> /etc/apt/sources.list

apt-get update || true
EOF

chmod +x /tmp/hooks/D50Update

PROJECT_NAME=$(dpkg-parsechangelog -S Source)
tag=$(git describe --tags | cut -f1 -d '-') || echo
num=$(git describe --tags | cut -f2 -d '-') || echo
if [ "$num" = "$tag" ]; then num=$(git rev-list --all --count); fi
num=`echo $num | awk '{printf("%03d",$0)}'`
version=${tag:-0.0.0}+r${num}+g$(git rev-parse --short HEAD)
new_dir=${PROJECT_NAME}-$version
mkdir $new_dir
rsync -a . $new_dir --exclude $new_dir
rm -rf $new_dir/.git
cd $new_dir
	rm -rf $new_dir
  dch -M -bv "${version}-1" "update"
  if [ -f ./debian/source/format ]; then sed -i 's/native/quilt/g' './debian/source/format'; fi
  dh_make --createorig -sy || true
  dpkg-source -b ./
cd ..
sudo DEB_BUILD_OPTIONS=nocheck pbuilder --build \
                            --basetgz ~/apricot-base-amd64.tgz \
                            --allow-untrusted \
                            --hookdir /tmp/hooks \
                            --use-network yes \
                            --logfile `uname -m`-build.log \
                            --aptcache "" \
                            --buildresult . ./*.dsc
```

### 国内镜像站点
```
deb [trusted=yes arch=amd64] https://code.gitlink.org.cn/justforlxz/deepin-dde-repo/raw/branch/main apricot main
deb [trusted=yes arch=amd64] https://code.gitlink.org.cn/justforlxz/deepin-dde-deps-repo/raw/branch/main apricot main

deb [trusted=yes arch=amd64] https://code.gitlink.org.cn/justforlxz/debian-sid-dde-repo/raw/branch/main sid main
deb [trusted=yes arch=amd64] https://code.gitlink.org.cn/justforlxz/debian-sid-dde-deps-repo/raw/branch/main sid main

[deepin]
SigLevel = Never
Server = https://code.gitlink.org.cn/justforlxz/arch-dde-repo/raw/branch/main
```
