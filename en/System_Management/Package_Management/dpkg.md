---
title: dpkg
description: 
published: true
date: 2022-08-09T01:00:36.309Z
tags: 
editor: markdown
dateCreated: 2022-08-08T10:47:49.420Z
---

# DPKG – Debian Package Management System

dpkg is a base package management system for the Debian Linux family, it is used to install, remove, store and provide information about .deb packages.

It is a low-level tool and there are front-end tools that help users to obtain packages from remote repositories and/or handle complex package relations and these include:

| Command | Funciton |
| - | - |
dpkg -i packagename.deb	| Install a Package
dpkg -i packagename.deb	| Upgrade a Package
dpkg -r packagename	| Remove a Package
dpkg -P packagename	| Remove a Package with configuration file
dpkg --info package.deb	| View the description of a Package
dpkg -c packagename.deb	| View the Content of a Package
dpkg -l 			| List all the installed Packages
dpkg -l packagename	| List the version of Packages
dpkg --version | Display dpkg Version
dpkg --help	| Get all the Help about dpkg
dpkg -L packagename | list files owned by package

# APT (Advanced Packaging Tool)

It is a very popular, free, powerful and more so, useful command line package management system that is a front end for dpkg package management system.

Users of Debian or its derivatives such as Ubuntu and deepin should be familiar with this package management tool.

| Command | Funciton |
| - | - |
| sudo apt search packagename | Searching a Package |
| sudo apt install packagename | Installing a Package |
| sudo apt update	packagename | Updating Package Information sources |
| sudo apt upgrade packagename |	Upgrading a Package |
| sudo apt remove packagename| Uninstalling a Package |
| sudo apt purge packagename | Uninstalling Packages with configuration file |





