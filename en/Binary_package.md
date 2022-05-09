---
title: Binary_package
description: 
published: true
date: 2022-05-07T02:28:27.762Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:54:02.333Z
---

## Summary

A binary package is a per-compiled package aimed to run in a system of specified architecture. Packages of different architectures do not replace each other.

Generally, binary packages refer to those packages that do no belong to source packages. There are several types of binaries:

* .deb packages: Used by dpkg, the package manager in Debian and its derivatives. As deepin also uses dpkg, it is convenient to install programs from binary DEB packages, either from Debian or Ubuntu repositories.

* .bin packages: General binary packages that can be commonly accepted by most distributions.

* .run packages: Many graphics drivers adopt this kind of packages.

* .sh script: Commonly used for installing scripts and other binaries. When executing permission is granted, users can directly run it by one click.

* Executable files with no extension: Some compiled binaries do not have any extension (like those of ".exe" in Windows). Users just need to set executing permission to them before they can launch it from terminal or with file manager

## Installing and launching

### deb packages

Be careful when installing deb packages. If you apply a wrong version of deb to your system, the dependency of system software may be broken!

Two ways to install deb packages:

Using program Gdebi. Double click a package in the file manager, and it will be opened with Gdebi by default. Just follow the instruction.

Using command "dpkg". For example, to install the package "chrome.deb" in the current directory, execute in terminal:

        sudo dpkg -i chrome.deb

### bin packages

Take the bin package of crossover as example. After download the package from homepage of Crossover, we can get a file called "install-crossover-12.5.1.bin". Then we need to grant it the executing permission:

Execute in terminal:

         sudo chmod +x install-crossover-12.5.1.bin 

or right click on the file -> Property, then check "Allow to execute as  program". Then execute in terminal:

         ./install-crossover-12.5.1.bin 

to launch the setup wizard.

If the command above does not have any effect, you can try to run it with root permission:

        sudo ./install-crossover-12.5.1.bin 

### RUN packages

The way to install RUN packages is quite similar to that of BIN packages. Here we take example of closed source graphics driver of ATI. First download the right driver from official site of ATI, we get file "ati.run". Then we need to grant it the executing permission:

         sudo chmod +x ati.run

or right click on the file -> Property, then check "Allow to execute as  program".

Then execute in terminal:

         ./ati.run

to get a setup wizard. Similarly, if this does not help, try to run with sudo:

         sudo ./ati.run

### SH scripts

For example, we have got a script named "deepin.sh". To grant it the executing permission, either run in terminal:

         sudo chmod +x deepin.sh

or right click on the file -> Property, then check "Allow to execute as  program".

Then execute in terminal:

         ./deepin.sh

or double click this script to "Run in terminal".

### Executable files with no extension

These are usually "green" software, which can be launched directly after being put to the correct place, with optional soft link created for convenient use.

Take Firefox browser as example. First download the binary package "Firefox-latest-x86_64.tar.bz2", then launch the terminal in the directory that contains this file, and execute:

        sudo cp Firefox-latest-x86_64.tar.bz2 /opt  ## Copy the downloaded file to directory "/opt"
        cd /opt  ## Go into directory "/opt"
        sudo tar -xvjf Firefox-latest-x86_64.tar.bz2 # Extract files from that file for use

After this, a directory named "firefox" will appear in the /opt. Then create a soft link in /usr/bin:

        sudo ln -sf /opt/firefox/firefox  /usr/bin/firefox

The option "-f" means that the new link will replace the old one (if any). Now you can launch Firefox either by executing "firefox" in terminal,

## Uninstallation and remove

### DEB packages

Double click on the original .deb file, then follow the instruction from Gdebi.

Or we can use command to do so. Take Chrome as example. Execute in terminal:

        sudo apt-get  remove google-chrome                 ## Uninstall google-chrome (while keeping configuration files)
        sudo apt-get --purge remove google-chrome     ## Uninstall google-chrome (and delete its configuration files)
        sudo dpkg -r google-chrome                               ## Uninstall google-chrome (while keeping configuration files)
        sudo dpkg --purge google-chrome                      ## Uninstall google-chrome  (and delete its configuration files)

### BIN packages

These packages generally have their own uninstallation scripts. Just run these script to finish uninstallation. For example, to uninstall crossover that has previously been installed to /opt, double click the file "/opt/cxoffice/bin/cxuninstall" to uninstall it.

Note: See the help documentation from that software to get the exact path of uninstallation script.

### RUN packages

Similar to BIN packages. For example, to uninstall the graphics driver from ATI, execute in terminal:

        sudo sh /usr/share/ati/fglrx-uninstall.sh

Note: See the help documentation from that software to get the exact path of uninstallation script.

### SH scripts

Remove the script itself to uninstall it.

### Executable files with no extension

Remove the executable itself and all its resources to uninstall it.

For example, to uninstall the Firefox that has previously been  installed to /opt/firefox, run in terminal:

        sudo rm -rf  /opt/firefox/firefox
