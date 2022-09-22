---
title: rpm
description: 
published: true
date: 2022-08-09T01:57:07.060Z
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
| yum repolist all	|	list all repository | 
| yum list all		| list all packages of repository | 
| yum info  packagename	| view package information |
| yum install packagename | installing a package |
| yum reinstall packagename | resinstall a package |
| yum update packagename	| upgrade a package |
| yum remove packagename	| remove a package |
| yum clean all		| clean cache of all repository |
| yum check-update	|	check for updatable software package |
| yum grouplist		| View the package groups that have been installed |
| yum groupinstall package_group_name	| Install the specified package group |
| yum groupremove package_group_name | remove the specified package group |
| yum groupinfo package_group_name | Query information of specified package group |




