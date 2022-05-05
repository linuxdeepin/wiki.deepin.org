---
title: Source_package
description: 
published: true
date: 2022-04-21T03:57:20.695Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:20.695Z
---



## Summary

The commonly seen archive format, .tar.gz, can be used to pack source code of program. Developer may also use other formats like zip, 7z or rar.

To convert source code into an executable binary, one needs to do configuration and compilation, which is a little be complex. For commonly used programs, such as firefox and chrome, these works have already been done, so that you can directly install binary package to your computer and use them. For other software that only source code is provided, it is necessary to follow certain steps before a usable program can be obtained.

## Obtaining source code

All source code of deepin components can be obtained from official repository. For example, to get the source code of deepin-music-player, execute in terminal:

    sudo apt-get source deepin-music-player 

## Compile and install

A general procedure of compile and install, using "make" utility, contains three step: configure, make and make install.

Install gcc and make:

    sudo apt-get update && sudo apt-get install build-essential

Extract files from source code package:

    tar -zvxf XXX.tar.gz 

Swtich to the directory you get:

    cd XXX  ## 

Do configurations:

    ./configure

If any dependency is missing, an error will be generated during executing of file "configure". Most dependency problem can be solved by installing corresponding packages using apt or synaptic.

Do compilation:

    make

This will generate the executable binaries, and may take a long time when the whole program is very large.

Do installation:

    sudo make install

This will install the executables, libraries as well as documents (if available) to the path specified during the configuration step.

Note: Please follow the instructions in README file or INSTALL file for the exact way of installing.

## Uninstalling

Switch to the directory that contains the source code, then execute:

    make uninstal

Or, it is possible to delete binaries (as well as unused libraries and resource) directly.'

Note: Please read description file before doing configuration and installation, as it sometimes requires changes in compile configurations. Some packages does not support command "make install" for uninstalling, so users need to remove the compiled file by hand.

As software components exist in different locations in system, it might not be easy to remove all of them during uninstallation. You can then use

    ./configure --prefix=Target

to specify the target directory, so that the whole program is installed to that target, and can be removed later using

    rm -rf Target

Installling from source code may be difficult for new Linux users, so it is not recommended for beginners.

## Frequently asked questions

### Why not pack all softwares into deb packages?

Most source code  does not depend on the hardware that they run on, so most programmers maintain only their source code, rather that the binaries. In contrast, binary deb packages depend on hardware and platform architecture, and can not be easily migrated from one platform to the other.

There are also packages of other format, such as rpm, pkg. deb js just one of those package formats, and it can only be dealt with dpkg utility.

The source code, published by developers, grants also authority to user of making his / her own binary packages. This may be useful when users want program to be of higher performance by adjusting the parameters of compilation. See manual of dpkg for more details about package making.

## Reference

[linux源码包软件的安装与卸载](http://blog.csdn.net/samxx8/article/details/7570542)