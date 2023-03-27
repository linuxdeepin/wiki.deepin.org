---
title: deepin-community协作指南
description: 协作指南
published: true
date: 2023-03-27T03:12:15.627Z
tags: 开发者贡献
editor: markdown
dateCreated: 2022-11-09T11:47:20.254Z
---

一、**仓库创建申请**

deepin-community/SIG 项目下修改[repos.yml](https://github.com/deepin-community/SIG/blob/master/repos.yml)文件，以PR形式提交需创建的仓库，merge后会自动创建对应仓库，请联系具有deepin-community组织[owner](https://github.com/orgs/deepin-community/people?query=role%3Aowner)权限成员审核，创建的仓库会默认添加工作流，一次新增的仓库建议不超过10个。示例 [deepin-community/SIG/pull/68](https://github.com/deepin-community/SIG/pull/68/commits/5c8688b32c8e9ed71615bbb37279fd5c32482f97)

[repos.yml](https://github.com/deepin-community/SIG/blob/master/repos.yml) 文件字段定义：

repo： 仓库名称

group:  所属sig组

info： 项目描述




**二、代码提交**

1. 从deepin-comunity fork 项目到个人账户下
2. 从个人账户下clone项目到本地
3. 添加upstream相关信息
4. 提交PR
5. 等待review (请联系sig对应的管理员review代码)
   为方便开发集成等相关工作我们开发了topic仓库机制、Pull Request 机器人等工具，这于这些工具的使用可以参照wiki，[Pull Request 机器人命令列表](https://wiki.deepin.org/zh/02_%E6%8C%89%E8%BD%AF%E4%BB%B6%E5%8A%9F%E8%83%BD%E5%88%92%E5%88%86/02_%E5%BC%80%E5%8F%91%E4%BA%BA%E5%91%98%E5%B8%B8%E7%94%A8%E8%BD%AF%E4%BB%B6%E4%BB%8B%E7%BB%8D/01_%E7%BC%96%E7%A8%8B%E5%BC%80%E5%8F%91/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6/%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9/pull-request-bot-commands-list)   [topic仓库机制说明](https://wiki.deepin.org/zh/02_%E6%8C%89%E8%BD%AF%E4%BB%B6%E5%8A%9F%E8%83%BD%E5%88%92%E5%88%86/02_%E5%BC%80%E5%8F%91%E4%BA%BA%E5%91%98%E5%B8%B8%E7%94%A8%E8%BD%AF%E4%BB%B6%E4%BB%8B%E7%BB%8D/01_%E7%BC%96%E7%A8%8B%E5%BC%80%E5%8F%91/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6/%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9/topic%E4%BB%93%E5%BA%93%E6%9C%BA%E5%88%B6%E8%AF%B4%E6%98%8E)
6. commit构建的产物存放在develop仓库中
	`develop仓库地址：deb [trusted=yes] https://ci.deepin.com/repo/develop/beige/ develop main dde`

**三、 CI/CD**

1. backup-to-gitlab
   仓库同步工作流，同步至内网gitlab
---


~~2. call-build-deb (替换)
   软件包构建工作流，提交PR时触发包构建检查~~
2. obs
	.obs/workflows.yml 文件替换 build-deb。将推送文件内容类似 .obs/workflows.yml@deepin-community/template-repository。
  
该文件中定义了三种OBS工作流：
- 	1). test_build
- 		pr的检查构建，提交pr时触发该工作流，在OBS创建出新的Project，在该Project下进行构建。
- 	2). tag_build
- 		打tag后的Unstable构建，当新的tag创建后，在 deepin:Unstable:{xxx} 下创建对应 {repo}-{tag} 的 Package。
- 	3). commit_build
- 		commit合入后的commit仓库更新，pr合入后，更新 develop 仓库的 Package 的代码。

	推送工作流后的构建将由 OBS 接管，在 GitHub 当 pr 创建后，OBS工作流将被触发，成功触发 OBS 工作流后，OBS 将会创建类似下面的状态。
  ![2023-2-14_9914.png](/2023-2-14_9914.png)
  
  此时可在 OBS 查看构建进度和构建日志。等待构建结束时，对应仓库的构建结果将会返回到 pr 。点击 Details 可直接跳转到 OBS 界面查看构建详情。
  ![2023-2-14_38923.png](/2023-2-14_38923.png)
  三个状态从上到下分别为：
		1). OBS 工作流的状态。
		2). 工作流文件(.obs/workflows.yml)中定义的 deepin_develop 的 aarch64 架构构建。
		2). 工作流文件(.obs/workflows.yml)中定义的 deepin_develop 的 x86_64 架构构建。

		当上面所有的状态都返回到 GitHub 时说明构建没有问题，缺少任何一个都代表构建出现问题或未完成。
		构建日志大家可在 https://build.deepin.com 查询。在 All Projects 中找到 deepin:CI 开头加上自己 PR 信息的 Project。比如 deepin:CI:deepin-community:inltool-debian:PR-1，如果使用了 topic（feat: add topic · linuxdeepin/open-build-service@2702a73，由 wineee 提供该patch）将会是 deepin:CI:topic:{topic}，topic 触发机制与之前保持一致，并且使用相同 topic 的仓库都会在该 Project 下。
  1). All Projects 界面找到 deepin:CI:deepin-community:intltool-debian:PR-1
  ![2023-2-14_63282.png](/2023-2-14_63282.png)
  2). 点开 deepin:CI:deepin-community:intltool-debian:PR-1 后的界面，此处未使用 topic，所以只有 intltool-debian 一个 Package。如果使用topic，相同 topic 的 pr 会在同一个Project下，此处会有多个 Package，构建顺序会由 OBS 根据依赖情况进行调整。
![2023-2-14_52534.png](/2023-2-14_52534.png)

小规模使用OBS工作流的 pr 有:
	1). Import Debian version 5.20230130 by hudeng-go · Pull Request #1 · deepin-community/dh-python (github.com)
	2). init: init stow by tsic404 · Pull Request #1 · deepin-community/stow (github.com)
	3). init: init intltool-debain by tsic404 · Pull Request #1 · deepin-community/intltool-debian (github.com)
	4). feat: init by xzl01 · Pull Request #1 · deepin-community/neofetch (github.com)
  
想了解关于 OBS 更多的使用请参考[OBS用户手册](https://openbuildservice.org/help/manuals/obs-user-guide/)

在使用过程中有遇到任何问题都可以到 [deepin-cicd](https://matrix.to/#/#deepincicd:deepin.org) 群中反馈。

> ps: deepin OBS 实例目前由于构建资源问题暂不开放注册。
{.is-warning}

obs构建产物获取:
    以[deepin-community:fstrcmp:PR-1](https://build.deepin.com/project/show/deepin:CI:deepin-community:fstrcmp:PR-1)为例，在project主页面 -> Repositories -> deepin_develop  ->  Go to download repository ,通过以上路径找到仓库地址。
 
仓库使用： `echo "deb [trusted=yes] https://ci.deepin.com/repo/obs/deepin:/CI:/deepin-community:/fstrcmp:/PR-1/deepin_develop/ ./" > /etc/apt/sources.list`

![2023-2-14_92866.png](/2023-2-14_92866.png)

![2023-2-14_95424.png](/2023-2-14_95424.png)
  
---

  

3. call-auto-tag
   tag创建工作流，PR修改debian/changelog时将deb版本号自动创建成Tag，:会被替换成% ~会被替换成_ ，DISTRIBUTION为UNRELEASED时不会触发tag的创建
---


4. call-build-tag
   tag创建完成后自动触发构建任务，构建完成的deb会合入unstable仓库，一次性引入多个包时需注意将其依赖通过Tag创建工作流合入unstable仓库以免后续依赖包无法构建，仓库地址说明请参见[仓库流转规范](https://wiki.deepin.org/zh/01_deepin%E9%85%8D%E5%A5%97%E7%94%9F%E6%80%81/01_deepin%E5%85%A5%E9%97%A8/02_%E5%BC%80%E5%8F%91%E7%9B%B8%E5%85%B3/04_%E4%BB%93%E5%BA%93/%E4%BB%93%E5%BA%93%E6%B5%81%E8%BD%AC%E8%A7%84%E8%8C%83) 

---

5. call-chatOps
   权限管控，Pull Request 机器人.
---


6. call-clacheck
   cla检查，验证开发者是否签署cla，同时这也是提交PR必须签署的协议
---


**四、软件包集成**

通过软件包更新流程将软件包提交testing仓库，测试团队定期验证testing仓库状态，持续修复问题待到符合发布标准后定期合入主仓库发布。详情请参见仓库流转规范。示例[Repository-Integration/pull/50](https://github.com/deepin-community/Repository-Integration/pull/50)
集成步骤：
1. 在[Repository-Integration](https://github.com/deepin-community/Repository-Integration) 提交PR

2. 修改intergration.yml文件提交填写集成的软件包，以及版本
		集成软件包的名称必须为deepin-community下的正确的仓库名，一次可集成多个软件包(推荐是不超过20个)，请按照格式填完写yml文件
    字段说明
-     repo #软件包名称
-     tag #tag版本 
-     tagsha: "3adc55ff5a3ef9751026cc598aeed88c4a3d46e6" #commit哈希
-     order #废弃字段 默认为0即可

> ps: tag可为空，考虑可能存在当前tag无法进行构建或者验证不通过需要重新提交commit修复 此时为一个commit打tag有些不太合适 因此该字段为空时不强制检查tag,打包时使用的版本为commit哈希里changelog的版本号,如无此特殊情况还请按规范填写对应tag，tag不为空时CI会效验tag是否存在
{.is-info}


3. 提交PR
	PR提交后相关工作流开始执行，检查结构会由机器人在当前PR下添加评论，有些项目需要进行管理员进行revie进阅读相关提示, 工作流完成后可以在OBS平台查看相关的project，具体为test-intergration-pr-50命名形式，可以主页检索，自行进行相关验证
  
4. 信息记录
	 集成的历史信息记录在[records.yml](https://github.com/deepin-community/Repository-Integration/blob/master/records.yml)文件中
 
5. 集成完毕
	 集成完毕后PR会被关闭，所提交的软件包会自动构建后合入[testing仓库](https://ci.deepin.com/repo/release/beige/pool/main/) 中，后续按仓库流转规范合入主仓库中, 仓库介绍详情请参阅[仓库流转规范](https://wiki.deepin.org/zh/01_deepin%E9%85%8D%E5%A5%97%E7%94%9F%E6%80%81/01_deepin%E5%85%A5%E9%97%A8/02_%E5%BC%80%E5%8F%91%E7%9B%B8%E5%85%B3/04_%E4%BB%93%E5%BA%93/%E4%BB%93%E5%BA%93%E6%B5%81%E8%BD%AC%E8%A7%84%E8%8C%83)


TODO:

