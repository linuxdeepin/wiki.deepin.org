---
title: rpm
description: 
published: true
date: 2022-08-09T01:28:56.974Z
tags: 
editor: markdown
dateCreated: 2022-08-09T01:25:35.947Z
---

# RPM (Red Hat Package Manager)

This is the Linux Standard Base packing format and a base package management system created by RedHat. Being the underlying system, there several front-end package management tools that you can use with it and but we shall only look at the best and that is:

| Command | Function |			
| - | - |
|rpm -ivh packagename.rpm	| Installing a package |
| rpm -Uvh packagename.rpm | Upgrading a package |
| rpm -e packagename.rpm 	| remove a package |
| rpm -qpi packagename.rpm	| query package information | 
| rpm -qf packagename	|	query file owned by package | 

# YUM (Yellowdog Updater, Modified)

It is an open source and popular command line package manager that works as a interface for users to RPM. You can compare it to APT under Debian Linux systems, it incorporates the common functionalities that APT has. You can get a clear understanding of YUM with examples from this how to guide:

| Command | Function |			
| - | - |
yum repolist all	|	列出所有仓库
yum list all		| 列出仓库中所有软件包
yum info  软件包名称		| 查看软件包信息
yum install 软件包名称| 	安装软件包
yum reinstall 软件包名称| 	重新安装软件包
yum update 软件包名称	|升级软件包
yum remove 软件包名称	|移除软件包
yum clean all		|清除所有仓库缓存
yum check-update	|	检查可更新的软件包
yum grouplist		|查看系统中已经安装的软件包组
yum groupinstall 软件包组	|安装指定的软件包组
yum groupremove 软件包组	|移除指定的软件包组
yum groupinfo 软件包组	| 查询指定的软件包组信息