---
title: git实践手册
description: 
published: true
date: 2022-10-25T02:15:03.281Z
tags: git
editor: markdown
dateCreated: 2022-06-28T08:25:43.395Z
---

# git 实践手册

## 前言

git是一个很好的版本管理工具，他也是一个很好同步工具。但是不可否认，具备一定的复杂性。

## 问题

1. 怎么忽略更改，切换到另一个版本

比如，你在网上下载git源代码，你实际上不关系什么版本，你只想用你要的那个版本。下载之后，你编译了，或者不小心做了一些无关要紧的更改。新版本出来之后，你想同步到新的版本，但是却发现你现在的版本已经被修改了，git 提示让你合并修改，而你根本没兴趣处理这个问题。

```bash
# 强制切换到源仓库上的v5.4.13 tag 版本
git reset --hard origin/v5.4.13
```

2. 只想下载某一个版本的源代码，而不想下载整个仓库

有时候你根本不想下载git整个仓库，因为会很大，你只想要最新的版本，或者某个发行版。

```bash
# 下载默认的最新版本
# --depth=1 只下载一个版本
git clone --depth=1 https://github.com/deepinwiki/wiki.wiki.git
```

```bash
# 查看远程仓库有哪些版本和分支
# 在ls-remote 后可添加选项
# --tags 版本
# --heads 分支
# 在链接后，可以设置匹配字符串，如
# v5.4.* 就只显示v5.4的所有小版本
git ls-remote https://github.com/deepinwiki/wiki.wiki.git
```

版本信息：

- HEAD ： 这个表示当前的工作版本
- refs/heads/master: 默认的主分支
- refs/heads/uefi: 自定义的其余分支
- refs/tags/v5.4.13: 自定义的tag，一般就是特定版本

```bash
# 下载指定版本
# --branch 指定版本，不一定要是分支，任意版本都行 
git clone --depth=1 --branch=vol.35 https://github.com/521xueweihan/HelloGitHub.git 

```

3. 跳转到其他远程版本

克隆仓库之后，又有新版本，或者想跳转到其他版本，而本地还没有的。你又不想重新克隆一个新仓库。

```bash
# 查看一下远程信息
# origin 远程仓库默认命名
git ls-remote origin

# 下载远程对象
# 这个命令会下载两个版本之间的所有版本
# 比如v5.4.14 是中间的版本也会下载
# 但问题不大，因为版本之间共享的对象是绝大多数的
git pull origin v5.4.15
# 下载相关对象，比如tag
git fetch
```

```bash
# 查看当前的版本历史
git log --oneline --graph --all
# 如输出：
* 0092579 (tag: vol.41) 发布：《HelloGitHub》第41期
* 7968f3c 更新共享协议
* 5bcb5e5 更新文章模版:上下页、反馈、目录
* f2bbf3d 发布：《HelloGitHub》第39期
| * 24d4518 (tag: vol.39) 发布：《HelloGitHub》第39期
|/  
* ecaab5c Update contributors.md
* b877a12 Update README.md
* 3b9edda Update README_en.md
*   c52216c Merge pull request #651 from RobiNexy/master
|\  
| * d772748 Create README_en.md
|/  
* 5d50b1c 更新内容模版
* 5277641 update readme
* 233b95d 发布：《HelloGitHub》第38期
* 3324060 Update README.md
* a3c6776 (HEAD, grafted, tag: vol.37, vol.37) 发布：《HelloGitHub》第37期
* a248acc (grafted, tag: vol.35, vol.35) 发布：《HelloGitHub》第35期

```

理解版本历史：

1. 从新到旧排序
2. 第一组数字和字母，如a248aac是版本的标识，用这个可以跳转版本
3. 第二组是说明
4. 括号内信息是有特殊意义的
   1. grafted ：标识当前的起点，因为当前本地仓库的版本是没有初始起点，而从中间开始的，所以这个起点叫“嫁接点”，有两个（a3c6676和a248acc）说明这两个没啥关联
   2. tag ：当前版本的tag别名，用tag也能跳转版本
   3. 黄绿色的vol.35 是branch分支名，同样也可以代表版本：a248acc
   4. HEAD： 当前版本的位置
5. |/  ：竖线和斜线绘画的是分支线

```bash
# 因此，跳转到目标版本即可
git checkout 0092579
# 或者
git checkout vol.41
```

不下载相关版本的方法：

```bash
git pull --depth=1 origin vol.41
git fetch
```

有时候会提示和当前版本不相干，然后会把同步的内容放到 `FETCH_HEAD` 这个指针里面。

```bash
git checkout FETCH_HEAD

# 任何时候，了解当前状态，查看git给你的建议都是好事
git status
```

## 版本跳转技巧

先要理解版本是啥意思。git 每一次提交commit，都会产生一个新的版本，用一串很长的签名标识，如cf6e59720ef259188ce19191789048e6704286d5，但一般只要截取其中一段git就能匹配到这个版本。

版本有多种别名，比如tag（标签），分支（关联某个版本），还有指针，比如HEAD、FETCH_HEAD、ORIG_HEAD等，他们都可以在一定意义上代表某个版本。

版本之间大部分内容都是相同的，git只会记录差异，因此跳转版本是很快捷的操作，提交新版本也是很高效的操作，这是git引以为傲的特性。

但是，因为要记录版本历史，那么必然有一些对象在新版本已经不存在，git还是会保留他的历史数据。这往往不是消费者关心的（作者可能会比较在乎）。

好消息是git支持分离历史，只保留一个或者多个版本。

对于创作者来说，提交一个正式版本之前，会有临时的版本，下面来总结一下他们之间的关系：

本地仓库（即某文件夹）内:

1. `git add` ：将新文件加入git版本管理，否则这个文件不被git控制。这一步很重要，你要选择那些文件被管理，那些不被管理。不被管理也是一种有用的选择，这样你新添加的文件和修改不会污染你的git仓库。
2. 当修改了被管理的文件，自然和当前版本不一致了，但这个差异并不会立即被保存起来，而需要正式提交。因此这个状态就是临时版本。

假如没有临时版本（或者叫工作版本），那么跳转版本是很简单很自然的，因为都在正式的版本历史中跳跃而已。但是因为有了工作版本，就说明了你修改了东西，就涉及合并内容的问题。如果你只是修改了最后的版本，那么还算简单的，如果你是修改了中间的版本，或者一开始的版本，或者毫无关联的版本，这要怎么融合到版本的历史记录中去，是一个复杂的课题。

总结，麻烦的根源就是工作版本。

```bash
# 将某个文件加入当前工作版本
git add xxx

# 从工作版本复原到历史版本
# HEAD 是指向当前历史版本的指针
# --hard 抛弃工作版本
# --soft 保留工作版本
# --mixed 不保留工作版本但保留文件(默认)
git reset --hard HEAD
```

默认情况，是会保留当前所做的修改，但是我们不想处理版本合并这种麻烦事，所以要抛弃临时的修改（反正也是意外修改的）。

之后就是普通的跳转操作即可，不会提示什么内容合并冲突之类的麻烦事。

```bash
# 查看历史版本
git log --oneline --graph --all
# 用checkout正常跳转到任意历史版本
git checkout bba08fc
```

## 子仓库

git 支持子仓库，也就是让另一个 git 仓库成为主仓库的一个子目录。

其中有两种方式：

1. submodule : 链接到主仓库
2. subtree : 拷贝到主仓库

## 排除特定文件

git 支持排除特定文件或目录。

方法：

1. .gitignore : 文件

## 技巧

1. 怎么一次性同步到多个仓库？

答： git 的主要作用之一就是同步到不同的仓库，比如gitee 和 github。但是默认的情况下， git branch 只能设置一个上游 remote 位置。不过， remote 可以设置多个 Push 地址，这很符合常理。

```bash
git remote add origin xxx.git # 原有的 origin remote 仓库
git remote set-url --add --push yyy.git # 添加一个新的 push 地址
git branch -u origin # 设置默认的上游地址 origin
git fetch # 同步远程信息
git push # 一次推送 xxx.git 和 yyy.git
```

## 参考

1. 如何将现有 git 仓库中的子目录分离为独立仓库并保留其提交历史: <https://printempw.github.io/splitting-a-subfolder-out-into-a-new-git-repository/>
2. Git 本地仓库和裸仓库: <https://segmentfault.com/a/1190000007686496>