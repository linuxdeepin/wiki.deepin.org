---
title: General_program_configuration
description: 
published: true
date: 2022-04-21T03:55:45.839Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:55:42.869Z
---



## Summary

This entry introduces general configuration files for programs.

## Configuration file

### Configuration directory

There are two kinds of configurations: user configurations (usually in ~/ and ~/.config) and global configuration (usually in /usr/share). Note that "~/" refers to "/home/YourUserName".

### Backup configuration

For eaxmple, to back up configuration files of program "amule", simply copy directory "~/.amule" to another place, and copy it back when we want to restore this configuration.

Some programs, such as chromium, put their configurations in "~/.config". So we need to copy directory "~/.config/chromium" instead.

### Delete configuration

To use default configuration of one program, simply delete its configuration directory. This is useful when some errors in configurations prevent it from starting.

## Commonly used configurations

### User configurations

Here are some basic configuration files that are usually hidden when viewed in file manager:

Directories:

- .cache: Local caches
- .config: Most configurations are stored here
- .dbus: Configuration of dbus service
- .gconf: Configurations of Gnome applications
- .gnome2: Configurations of Gnome 2 Desktop
- .gvfs: Configurations of gvfs manager
- .local: User-scope configurations of applications

Files:

- .bash_logout: Shell commands needs to be run after user logs out
- .bashrc: Shell commands needs to be run every time user logs in and new shell (bash as default) is opend
- .dlockpid: (To be filled)
- .dmrc: Default configurations of language settings of GDM / KDM session
- .esd_auth: Configurations of ESD sound effect
- .ICEauthority: Like .dmrc, language configuration of GDM session
- .profile: Shell commands needs to be run everytime user logs in
- .Xauthority: Logs for command "startx"
- .xinputrc: Configurations for input method
- .xsession-errors: Error logs from X session

### Clean unused configuration files

Execute in terminal"

    dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P 

If following warnings are generated, then there is no unused configurations.
```
dpkg: error: --purge needs at least one package name argument
Type dpkg --help for help about installing and deinstalling packages [*];
Use 'apt' or 'aptitude' for user-friendly package management;
Type dpkg -Dhelp for a list of dpkg debug flag values;
Type dpkg --force-help for a list of forcing options;
Type dpkg-deb --help for help about manipulating *.deb files;

Options marked [*] produce a lot of output - pipe it through 'less' or 'more' !
```