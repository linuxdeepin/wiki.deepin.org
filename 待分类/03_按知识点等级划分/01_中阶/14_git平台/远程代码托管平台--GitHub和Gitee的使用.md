---
title: 远程代码托管平台--GitHub和Gitee的使用
description: 
published: true
date: 2022-10-25T02:30:50.417Z
tags: github gitee
editor: markdown
dateCreated: 2022-07-11T02:20:06.766Z
---

# 远程代码托管平台--GitHub和Gitee的使用

## 一、团队协作与代码托管中心
本文章需要阅读者有Git基础，如果不知道Git是什么或者不知道Git的基本操作的小伙伴可以先看一看我上一篇文章： Git 的介绍、安装及其基本操作

### 1、代码托管中心
在上一节中我们学习了目前全球最流行的分布式版本控制工具 – Git的产生、安装以及基本使用，了解了如何通过Git进行版本控制，但是我们可以发现，在上一节中我们所有的操作都是在本地进行的（由工作区添加到暂存区，由暂存区提交到本地库），但是我们知道，在公司内部，一个项目的开发是由一个团队协作完成的，这种协作包括团队内协作和跨团队协作，那么如何实现团队协作呢？事实上，实现团队协作需要用到代码托管中心。

### 2、利用代码托管中心实现团队内协作
假设一个项目由A和B两个人组成的一个团队来共同开发，现在A实现了项目的基本框架，并把相关代码文件提交到了本地库，他希望B来完善该项目的细节，那么整个过程一共可分为四步：

A把本地库中的代码push到代码托管中心的远程库中；
B把远程库中A推送的代码完整的clone到自己的本地库中；
B对本地库中的代码进行修改完善，在获得A的授权后(即A把B添加进自己的团队)把本地库中的代码push到远程库中；
A pull远程库中B push的代码，来对A的本地库进行更新。

![image-20220628215258281](https://img-blog.csdnimg.cn/img_convert/317a47b7deb9b92887fdea95dfab4865.png)



### 3、利用代码托管中心实现跨团队协作
假设现在有A和B两个团队合作开发一个项目，A团队负责项目的function1，B团队负责项目的function2，并且两个功能之间是相互关联交叉的，那么A团队和B团队协作的过程分为五步：

B团队把A团队远程库中的代码fork到自己的远程库中；
B团队把自己远程库中的代码clone到自己的本地库中；
B团队把本地库中的代码修改完善后，push到自己的远程库中；
B团队向A团队发送一个pull request；
A团队经过审核后merge该分支，从而更新自己的远程库，再把远程库中的代码pull到本地库中进行其他操作。

![image-20220628223043729](https://img-blog.csdnimg.cn/img_convert/62053cdb26b8f5a7ef1fafe1e4481853.png)



### 4、常见的代码托管平台
代码托管平台有很多，其中最常用、最流行的是GitHub、Gitee和Gitlab：

GitHub：一个基于Git的面向开源及私有软件项目的托管平台，是全球最大的同性交友网站，技术宅男的天堂。
Gitee：是基于Git的代码托管和研发协作平台，被称为国内版的GitHub。
Gitlab：一个用于仓库管理系统的开源项目，使用Git作为代码管理工具，并在此基础上搭建起来的Web服务，一般用来搭建公司内部私有的代码托管中心。（由于GItlab的安装需要购买云服务器，所以本文章不讲）
## 二、GitHub的使用
### 1、国内无法访问GitHub问题的解决
由于GitHub是国外的，所以在国内访问GitHub会出现速度慢以及连接失败的问题，我们可以选择Gitee来代替GitHub，但是由于使用GitHub的人数要远多于Gitee，且GitHub上的开源项目十分丰富，所以我们还是有必要掌握一些访问GitHub的方法，此问题的解决方法分为以下几种：

第一：使用梯子翻墙 ；缺点：1、翻墙使用网络网速较慢 2、每次访问GitHub都需要翻墙，比较麻烦 3、在国内翻墙违法；所以我不建议大家为了访问GitHub而去翻墙。
第二：使用各种游戏加速器来加速GitHub，如UU、Steam++；缺点：访问不稳定。
第三：使用镜像站。
第四：通过本地代理直连加速，绕过SNI干扰。
第五：出国。
这里我为大家提供第四种解决方式的具体操作方法，这也是我本人一直在使用的；这种方法是我在逛Greasy Fork的时候无意间发现的，当时我是在浏览GitHub增强的脚本（脚本地址），然后看到作者在对该脚本的介绍中提供了访问GitHub的方法，其中介绍具体解决办法的文章如下：docmirror/dev-sideca（注意：由于这篇文章是在GitHub里面的，所以第一次可能会访问失败，多访问几次或者等几分钟再访问即可），请需要使用此方法的小伙伴仔细阅读该文章，特别是注意文章中提到的软件在打开状态中关闭电脑重启时会出现电脑无法正常上网的问题，虽然解决办法非常简单，但是如果不注意这个点的话可能让自己以为电脑出现了问题；另外由于在GitHub中下载该软件非常慢，速度只有几十KB，所以我把该软件放到了百度网盘中，有需要的自取。

百度网盘链接：https://pan.baidu.com/s/1edtD4fw5uPnOC2bS6OoZtA 
提取码：yzpq

### 2、在GitHub上创建远程仓库

#### 2.1 登录/注册GitHub账号

GitHub网址：[GitHub: Where the world builds software · GitHub](https://github.com/)

![image-20220629231159503](https://img-blog.csdnimg.cn/img_convert/f279d1b1a9f8582d8047062ef3d33d1a.png)

#### 2.2 创建远程仓库

进入GitHub后点击右上角的+号，然后选择New repository

![image-20220629231528477](https://img-blog.csdnimg.cn/img_convert/195dacbe7e722b1c1e6a933a461b178c.png)



填写仓库名称，仓库性质建议设置为公开

![image-20220629232334528](https://img-blog.csdnimg.cn/img_convert/af0a6a52da1e8a94b5eb93ced703cb2f.png)

## 3、为远程仓库创建别名

在远程库创建成功后，我们把远程仓库的HTTPS地址复制下来，然后在Git管理文件中打开Git，使用 “git remote add 别名 仓库地址” 命令来创建仓库的别名，别名创建成功后，我们可以通过 “git remote -v” 来查看别名。(注意：粘贴远程库网址的时候不能用Ctrl+V，因为Git的指令与Linux是一样的)


![image-20220629233100391](https://img-blog.csdnimg.cn/img_convert/91d2707c4427de866bf246b9517b47b9.png)

![image-20220629233500668](https://img-blog.csdnimg.cn/img_convert/b47d405e0d9cd0a6358db51b5d4a2703.png)

![image-20220629233655852](https://img-blog.csdnimg.cn/img_convert/6e3c0f8179fd1d5d14bf3b60e9831b51.png)

## 4、推送本地库代码到远程库

在上一节关于Git的介绍中我们已经把测试代码git_test添加至暂存区、提交到本地库了，现在我们只需要通过 “git push 仓库别名 分支名” 命令来把本地库中的代码推送到GitHub中的建立的远程库中。

![image-20220629235124833](https://img-blog.csdnimg.cn/img_convert/b00eb48bae8edbc9055209502786407c.png)

我们输入命令后它弹出提示，让我们登录GitHub账号，点击第一个登录即可。



![image-20220629235337670](https://img-blog.csdnimg.cn/img_convert/41e94adb2cb79b2729eff519c8ccef64.png)



登录账号之后，显示推送成功。

![image-20220629235819320](https://img-blog.csdnimg.cn/img_convert/79cbe2b291d53bec89fd6b7d3075beb2.png)

推送成功之后，我们刷新GitHub仓库，就会发现我们推送的git_test代码。

![image-20220630000043771](https://img-blog.csdnimg.cn/img_convert/3c1c41029410a3f525b2582744e1de60.png)

![image-20220630000057672](https://img-blog.csdnimg.cn/img_convert/657b1d6f4c908eee910623d28b572946.png)

## 5、拉取远程库到本地库

我们可以在家中登录GitHub修改我们远程库里面的代码，回到公司之后，再拉取远程库中的代码来更新本地库，从而实现随时随地办公。

![image-20220630101351707](https://img-blog.csdnimg.cn/img_convert/cd365814727f2faa98071ac4f5a6ce9b.png)

![image-20220630101625811](https://img-blog.csdnimg.cn/img_convert/16dcdebfb07d1af08aac404900f9ea3a.png)

![image-20220630101706195](https://img-blog.csdnimg.cn/img_convert/8a32af0657ca1fa86734b54fae5c582b.png)

远程库修改完毕后，我们就可以通过拉取操作来更新公司电脑的本地库代码，拉取命令和推送命令格式一样：“git pull 仓库别名 分支名”

![image-20220630102157518](https://img-blog.csdnimg.cn/img_convert/2aabcf2d2079624fbcf257a2f37c98a4.png)

## 6、克隆远程库到本地库

我们可以通过克隆操作克隆GitHub上公开仓库中的代码，这里我以在GitHub随便搜的一个五子棋游戏代码为例：我们搜索五子棋游戏，然后随便点击一个，复制该代码HTTPS链接，然后在Git里面使用 “git clone 链接” 操作来克隆代码。![img](https://img-blog.csdnimg.cn/img_convert/9ff934d0f0fc2d0fea9507a9f3e961a4.png)

![image-20220630103516689](https://img-blog.csdnimg.cn/img_convert/ae3711ffee553465465387e0f65e9c90.png)

![image-20220630103611669](https://img-blog.csdnimg.cn/img_convert/7708d5399a6fc05f0c405d303b7de95e.png)

clone会进行如下操作：1、拉取代码 2、初始化本地库 3、创建别名（origin）

![image-20220630104333240](https://img-blog.csdnimg.cn/img_convert/e277a507d73ab736dc04860ba3f24d30.png)

## 7、SSH免密登录

首先，我们要创建SSH公钥：

![image-20220630105856820](https://img-blog.csdnimg.cn/img_convert/fb75d86c2015766ac0736441f078d770.png)

我们在Windows家目录下打开GIt Bush Here ，输入 “ssh-keygen -t rsa -C 邮箱” 来生成.ssh秘钥目录，其中rsa是一种非对称加密协议，-C(大写)是用来指定账号；输入之后，连续敲击三次回车即可；执行成功后就会在家目录下生成一个.ssh文件。



![image-20220630111024853](https://img-blog.csdnimg.cn/img_convert/daf8ab44bcba420261943204c7dabfd0.png)

然后我们打开id_rsa.pub文件(即公钥文件)，复制里面的内容，在我们GitHub账号的设置里面找到SSH and GPG keys进行添加。

![image-20220630111932374](https://img-blog.csdnimg.cn/img_convert/45c457150864bc5c4dc202f4363349dd.png)

![image-20220630112010566](https://img-blog.csdnimg.cn/img_convert/575f6c8c1c0724a224dbaf277accf769.png)

![image-20220630112058517](https://img-blog.csdnimg.cn/img_convert/a3ebc367bc2b5a8716752a418f343055.png)

![image-20220630112138254](https://img-blog.csdnimg.cn/img_convert/dbf326f22fb0b5e9459c9896116bb2db.png)

![image-20220630112304130](https://img-blog.csdnimg.cn/img_convert/142c390a26ab4c3bba4a3203b3e3fafb.png)

![image-20220630112334662](https://img-blog.csdnimg.cn/img_convert/f03928838505ddafbbf010cdbf7884b4.png)

利用SSH来拉取以及推送代码时不必每次都输入密码，可以大幅提高工作效率；但是如果要测试我自己账号的SSH免密登录是否有效的话，需要另外一个账号，所以我这里就不在进行演示了，大家只需要在pull以及push的时候把HTTPS的地址换成SSH的地址即可。

三、Gitee的使用
1、Gitee介绍
众所周知,GitHub服务器在国外,使用GitHub作为项目托管网站,如果网速不好的话，严重影响使用体验，甚至会出现登录不上的情况。针对这个情况，大家也可以使用国内的项目托管网站-码云。

码云是开源中国推出的基于Git的代码托管服务中心，网址是 ，使用方式和GitHub一样，而且它还是一个中文网站，如果你英文不是很好它是最好的选择。

点击进入Gitee官网：Gitee - 基于 Git 的代码托管和研发协作平台，然后登录或者注册账号。

![image-20220630123056711](https://img-blog.csdnimg.cn/img_convert/eb434485ee2a3c62efc5ab6dc5664b84.png)



## 2、在码云上创建远程仓库

**由于码云完全是仿照GitHub开发的，使用起来与GitHub几乎没有区别，使用后面的系列操作我就不再进行文字描述了，而是直接上流程图。**

![image-20220630123600485](https://img-blog.csdnimg.cn/img_convert/5d27d75642b5e92a9f4954b1659e8342.png)

![image-20220630123728670](https://img-blog.csdnimg.cn/img_convert/71060b483754ed5ce0d3ed5c2eac7b05.png)

这里要特别注意：现在码云经过更新后，不能直接把仓库设置为公开，需要先设置为私有，只有等我们往仓库推送一定数量代码后，才能申请仓库公开。

![img](https://img-blog.csdnimg.cn/img_convert/38fbc1b98cc86aace73e95d4e8d508b8.png)

![image-20220630124732732](https://img-blog.csdnimg.cn/img_convert/105c2258f1ede7ac7b43c19958f491e3.png)

## 4、推送本地库代码到远程库

![img](https://img-blog.csdnimg.cn/img_convert/367d4bcf790de3d179b2aead4fd55026.png)

![img](https://img-blog.csdnimg.cn/img_convert/c838416cee2b9e776c04a11335fb650c.png)

![image-20220630130427040](https://img-blog.csdnimg.cn/img_convert/a09ab5635b358cc6a66ac9777867f055.png)



拉取远程库到本地库以及克隆远程库到本地库和GitHub操作一样，这里我就不再演示。

## 5、SSH免密登录

由于在刚才GitHub的演示中我已经生成了.ssh文件，所以这里我就直接用id_rsa.pub在Gitee的设置里面添加SSH公钥

![image-20220630131126313](https://img-blog.csdnimg.cn/img_convert/23fa7953054ee1334f3b8eba6adbd7e1.png)



![image-20220630131225031](https://img-blog.csdnimg.cn/img_convert/fb365446b77f01c8268bf02cecea76b6.png)

![image-20220630131301635](https://img-blog.csdnimg.cn/img_convert/db3553aa65f618fd24090c3003c7a47d.png)

![image-20220630131325653](https://img-blog.csdnimg.cn/img_convert/e9634b6499d4caac7101192eebc7fc36.png)

## 6、Gitee导入GitHub仓库

为了避免GitHub登录不上使得项目无法查看的情况，码云提供了直接把GitHub仓库复制迁移到Gitee的方法，这里我们以刚才在GitHub中创建的daily-study仓库为例：我们点击新建仓库，点击导入已有仓库，然后输入GitHub仓库的HTTPS网址即可。我们还可以通过点击小圆圈刷新来获取GitHub远程库中的最新状态来同步仓库。




## ![image-20220630132508386](https://img-blog.csdnimg.cn/img_convert/c5f8d35a985d770e8b0e701ce690890f.png)



![image-20220630132550099](https://img-blog.csdnimg.cn/img_convert/24ffe0fb0b8d56dca28a8fe95cc7323a.png)



![image-20220630132733352](https://img-blog.csdnimg.cn/img_convert/034c81d7efa7c015d6b4dbd0fe1028f2.png)



![image-20220630132808320](https://img-blog.csdnimg.cn/img_convert/fdd1893241a373d3a50e11b3d5d0fbec.png)

我们还可以通过点击小圆圈刷新来获取GitHub远程库中的最新状态来同步仓库。

![image-20220630132855171](https://img-blog.csdnimg.cn/img_convert/56e9f6b99ca7b51dd766a623386218e1.png)