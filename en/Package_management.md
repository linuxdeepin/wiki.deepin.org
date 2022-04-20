[[zh:软件包管理]]


## Summary

deepin uses dpkg to manage its packages. Beside from graphical tools like Deepin Store and Synaptic, it is also very common to do installation, uninstallation and upgrade using command.

Here we give brief introduction to the command used to manage software packages, including:

* Install / manage a single package;

* Upgrade a software suite;

* Install a security patch

Note:

* dpkg is the basic command for manage packages. It is similar to rpm in Red Hat Linux;

* apt is a smart front end of dpkg. It can solve dependency problems, as well as search, install, upgrade from repositories on the Internet;

* dselect is the interface of dpkg using ncurses. It can also handle dependencies, but it is not so intelligent as apt;

* synaptic is the graphical interface of dselect

## Commands for package management

### dpkg

dpkg usually needs to be run with root permission, so it is common to see commands like "sudo dpkg XXX".

Syntax:

        dpkg [<Options> ...] <Command>
        dpkg -s Package | grep Status  ## See if the package has been installed already
        dpkg -s Package    ## See if the package has been installed, as well as other useful information
        dpkg -S Filename ## Search the package that contains this file
        dpkg -C   ## Search for packages that are damaged
        dpkg -i <package.deb>   ## Install the given package
        dpkg -r Package    ## Delete the package that has already been installed, but keep its configuration files
        dpkg -P Package   ## Delete the package installed, as well as its configuration files
        dpkg -h   ## Get helps for dpkg command
        dpkg -L Package ## List all files contained in the package
        dpkg –force-all –purge Package ## Remove packages that cannot be uninstalled because of dependency problems; this is sometimes risky

### apt

apt is a collection of tools aimed to complete different kinds of tasks:

        apt-get    Automatically solve dependency problems for installing and upgrading
        apt-cache    Query package information in binary caches of apt
        apt-file    Search for files and installing path of a package

Note: In the latest version of apt, users can use "apt" command to replace "apt-get" command.

#### apt-get

        apt-get update ## Update the cache of package list. Usually run after changes made to "/etc/apt/sources.list" or "/etc/apt/preferences"
        apt-get install Package ## Install a new software package
        apt-get remove Package  ## Uninstall a package (while keeping its configurations)
        apt-get --purge remove package_name ## Uninstall a package (and remove its configurations)
        apt-get autoclean  ## Delete cached .deb packages that are not installed
        apt-get clean ##  Delete all cached .deb packages
        apt-get upgrade ## Upgrade all installed packages
        apt-get dist-upgrade ## Upgrade the system
        apt-get autoremove ## Remove all packages that have been installed as dependencies and are no used anymore

#### apt-cache

        apt-cache show Package ## Show package record; similar to "dpkg –print-avail"
        apt-cache search Package ## Search specified packages in the package list
        apt-cache showpkg Package ## Show information of the package
        apt-cache policy Package  ## Show installing status and version of the package
        apt-cache depends Package  ## Show dependencies of the package
        apt-cache rdepends Package  ## Show reverse dependencies of the package, i.e. the packages that require this package
        apt-cache dumpavail Package  ## Print all available packages
        apt-cache pkgnames Package  ## Print names of all packages in the package list

### apt-file

        apt install apt-file  ## Install utility "apt-file"
        apt-file update  ## Update database of package information; needed to be run after installation of apt-file, or each time "apt-get update" is run
        apt-file search  File ## Search for the packages that contains files of given name
        apt-file list Package  ## Show the files contained in the given package

### aptitude

Like apt-get, aptitude is another powerful tool for managing packages. It is a little bit more intelligent in dealing with remaining dependencies when uninstall certain packages.

aptitude needs root permission to run as well, thus you can see commands like "sudo aptitude XXX".

Syntax:

        aptitude [-S File] [-u|-i]
        aptitude [Option] <Action> ...    ## If no action is provided, aptitude will enter interactive mode
        aptitude install <Package>   ## Install / upgrade packages
        aptitude remove <Package>   ## Uninstall package
        aptitude purge <Package>    ## Uninstall package and its configuration files
        aptitude forbid-version   ## Forbid aptitude to upgrade a package to a specified version
        aptitude update  ## Update caches of package list
        aptitude safe-upgrade   ## Do a upgrade safely
        aptitude full-upgrade    ## Do a upgrade that may lead to installation of new packages or remove of installed packages
        aptitude search  ## Search packages using their names or expressions
        aptitude show   ## Show the detailed information of a package
        aptitude clean  ## Delete downloaded .deb packages
        aptitude autoclean  ## Delete downloaded packages that are no longer used
        aptitude download   ##Download .deb packages (without installing it)
        aptitude reinstall    ## Download and re-install a package that has already been installed
        aptitude --help   ## See helps for aptitude

## Related information

### Clean apt cache

When installing software using apt-get, it automatically download files needed from repositories on the Internet, and store them in directory "/var/cache/apt/archives". If you do not need these cached files anymore, you can delete them by executing in terminal:

        sudo apt-get clean

or

        sudo apt-get autoclean

The first command deletes all packages in directory /var/cache/apt/archives/ and /var/cache/apt/archives/partial/ except those that have been locked, while the second command deletes only unused packages and packages of old version.

For example, we have two packages of gimp of different version (2.3 and 2.6), one package of Chrome 28 and one package of Firefox 15  in directory /var/cache/apt/archives. Gimp 2.6 and Chrome 2.8 is currently installed in the system. If we run "sudo apt-get clean", all these cached packages will be removed. If we run "sudo apt-get autoclean", only package of gimp 2.3 and Firefox 15 will be removed.

### Delete software configurations

To reset or completely remove a software, just delete its configuration files. There are several places where configurations are usually stored:

        ~/
        ~/.config
        /usr/share (System-scope configurations)

Note: ~/ is the home folder of a user.

For example, deepin-music-player will generate configurations in the following directory:

        /home/cxbii/.config/deepin-music-player  ## Available only to user"cxbii"
        /usr/share/deepin-music-player  ## Available to all users in the system

If we want to remove configurations while uninstalling programs, add "--purge" option to "apt-get":
    
    sudo apt-get remove --purge package_name

### Uninstall packages

To uninstall specified packages, execute in terminal:

        sudo apt-get remove  package_name

To remove dependencies that are no longer used, execute in terminal:

        sudo apt-get autoremove

This operation can be risky as it may remove packages that are in fact needed by users.

For example, package A depends on B, and B depends one C. Run

        apt-get remove C

will remove A, B and C at a same time, as A and B cannot function normally without C.

Another example is that both C and D depends on B. Run

        apt-get autoremove

in the condition that C and D have already been uninstalled will cause B to be removed, except that B was installed manually in the system. You may also use

        aptitude remove C

to remove B, C, D at a same time.

## Upgrade packages

To upgrade packages, first we need to get the latest list of software packages, which is then used to determine if the installed packages have available updates. If any updates presents, download them to local disk and use them to replace the old ones.

To get latest list of packages, run:

        sudo apt-get update

To download and apply available updates, either run:

        sudo apt-get upgrade

or

        sudo apt-get dist-upgrade

The common point of "upgrade" and "dist-upgrade" is that they all require the package list to be updated before upgrade. 

The difference lies in that the former do not introduce new software or discard existing software (including Linux kernel), while the later may install new packages or delete existing packages according to the changes of dependencies.

For regular security updates for most Linux distributions, it is recommanded to use "apt-get upgrade". If users need to upgrade their system from one old stable version to another one, then "apt dist-upgrade" is suggested.

##Add PPA for using

To use packages from non-official repository, execute in terminal:

```
sudo apt-get install python-software-properties
sudo apt-get install software-properties-common
sudo apt-get update
```

## Common problems

Although apt-get can deal with most of the problems generated during installation, it is still possible that you encounter difficulties in handling certain types of packages. Here we describe several commonly seen problems and discuss the solutions to them.

### dpkg returned an error code

If you see the error in terminal:

        E:Sub-process /usr/bin/dpkg returned an error code (1)

Try to execute:

        cd /var/lib/dpkg
        sudo mv info info.bak
        sudo mkdir info
        sudo dpkg --configure -a
        sudo apt-get install -f
        sudo mv /var/lib/dpkg/info/* /var/lib/dpkg/info.bak
        sudo rm -rf /var/lib/dpkg/info
        sudo mv /var/lib/dpkg/info.bak /var/lib/dpkg/info

### Could not get /var/lib/dpkg/lock

If you see errors from apt-get command:

    E: Could not get lock /var/lib/dpkg/lock - open (11 Resource temporarily unavailable)
    E: Unable to lock the administration directory (/var/lib/dpkg/) is another process using it?

Try one of the following solutions:

The first solution:

Close running programs like apt-get or aptitude. If you do not know the exact program that is using dpkg, please open system monitor to see if there is any process related to apt-get, then kill it. You may also reboot your computer.

The second solution:

Execute in terminal:

        sudo rm /var/cache/apt/archives/lock
        sudo rm /var/lib/dpkg/lock sudo rm /var/lib/apt/lists/lock

It may be useful when the first solution has no effect.

### Fail to download index files

If you see errors from apt-get command: 

        E: Some index files failed to download. They have been ignored, or old ones used instead.

Try one of the following solutions:

The first solution:

Restore the default software source if you have modified it. See [default source for deepin](https://wiki.deepin.org/index.php?title=Software_source).

The second solution:

Please wait for a moment, then try to re-run apt-get. If problem exists, try executing:

        sudo rm /var/lib/apt/lists/partial/*
        sudo apt-get update

### GPG error

If you see errors from apt-get command like:

        W: GPG error: http://apt.tt-solutions.com dapper Release:  The following signatures couldn't be verified because the public key is not available: NO_PUBKEY XXXXXX

Try to execute:

        gpg --keyserver subkeys.pgp.net --recv xxx
        gpg --export --armor xxx | sudo apt-key add -

where "xxx" is the last 8 characters of "XXXXXX".

Or if you are using PPA source, execute:

        sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com

### dpkg interrupted

If you see errors from apt-get command:

    E: dpkg was interrupted you must manually run 'dpkg -- configure - a' to correct.

Try to execute:

        sudo dpkg --configure -a

If this does not help, try:

        sudo rm /var/lib/dpkg/updates/*
        sudo apt-get update
        sudo apt-get upgrade

### Broken packages held

If you see errors from commands:

        E: Unable to correct problems, you have held broken packages.

This is usually caused  by dependency problems. Try to execute:

        sudo apt-get install -f

If this does not help, try:

        sudo dpkg--configure -a

Or, you may remove the packages that cause this problem:

        sudo apt-get remove XXX ## XXX is the package that causes this problem
        sudo apt-get update

### Section with no Package

If you see errors from commands like:

    E:Encountered a section with no Package: header,
    E:Problem with MergeList     /var/lib/apt/lists/archive.canonical.com_dists_maverick_partner_binary-i386_Packages,

Try to execute:

        sudo rm -rf /var/lib/apt/lists/* -vf
        sudo apt-get update

### Cannot install multiple software at a same time

It is because that dpkg cannot safely deal with more than one package at a same time while maintain the correct dependencies. If two dpkg are running concurrently, the database file cannot be locked, making dependencies out of control.

Note: deepin uses dpkg to manage its packages, so the description above may not be true to other distribution that have other kinds of package manager.

### Downgrade packages

Sometimes we need a package of lower version when we already have a higher version, a downgrade of packages is then needed.

Take Firefox as example. We now have Firefox 16.0, but we need a lower version of Firefox to support some extensions. First, we can search in repository for the available version of Firefox:

        $ apt-cache madison firefox
        firefox | 15.0.1+build1-0ubuntu0.12.04.1 | http://packages.linuxdeepin.com/ubuntu/ precise-security/main i386 Packages
        firefox | 15.0.1+build1-0ubuntu0.12.04.1 | http://packages.linuxdeepin.com/ubuntu/ precise-updates/main i386 Packages
        firefox | 11.0+build1-0ubuntu4 | http://packages.linuxdeepin.com/ubuntu/ precise/main i386 Packages
        firefox | 11.0+build1-0ubuntu4 | http://packages.linuxdeepin.com/ubuntu/ precise/main Sources
        firefox | 15.0.1+build1-0ubuntu0.12.04.1 | http://packages.linuxdeepin.com/ubuntu/ precise-security/main Sources
        firefox | 15.0.1+build1-0ubuntu0.12.04.1 | http://packages.linuxdeepin.com/ubuntu/ precise-updates/main Sources

If we would like to downgrade Firefox to 11.0, we can execute:

        sudo apt-get install firefox=11.0+build1-0ubuntu4

The format of the command used is:

        sudo apt-get install Package=Version

where "Package" is the name of that package, and "Version" is the exact version number you would like to downgrade to.

In addition, to prevent package manager to upgrade this package again, execute:

        sudo echo "firefox hold" | sudo dpkg --set-selections

### Making choice in installation process

Sometimes we may need to do choices during the installation of certain packages, like wine. Just press TAB to move the selection to the right option, then press Enter to continue.

### Error when add PPA repository

If you see errors from add-apt-repository command like:

        Traceback (most recent call last):        
        File "/usr/bin/add-apt-repository", line 160, in         
        sp = SoftwareProperties(options=options)        
        File "/usr/lib/python3/dist-        
        packages/softwareproperties/SoftwareProperties.py", line 96, in init        
        self.reload_sourceslist()        
        File "/usr/lib/python3/dist-        
        packages/softwareproperties/SoftwareProperties.py", line 584, in reload_sourceslist        
        self.distro.get_sources(self.sourceslist)         
        File "/usr/lib/python3/dist-packages/aptsources/distro.py", line 87, in get_sources        
        raise NoDistroTemplateException("Error: could not find a "        
        aptsources.distro.NoDistroTemplateException: Error: could not find a distribution template

Execute:

        sudo gedit/usr/share/python-apt/templates/LinuxDeepin.info

And append following content:

        Suite: quantal
        RepositoryType: deb
        BaseURI: http://packages.linuxdeepin.com/deepin/
        MatchURI: packages.linuxdeepin.com
        MirrorsFile-amd64: LinuxDeepin.mirrors
        MirrorsFile-i386: LinuxDeepin.mirrors
        Description: Linux Deepin 12.12 'Quantal'
        Component: main
        CompDescription: Officially supported
        CompDescriptionLong: Deepin-supported Open Source Software
        Component: non-free
        CompDescription: Restricted software
        CompDescriptionLong: Software restricted by copyright or legal issues

Then execute:

        sudo add-apt-repository ppa:realender/XXX
        sudo apt-get update
        sudo apt-get install XXX

### Hash Sum mismatch

If you see errors from "apt-get update" command like:

W:Failed to fetch bzip2:/var/lib/apt/lists/partial/it.archive.ubuntu.com_ubuntu_dists_precise-updates_universe_binary-amd64_Packages  Hash Sum mismatch

This means the downloaded file is corrupted, generally caused by network problems.

Try to execute the following command to get the package list for upgrading:

        sudo apt-get update --print-uris > apt-get-urls.txt

Then download files from those URLs in file "apt-get-urls.txt". It is convenient to use downloadthemall plugin in Firefox to do so, by setting rename mask as "*curl*/*name*.*ext*" (quotes not included). For example, a file downloaded from

    http://www.ubuntu.com/folder/subfolder/filename.gz

will be saved as

    ~/pool/www.ubuntu.com/folder/subfolder/filename.gz

The  downloaded "Release" files, as they have no file extension, will be saved as "Release.txt" by default. It is thus necessary to manually correct the rename mask for them: first soft download list by "resource name", then select all files named "Release" and set rename mask to "*curl*/*name*" (quotes not included). It is sometimes necessary to limit numbers of concurrent download to 1 as well, to prevent errors generated by some mirror servers.

After finishing download, there will be files of whole package list in directory "~/pool" like "packages.deepin.com/deepin/dists/unstable".

Then backup the original /etc/apt/source.list as /etc/apt/source.list.normal, and edit /etc/apt/source.list with text editor (i.e. gedit) to replace the folloing lines:

        deb http://
        deb-src http://

with

        deb file:///home/YourUserName/pool/
        deb-src file:///home/YourUserName/pool/

Next time when you run "sudo apt-get update", it will fetch package list from local files.

For command "sudo apt-get upgrade", you will also need to download related packages manually. First, get a list of needed packages:

        sudo apt-get upgrade --yes --print-uris > ~/pool/apt-get-upgrade.txt

Then replace the prefixes of files listed in "~/pool/apt-get-upgrade.txt" from 

    file:///home/YouUserName/pool/

to

    http://

and download all these URLs using downloadthemall plugin.

To make apt-get find these downloaded packages, execute:

        sudo mount -o bind /home/YouUserName/pool/deb /var/cache/apt/archives

Next time when you run "sudo apt-get upgrade", it will fetch packages from local files.


## References

[APT HOWTO (Obsolete Documentation)](http://www.debian.org/doc/manuals/apt-howto/)

[apt-get的更新; apt-get的升级;和apt-get dist-upgrade时各自的作用](http://www.cnblogs.com/lexus/archive/2011/11/30/2268896.html)

[慎用 apt-get autoremove](http://www.danbaise.com/69.html)

[添加ppa问题解决](https://bbs.deepin.org/forum.php?mod=viewthread&tid=44403&highlight=ppa)

[(基本解决)无法下载bzip2, Hash 校验和不符](http://forum.ubuntu.org.cn/viewtopic.php?p=3012290&sid=be86037ef2bc4d4a8372e4301b84a753#p3012290)

[aptitude 使用快速参考](http://linuxtoy.org/archives/aptitude_quick_reference.html)