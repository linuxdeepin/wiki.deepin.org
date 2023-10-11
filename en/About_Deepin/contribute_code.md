---
title: How to contribute code
description: The R&D workflow related to deepin
published: true
date: 2023-10-11T04:20:59.345Z
tags: contribute code, contribute, how to contribute code
editor: markdown
dateCreated: 2023-10-11T04:20:59.345Z
---

## Before the Beginning
The R&D workflow related to deepin is located on GitHub, so the code contribution process is not much different from the regular Fork + Pull Request process on GitHub.  [About pull requests](https://docs.github.com/zh/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)。

### Contribution channel
1. linuxdeepin organization: hosts various self-developed projects of deepin, including DDE and dozens of other applications, as well as some tool projects.  [linuxdeepin Contribution Guidelines](https://wiki.deepin.org/zh/05_HOW-TO/06_%E5%8F%82%E4%B8%8Edeepin%E8%B4%A1%E7%8C%AE%E7%9B%B8%E5%85%B3/linuxdeepin_%E8%B4%A1%E7%8C%AE%E6%8C%87%E5%8D%972)
2. deepin-community organization: hosts all projects related to the deepin distribution, and also maintains the work of upstream projects that the distribution depends on. [deepin-community Contribution Guidelines](https://wiki.deepin.org/zh/05_HOW-TO/06_%E5%8F%82%E4%B8%8Edeepin%E8%B4%A1%E7%8C%AE%E7%9B%B8%E5%85%B3/deepin%E7%A4%BE%E5%8C%BA%E8%B4%A1%E7%8C%AE%E6%8C%87%E5%8D%97)

### Code of Conduct
A healthy community requires everyone to create together, here we referencing [Contributor Covenant 2.1](https://www.contributor-covenant.org/zh-cn/version/2/1/code_of_conduct/), please abide by the convention together.

[deepin Community Code of Conduct](https://wiki.deepin.org/zh/06_%E5%85%B3%E4%BA%8EDeepin/Deepin%E7%A4%BE%E5%8C%BA/%E8%A1%8C%E4%B8%BA%E5%87%86%E5%88%99)

### License Agreement
Each project comes with its corresponding license agreement.  Before you start contributing code, please ensure that you understand and agree with the corresponding license agreement.If not stated otherwise, your code will be licensed using the same license agreement as the corresponding project.

In order for the deepin community to make necessary adjustments to the code license agreement to address licensing issues, when you plan to contribute code, you also need to sign the corresponding CLA（Contributor License Agreement）（Note: For individual contributors, signing can be easily completed online）. Details related to this can be found in [Contributor License Agreement](https://wiki.deepin.org/zh/03_%E6%8A%80%E6%9C%AF%E8%A7%84%E8%8C%83/01_%E6%96%87%E6%A1%A3%E8%A7%84%E8%8C%83/%E8%B4%A1%E7%8C%AE%E8%AE%B8%E5%8F%AF%E5%8D%8F%E8%AE%AE) .

If you have any questions regarding this matter, please email us for consultation: support@deepin.org

### Contribution process

![code-contribute.png](/06_关于Deepin/code-contribute.png)

## Get code
If you are not sure where and how to obtain the source code, please refer to [Obtaining and Using Source Code](https://wiki.deepin.org/zh/05_HOW-TO/06_%E5%8F%82%E4%B8%8Edeepin%E8%B4%A1%E7%8C%AE%E7%9B%B8%E5%85%B3/%E8%8E%B7%E5%8F%96%E5%92%8C%E4%BD%BF%E7%94%A8%E6%BA%90%E7%A0%81).

### Create Github account
How to create a Github account, you can refer to [Signing up for GitHub](https://docs.github.com/en/get-started/signing-up-for-github/signing-up-for-a-new-github-account)

### Fork repository
Currently, all the source codes of deepin community open source projects are available on GitHub, so you only need to find the corresponding project and then obtain the code through git clone or your preferred method.

[Git User Manual](https://git-scm.com/book/en/v2)

Since the master branch of each project in the deepin community is a development branch, the master branch status of an application may depend on other versions that are also in the master branch status.Therefore, directly obtaining the code from the master branch does not always guarantee compilation in many cases. So, when you obtain a version from the git repository, it is recommended that you git checkout to switch to the corresponding tag you need, if necessary, before attempting to build.

Note: The corresponding README.md document for a project usually includes steps related to building the project. Please refer to these documents to help you build the project.

## Submit patches or Pull Requests
The deepin community accepts GitHub Pull Requests, and the merging of development commits is done through Pull Requests. You can directly click on the Fork button of the corresponding project to obtain your own Fork, submit it above it, and directly initiate a Pull Request through the GitHub webpage after modification. For more information on how to use Pull Requests, see the GitHub documentation [About pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests).

Thanks to GitHub's code review and automation tools, it's easy to conduct code reviews and use automated builds to find problems in your code. If you're not familiar with GitHub's code review process, here's how it works [About pull request reviews](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/about-pull-request-reviews)。

### Software Package Development Warehouse
For the convenience of developers, we provide three versions of development repositories: deepin, Arch Linux, and Debian. All the project will automatically build packages into the corresponding repositories after merging in new code, currently only supporting projects in the linuxdeepin organization. Please check for details [Create a new git repository using action](https://wiki.deepin.org/zh/%E4%BD%BF%E7%94%A8action%E5%88%9B%E5%BB%BA%E6%96%B0%E7%9A%84git%E4%BB%93%E5%BA%93)。

### Continuous integration inspection
To ensure code quality and project compliance, we have configured corresponding continuous integration checks for the project. Among them, common inspections include the following：

- CLA Check
- Commit Check（commitlint）
- UOS Environment Build Check（build-deb）
- deepin and other distribution build checks（build-distribution）

For the check of Commit information, please refer to [Commit Submission Specification](https://wiki.deepin.org/zh/Commit%20%E6%8F%90%E4%BA%A4%E4%BF%A1%E6%81%AF%E8%A7%84%E8%8C%83).

#### GitHub Continuous Integration assistance tools
To make the development process easier and give developers and contributors the flexibility to handle problems with continuous integration checks, we've provided GitHub's Continuous Integration with convenient commands to perform actions such as assigning code reviewers, re-triggering specific checks, and proactively performing code merges. For these commands, we also provide plugin scripts that enable developers to automate the input of the corresponding commands.

How to use Continuous Integration(CI), please refer to [Pull Request robot command list](https://wiki.deepin.org/zh/Pull%20Request%20%E6%9C%BA%E5%99%A8%E4%BA%BA%E5%91%BD%E4%BB%A4%E5%88%97%E8%A1%A8)。

#### Debug Service
In order to facilitate developers to quickly debug and locate problems, we provide a debuginfod service. Debuginfod provides the ability to quickly load debugging information based on an HTTP file server. For specific operations, please refer to [debuginfod introduces](https://wiki.deepin.org/zh/debuginfod%20%E4%BB%8B%E7%BB%8D).

###  Add a new code library
If you need to apply for the creation of a new code repository, please send a request in the developer's email list（[Communication Methods](https://wiki.deepin.org/zh/06_%E5%85%B3%E4%BA%8EDeepin/Deepin%E7%A4%BE%E5%8C%BA/%E4%BA%A4%E6%B5%81%E6%96%B9%E5%BC%8F)）, indicating the name of the project to be created and which GitHub organization to store it in. Additionally, please provide a detailed explanation of the reason for the creation, and the community administrator will reply to the email notifying you of the results of the creation. For example：

```
Name：qtbase
Organization：deepin-community
USE：A graphics development library, which is relied on by DDE, DTK and many deepin applications
```
You can add a more detailed description of why the repository was created in the body of the email.

### Points for attention
Once you have ensured that your changes are acceptable, you are ready to write your patch and submit your Patch / Pull Request in the way mentioned in the above paragraph. Please take note of the following considerations when you start preparing your patch:

1. Commit information specification
Please refer to [Commit Submission Specification](https://wiki.deepin.org/zh/Commit%20%E6%8F%90%E4%BA%A4%E4%BF%A1%E6%81%AF%E8%A7%84%E8%8C%83).

2. A Pull Request Does One Thing
You may want to make a series of changes to a project, but in order to ensure that the code submission information is organized, and also to ensure that the code review is convenient and orderly, please make sure that one of your Pull Requests only does one thing, for different changes, please submit multiple different Pull Requests to achieve.

3. Brand specific terms
During the code modification and submission process, some brand specific terms related to deepin may be involved, please refer to [Brand specific terminology guidelines](https://wiki.deepin.org/zh/%E5%93%81%E7%89%8C%E4%B8%93%E6%9C%89%E5%90%8D%E8%AF%8D%E6%8C%87%E5%AF%BC%E6%96%B9%E9%92%88) .

### Contact Us
For deepin community related research and development activities, in addition to the developer center on GitHub, we also use the [deepin-devel mailing list](https://www.freelists.org/list/deepin-devel) to notify relevant matters and discuss related topics.We recommend those who want to participate in deepin community to subscribe to this mailing list and participate in the discussion as needed.

There are other communication channels and contacts, but the deepin-devel developer mailing list involves announcements and discussions of major R&D-related issues, so please consider subscribing to the developer mailing list if you decide to continue contributing to deepin-related development.

We offer regular Issue discussion boards, mailing lists, and live chat as ways to get in touch. For specific contact information, please see [Communication Methods](https://wiki.deepin.org/zh/06_%E5%85%B3%E4%BA%8EDeepin/Deepin%E7%A4%BE%E5%8C%BA/%E4%BA%A4%E6%B5%81%E6%96%B9%E5%BC%8F) .