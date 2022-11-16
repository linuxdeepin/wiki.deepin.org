---
title: deepin-community协作指南
description: 协作指南
published: true
date: 2022-11-16T05:16:52.781Z
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

2. call-build-deb
   软件包构建工作流，提交PR时触发包构建检查

3. call-auto-tag
   tag创建工作流，PR修改debian/changelog时将deb版本号自动创建成Tag，:会被替换成% ~会被替换成_

4. call-build-tag
   tag创建完成后自动触发构建任务，构建完成的deb会合入unstable仓库，一次性引入多个包时需注意将其依赖通过Tag创建工作流合入unstable仓库以免后续依赖包无法构建，仓库地址说明请参见[仓库流转规范](https://wiki.deepin.org/zh/01_deepin%E9%85%8D%E5%A5%97%E7%94%9F%E6%80%81/01_deepin%E5%85%A5%E9%97%A8/02_%E5%BC%80%E5%8F%91%E7%9B%B8%E5%85%B3/04_%E4%BB%93%E5%BA%93/%E4%BB%93%E5%BA%93%E6%B5%81%E8%BD%AC%E8%A7%84%E8%8C%83) 

5. call-chatOps
   权限管控，Pull Request 机器人.

6. call-clacheck
   cla检查，验证开发者是否签署cla，同时这也是提交PR必须签署的协议

**四、软件包集成**

通过软件包更新流程将软件包提交testing仓库，测试团队定期验证testing仓库状态，持续修复问题待到符合发布标准后定期合入主仓库发布。详情请参见仓库流转规范。示例[Repository-Integration/pull/1](https://github.com/deepin-community/Repository-Integration/pull/1)
集成步骤：
1. 在[Repository-Integration](https://github.com/deepin-community/Repository-Integration) 提交PR

2. 修改intergration.yml文件提交填写集成的软件包，以及版本
		集成软件包的名称必须为deepin-community下的正确的仓库名，tag也必须存在，一次可集成多个软件包，请按照格式填完写yml文件
    字段说明
    repo #软件包名称
    tag #tag版本
    order #构建级别，相同级别并发构建，不同级别按照大小依次构建，最高支持1-10个级别，1为最高优先级。设置该字段目的是为了解决同一批次集成多个软件包中有依赖顺序的情况。

3. 提交PR
	PR提交后相关工作流开始执行，检查结构会由机器人在当前PR下添加评论，有些项目需要进行管理员进行revie进阅读相关提示
  
4. 集成完毕
	 集成完毕后PR会被关闭，所提交的软件包会自动构建后合入[testing仓库](https://ci.deepin.com/repo/release/beige/pool/main/) 中，后续按仓库流转规范合入主仓库中

TODO:

