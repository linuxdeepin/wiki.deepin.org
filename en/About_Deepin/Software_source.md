---
title: Software_source
description: 
published: true
date: 2022-05-17T02:16:07.927Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:17.113Z
---

## Summary

Software sources refers to the repositories that are used to store various Linux applications, including free or non-free software. There are several types of repositories:

- Network repository: binaries and source code from a server
- ISO repository: ISO files from distributions
- Local repository: directories that contain software packages.

Take official repository of deepin (<http://packages.deepin.com/>) as example:

- pool/    directory contains all released packages
- project/    contains resources used by developper

In order to provide faster and stable repositories for user, deepin has created new mirrors of its official repository. There are currently 70 mirrror sites, covering 24 countries all over the world.

## Source configuration

The default configuration of source for deepin is "/etc/apt/sources.list", which contains the web address of description of each software packages.

deepin inherits apt utility from debian. apt is a powerful tool for searching, installing, upgrading and removing software obtained from repository. Other Linux distributions, like Red Hat or Gentoo, use their own tools which have the similar functions.

To view current configurations of source, execute in terminal:

    sudo gedit  /etc/apt/sources.list

The default content is like:

    deb [by-hash=force] http://packages.deepin.com/deepin stable main contrib non-free
    #deb-src http://packages.deepin.com/deepin stable main contrib non-free

The first word of each line, either "deb" or "deb-src", describe the type of the repository:

- deb: Repository of binary packges, i.e. pre-compiled package ready to be used
- deb-src: Repository of source code packages, as well as package description (.dsc) and changes made to source code (.diff.gz).

## Change software source

There are two ways of modifying source:

Launch control panel -> System Info -> Update -> Update settings -> Switch Mirror, then choose your prefered source.

Or modify configuration file manually (only when you have fully understood its mechanism):

    sudo gedit  /etc/apt/sources.list

After modification, update local cache of software source:

    sudo apt-get update

## Create a mirror for deepin repository

If you would like to offer a repository mirror to deepin users, please refer following instructions:

- Make sure that you have enough disk space.

- Synchronizing package repository (about 330 GB in total)

    rsync -av --delete-after rsync.deepin.com::deepin/ /var/www/deepin/

- Synchronizing ISO repository (about 520 GB in total)

    rsync -av --delete-after rsync.deepin.com::releases/ /var/www/deepin-cd/

※Note:

1. "/var/www" is used for storage in the example above. You can use other directory for deepin mirrors if you like.

2. It is recommand to add a cron job for daily update of repository, so that your mirror is always up-to-date.

3. It is recmmanded to synchronize deepin package repository first, then ISO repository.

4. Please do not store other files (files not from official mirrors) in the directory if you intent to make it a public mirror.

5. Any feedback or suggestion sent to support@deepin.org is welcome.

## Reference

[深度操作系统镜像源页面](http://www.deepin.org/mirror.html)
