---
title: Package
description: 
published: true
date: 2022-05-07T02:30:27.108Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:56:28.170Z
---

## Summary

This entry gives brief introduction to the basic concept of Debian package in Linux.

## Types of package

The Debian package file usually includes binaries, libraries, configuration files as well as man / info manuals. The extension of a Debian package file is ".deb", so we often call it a "deb" file.

There are mainly two types of Debian package: binary package and source package.

* Binary packages: Packages containing binaries, libraries, configurations, licence information or other documents;

* Source packages: Packages containing source code, change logs, build constructions or compile tools. Usually packed with tar utility.

You can use `file` command to check the type of a package file. For example:

    $ file  g++_4.1.2-9ubuntu2_i386.deb
    g++_4.1.2-9ubuntu2_i386: Debian binary package (format 2.0)

It is worth noting that the "virtual" package in Debian package system is a collection of multiple software packages, with one of them specified as the default option. This is for avoiding conflicts during software installation. For example, exim, sendmail and postfix are all software for mail proxy, thus "mail-transport-agent" are created as a virtual package for them. When a user want to install package "mail-transport-agent", the default one in this virtual package will be installed.

## Nomination of packages

In Debian package system, the nomination of packages follows the rule:

        Filename_Version-Reversion_Architecture.deb

where:

Filename is the file name of this software;

Version is the version number;

Reversion is the revision number;

Architecture is the architecture it is applied to.

Generally, revision number is set by the developer or the person who compiled this package. The revision number is increased by 1 every time it is modified.

For example, a deb package named "g++_4.1.2-9ubuntu2_i386.deb" has file name "g++", version number "4.1.2", revision number "9ubuntu2" and architecture "i386".

## The priority of packages

Debian package system uses package priority to determine the importance of each packages when choosing and installing software. A package of higher priority must not depend on a package with lower priority, thus the software freezing is achieved in a structured process, which benefits the schedule of a new release.

A basic Debian system consists of packages of "Required" and "Important" level. These two kinds of packages are depended by other packages, so they need to be stable for the usability of the whole system. The packages of "Standard" level are then frozen, followed by those of "Optional" and "Extra" level before the final release of a new version of system.

## Package state

To record the installing actions done by user, package state is used to describe the status of each package:

* Expected state: the wanted state of a package by user

* Current state: the final state of a package

## Package dependencies

Linux is a complex system where software components need to be well organized and cooperate for the whole system to function normally. Thus Linux is designed to constructed with a strong coupling of each software. This guarantee the tightness of software connections, reduce the error that might exist in  intermediate links. However, this also cause the problem of software dependencies and component conflicts.

To solve the problem, Debian package system provides a mechanism for solving dependencies with a detailed definition.

Package manager use software dependence to judge the coupling between different programs in the system, and do installation or uninstallation according to the dependence. For example, gcc depends on package binutils, which provides linker and compiler. If a user tries to install gcc without installing binutils, the package manager will stop the installation and tell user to install binutils first.

## Frequently asked questions

### How to convert a RPM package to DEB package

deepin OS provides a huge number of software packed with DEB format. If you really want to use software from a RPM package, try to convert it with "alien" command. However, it is not advised to do so, as this may cause dependency problems.

First, install alien by executing in terminal:

         apt-get install alien

Convert RPM to DEB:

         alien XXX.rpm

To convert the package "xine-skins-1.8-1.lvn5.noarch.rpm" to a deb file, execute:

        $ sudo alien xine-skins-1.8-1.lvn5.noarch.rpm
        xine-skins-1.8-1.lvn5.noarch.rpm: Header V3 DSA signature: NOKEY, key ID a109b1ec
        xine-skins-1.8-1.lvn5.noarch.rpm: Header V3 DSA signature: NOKEY, key ID a109b1ec
        Unknown host
        xine-skins_1.8-2_all.deb generated

This will generate a DEB package from the original RPM package in the same directory.

## Reference

[Ubuntu标准教程](http://book.51cto.com/art/200811/96247.htm)