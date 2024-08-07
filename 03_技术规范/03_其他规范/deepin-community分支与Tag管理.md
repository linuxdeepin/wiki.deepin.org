---
title: deepin-community分支与Tag管理
description: 分支与Tag管理
published: true
date: 2024-07-24T08:08:38.418Z
tags: 
editor: markdown
dateCreated: 2022-09-21T02:21:35.049Z
---

**分支命名规范**

deepin 社区版为滚动发布制，一般没有维护分支，`master` 分支即为研发基线分支。对于开发者，若需要多个提交才能完成一个特性时，由于研发过程使用 GitHub 的 Fork + Pull Request 模式，故也不需要单独创建研发分支。但特殊情况下，若确需使用研发分支，则可使用 `develop/*` 作为临时的特性研发分支。


*  `master` 用于作为默认分支，接收 Pull Request，作为当前版本的开发分支使用
*  `develop/*` 仅特殊情况下允许使用的临时研发分支,如在适配RISC-V架构时需要给一些项目提交patch但是该patch暂时不适合合入主线，那么可以申请切出`develop/riscv6` 的分支单独维护，等到稳定后在合并到主线分支
   ps：如果有特殊的需求以上规范无法满足，请通过邮件列表 issues等方式联系我们进行处理

 

**changelog版本规范**



`upstreamversion-${ver1}deepin${ver2}`

1. 假设上游项目打包版本号为 x.y.z ，deepin打包版本则为 `x.y.z-${ver1}deepin${ver2}` 
 ver1：ver1为0时表示 deepin自行打包的上游软件，ver1不为0时表示来自上游的quilt软件包自带的-ver版本
 ver2：表示来自deepin社区的patch数量，依次递增，可为空

2. 来自deepin community自行打包的上游软件 以0deepin开头标识，若该项目添加了来自deepin的patch则以deepin1 标识，依次累加, 版本号形式x.y.z-0deepin1 , 若上游已经添加-2这类版本号，版本号则为 x.y.z-2deepin1

3. 若需要集成native软件包到deepin，则应改为quilt格式 遵循条例2

4.  CI自动构建版本号 `x.y.z-${ver1}deepin${ver2}+u001+rb1`，001为距离上一次修改changelog的commit次数，rb1为rebuild次数，依次累加
> ps：commit版本号使用仅限于开发阶段自测验证，不应流转到集成阶段
{.is-warning}


**changelog版本规范修订(待讨论)**
4. rebuild版本号规范
上文一二条保持不变，本次修订主要针对rebuild版本规范
若存在需要rebuild软件包，
quilt软件包rebuild规范：
情况1：ver2不为空时直接+rb{ver}后缀，表现形式为`x.y.z-1deepin1+rb1`
情况1：ver2为空时的rebuild版本ver2默认为0，表现形式为
  `x.y.z-1deepin0+rb1`
native软件包rebuild规范：
情况1：ver2不为空时直接+rb{ver}后缀，表现形式为`x.y.zdeepin1+rb1`
情况1：ver2为空时的rebuild版本ver2默认为0，表现形式为
  `x.y.zdeepin0+rb1`
> ps：rebuild版本号使用仅限于依赖包升级，相关依赖树需要基于新版本构建编译的场景使用
{.is-warning}


> 注：如无特殊情况所有的rebuild版本应该在构建系统中修改，不应该提交到代码存储库中
{.is-warning}

> 注： deepin0版本的用意是为了方便后续的更新迭代，安装版本号规则 deepin+rb1版本大于deepin1+rb1，所以使用deepin0后缀方便维护
{.is-info}

3. 集成native软件包
将native软件包直接改成quilt格式存在一些changelog版本格式不兼容问题
因此重新修订native版本规范，形式为 `upstreamversiondeepin${ver2}`
native软件包没有-的连字符，因此直接在上游版本号后deepin${ver2} 标记来自deepin的修改

**分支与tag的创建申请**

**新建分支：**

请联系所属项目的SIG组管理员进行分支创建

> TODO： 暂未设计相关工作流管控分支的创建，若有想法欢迎在邮件列表 issues等渠道与我们讨论
{.is-info}


**Tag创建**： 

修改debian/changelog文件时，会自动触发auo-tag的工作流将changelog中的version创建成tag, :会被替换成% ~会被替换成_ ，DISTRIBUTION为UNRELEASED时不会触发tag的创建

