---
title: pm_introduction
description: 
published: true
date: 2022-08-08T10:45:44.235Z
tags: 
editor: markdown
dateCreated: 2022-05-17T02:20:26.608Z
---

## Summary

One of the main points how Linux distributions differ from each other is the package management. In this part of the Linux jargon buster series, you’ll learn about packaging and package managers in Linux. You’ll learn what are packages, what are package managers and how do they work and what kind of package managers available.

## What is a package manager in Linux?

In simpler words, a package manager is a tool that allows users to install, remove, upgrade, configure and manage software packages on an operating system. The package manager can be a graphical application like a software center or a command line tool like apt-get or pacman.

## What is a package?

A package is usually referred to an application but it could be a GUI application, command line tool or a software library (required by other software programs). A package is essentially an archive file containing the binary executable, configuration file and sometimes information about the dependencies.

In older days, software used to installed from its source code. You would refer to a file (usually named readme) and see what software components it needs, location of binaries. A configure script or makefile is often included. You will have to compile the software or on your own along with handling all the dependencies (some software require installation of other software) on your own.

To get rid of this complexity, Linux distributions created their own packaging format to provide the end users ready-to-use binary files (precompiled software) for installing software along with some metadata (version number, description) and dependencies.

## Source Code Compilation Vs Packaging in Linux

Around mid 90s, Debian created .deb or DEB packaging format and Red Hat Linux created .rpm or RPM (short for Red Hat Package Manager) packaging system. Compiling source code still exists but it is optional now.

To interact with or use the packaging systems, you need a package manager.

## How does the package manager work?

Almost all Linux distributions have software repositories which is basically collection of software packages. Yes, there could be more than one repository. The repositories contain software packages of different kind.

Repositories also have metadata files that contain information about the packages such as the name of the package, version number, description of package and the repository name etc. This is what you see if you use the apt show command in Ubuntu/Debian.

Your system's package manager first interacts with the metadata. The package manager creates a local cache of metadata on your system. When you run the update option of the package manager (for example apt update), it updates this local cache of metadata by referring to metadata from the repository.

When you run the installation command of your package manager (for example apt install package_name), the package manager refers to this cache. If it finds the package information in the cache, it uses the internet connection to connect to the appropriate repository and downloads the package first before installing on your system.

## Package Manager Handling Dependencies In Linux

A package may have dependencies. Meaning that it may require other packages to be installed. The package manager often takes care of the dependencies and installs it automatically along with the package you are installing.

Similarly, when you remove a package using the package manager, it either automatically removes or informs you that your system has unused packages that can be cleaned.

## Different kinds of package managers

Package Managers differ based on packaging system but same packaging system may have more than one package manager.
For example, RPM has Yum and DNF package managers. For DEB, you have apt-get, aptitude command line based package managers.

Package managers are not necessarily command line based. You have graphical package managing tools like Synaptic. Your distribution's software center is also a package manager even if it runs apt-get or DNF underneath.

In addition to,there are the new universal packaging formats like Snap 、Flatpak 、Appimage for now.
