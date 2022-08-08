---
title: dpkg
description: 
published: true
date: 2022-08-08T10:58:07.733Z
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
dpkg -L packagename | list files owned by pa

