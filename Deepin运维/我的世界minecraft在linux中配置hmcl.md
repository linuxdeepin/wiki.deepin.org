---
title: 我的世界minecraft在linux中配置 hmcl
description: 
published: true
date: 2022-07-05T13:00:42.846Z
tags: minecraft hmcl
editor: markdown
dateCreated: 2022-07-05T13:00:40.546Z
---

# 我的世界 minecraft 在 linux 中配置 hmcl

## 前言

我的世界是一个广受欢迎的沙盒游戏，最重要的是，它基于java技术，因此也能正常运行在linux中。

不过在linux中，有一些需要注意的事项。 首先是官方 java 的授权问题，据说 java 8 还是有 bsd 授权的，但是为了避免著作权的纠纷，我们还是想办法是用 openjdk 比较好。 openjdk 是开源的java 运行时。对于我的世界本身而言，openjdk 不会存在太大的兼容性问题，但是如果你是使用一个叫 hmcl 的我的世界登陆器来玩游戏，它使用了一个叫 javafx 的图形界面框架，需要你配置第三方类库。

本文就会探讨这个问题。

## 配置 openjdk + javafx

首先，我的世界低版本会对java的版本有一定的显示，通常来说java 8 或者 1.8 是兼容性最好的。高版本的java我也不知道带来什么好处，暂时没有看到。如果你需要制作安装包，提供给别人使用，最好就是绑定 java 8 的运行时。

不过 javafx 和 java 分离之后，独立发展，现在长期支持的版本是 javafx 11，对javafx8 支持不太有好。而 hmcl 暂时还不支持openjdk11 以上。鉴于种种原因，我选择了 openjdk11 + javafx11 这样的组合。javafx 的开源名字是openjfx。

因此这里我就以 java11 为例子，制作一个捆绑了 javafx11 的整合版 java。

openjdk11 下载地址：<https://github.com/AdoptOpenJDK/openjdk11-binaries/releases>

页面比较复杂。下载一个新版本即可。注意几个关键词，x64,linux,openj9,jre。 jre是运行时，不带开发组件，对我们运行来说足够了，但是如果你想制作整合openjfx的捆绑java，就要下载sdk版本。 openj9 和 hotspot 是java虚拟机的技术，其中openj9比较省内存，因此可以选择这个。 hotspot是甲骨文官方支持的，也可以选择。 x64 和 linux 就是平台的版本。


openjfx11 下载地址：<https://gluonhq.com/products/javafx/>

注意几个关键词， `javafx linux sdk` 这个包含了.jar库，可以用命令行来调用运行。 `javafx linux jmods` 是一种编译时插件，可以用来制作捆绑 javafx 的新 java 运行时。为了方便起见，制作捆绑包在用户使用时会相对比较简单一些。

分离式运行 hcml：

这个需要 jre11 + jfx11 sdk

```bash
# 假设jdk11解压缩出来是jdk-11.0.8+2=jre
# 假设javafx11解压出来是 javafx-sdk-11.0.2
# 假设 hmcl 是 hmcl.jar
# 以下命令即可正常使用

./jdk-11.0.8+2-jre/bin/java -p javafx-sdk-11.0.2/lib --add-modules ALL-MODULE-PATH -jar hcml.jar
```

捆绑包运行 hcml：

这个需要 jdk11 + jfx11 jmods

```bash
# 假设 jdk11 解压出来是jdk-11
# 假设 jfx11 jmods 解压出来是 javafx-jmods-11.0.2
# 输出整合包到 jdk11jfx11 目录
./jdk-11/bin/jlink --module-path javafx-jmods-11.0.2/ --add-modules javafx.base,javafx.fxml,javafx.media,javafx.web,javafx.controls,javafx.graphics,javafx.swing,java.base,java.desktop,java.logging,java.management,java.sql,java.xml,jdk.unsupported --bind-services --output jdk11jfx11

# 运行 hcml
./jdk11jfx11/bin/java -jar hcml.jar
```

当前版本hcml 3.3.158 依赖的包：

1. java.base
1. java.desktop
1. java.logging
1. java.management
1. java.sql
1. java.xml
1. javafx.base
1. javafx.controls
1. javafx.fxml
1. javafx.graphics
1. javafx.swing
1. javafx.web
1. jdk.unsupported

怎么知道 hcml 依赖什么包？

```bash
# 使用jdk 的一个叫 jdeps 的工具可以检查
./jdk-13/bin/jdeps --list-deps --multi-release=11 *.jar
```

## 另一个简单的方案：

bellsoft 这个网站提供 java8 +javafx 的整合包下载：<https://bell-sw.com/pages/java-8u252/>

注意下载 full version，才是整合javafx的包。

zulu 网站也提供了：<https://www.azul.com/downloads/zulu-community/?version=java-11-lts&os=linux&architecture=x86-64-bit&package=jre-fx>


## 参考

1. openjdk8 下载地址： <https://github.com/AdoptOpenJDK/openjdk8-upstream-binaries/releases/>
2. Java 9：使用第三方jar生成JLink的运行时映像: <https://stackoom.com/question/3H7FW/Java-%E4%BD%BF%E7%94%A8%E7%AC%AC%E4%B8%89%E6%96%B9jar%E7%94%9F%E6%88%90JLink%E7%9A%84%E8%BF%90%E8%A1%8C%E6%97%B6%E6%98%A0%E5%83%8F>