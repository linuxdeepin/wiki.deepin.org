---
title: File_and_directory
description: 
published: true
date: 2022-05-07T02:29:33.606Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:55:27.322Z
---

## Summary

This entry gives bref introduction to file and directory management in Linux system.

## File

File: A chunk of data stored in persistent or temporary storage device, and managed by file system. There are mainly two types of files:

Text file: consists of lines of strings, generally readable;

Binary file: the file that does not belongs to text file; it usually contains bytes that are not easily understood by human.

### File extension

File extension is used by some operating system to mark the type of certian files. A extension generally comes in the end of a file name, and there is a seperator between the body part of the file name and the extension. For example: "readme.txt" contains two part: "readme" as its body and "txt" as its extension, with which "readme.txt" will be recognized as a text file by system.

Note: In Linux, file extension is used only to describe file association. It has nothing to do with the executable permission of files. So long as the file has executable ("x") permission, it is executable to user, though not all file can be executed successfully. Note also that in Linux file extension is generally case-insensitive to the program that deal with it, while the body part of that file name is case-sensitive.

### Rules for file naming

When naming a file in Linux, avoid to use following characters:

 ＊？<>；＆！［ ］｜\ ' "（ ） ｛ ｝

The name of each file or directory can have up to 255 characters,  and the complete path for that file can have up to 4096 characters.

You can use TAB key to auto complete your input, while eliminating typos in commands or file names.

All hidden file have an dot ('.') in the beginning of its file name. For example, ".linux.txt" is a hidden file.

### Commonly used file types

#### Package file

##### DEB

DEB is the format of Debian package file. Its file extension is "deb".

DEB use standard archive format compatible to Unix ar. It is built by dpkg, by compressing with gzip and packed with tar. You can also use Alien utility to convert DEB to other package format.

To install a DEB package, just double click on the package file. As deepin is based on Debian, users can fully benefit from the convenience provided by DEB package!

##### RPM

The RPM Package Manager (RPM) is another wildly used in Linux. It is early developped by Read Hat, then handled by open source community. RPM is generally a part of Linux distribution, though it can also be a independent application (as in Gentoo). RPM is suitable only to management of RPM-packed software.

To install a RPM package, just double click on the package file. Only system using RPM to manage its software accepts installation from RPM package file.

### Archive file

Archive file usually has extension "zip", "gz", "xz", "7z", "rar", etc. You may need special software to extract it, or substract files from it.

### Library file

There are mainly two kinds of library in Linux: static link library and share library. The former has name "libXXX.a",  and the later has name "libXXX.so", while "XXX" is the name of this library. There may also be version number appended to the file extension, like "libXXX.so.1.0", indicator that the major version of this library is 1 and the minor version is 0.

### Script file

"sh" is the file extension of shell script (or batch script). It is  similar to ".bat" file in Windows, but much more powerful!

## Path and directory

### Path

Path: The directory chain pointed to the directory needed by user. There are two types of representation of path:

* Absolute path: the pat is always conducted by a slash ("/"). Example: /usr/share/doc.
* Relative path: paths that do not begin with "/". Example: "../doc" can be used to point to "/usr/share/doc" when current working directory is "/usr/share/man".

### Directory

Directory: The indexing of file created by operating system to describe the structure of files storage and to facilitate file management.

A "directory" (or "folder") is a virtual container for files in file system, where other files or directories can be placed. A typical file system may contains thousands of directories, each includes several files or subdirectories. In this way, a tree of directories is formed, making structured file storage possible.

Each file in directories has its own entry, so that information like its name, type, internal identifier, storage address, length, access permission and access time are recorded.

## File Management

### Using graphical interface

The file manager, like nautiluis, dolphin, pcmanfm, thunar, is installed in system by default and is powerful enough for ordinary file management. Other operations, like compressing, extracting and packing.

### Using command

#### cd

Use `cd DIR` to switch working directory to directory "DIR". Here "DIR" can be written as absolute path "/home/sun/downloads", or relative path "./downloads". If directory name is omitted, then switch to home directory (login directory).

Use "~" to represent home directory, "." to  current directory and ".." to parent directory.

Example:

    cd /usr/bin  # Enter directory "/usr/bin/"
    cd .. # Back to the partent directory of current directory
    cd - # Back to the last directory
    cd ~ # Enter the home directory of current user

#### pwd

Use `pwd` to print the absolute path of current (active) directory.

Example:

    pwd　 # Show current working directory

The default working directory is "~", i.e. home directory.

#### mkdir

Use `mkdir` to create an empty directory.

Example:

    mkdir DIR # Create empty directory "DIR" under current directory

#### rmdir

Use `rmdir` to remove an empty directory. If the directory is not empty and no extra argument is given, it will show an error.

Example:

    rmdir DIR　# Remove empty directory "DIR"

#### ls

Use `ls` to show the content of directory

Example:

    ls # List files in current directory (hidden file not shown)
    ls -a # List files in current directory (hidden file also shown)
    ls -l #  List files and their details in current directory
    ls -la #  List files and their details  in current directory (hidden file also shown)

#### cp

Use `cp` to copy file.

Example:

    cp FILE1 FILE2 # Copy from FILE1 to FILE2
    cp test.text test2.text # Copy test.text to test2.text under the same directory
    cp test.text /home/sun/ # Copy test.text into directory /home/sun, keeping its file name

#### mv

Use `mv` to move or rename file or directory.

Example:

    mv FILE1 FILE2 # Move / rename file FILE1 to FILE2

#### touch

Use `touch` to modify the modification time of file.

Example:

    touch FILE　　# If FILE does not exist, then create new empty file named "FILE"; otherwise, update modification time of "FILE" to the curent time

#### more

Use `more` to read files on screen.

Example:

    more FILE　　# Show the content of FILE. If it is not a text file, please use `bvi`, instead of `more`, to display the hexadecimal data

#### head

Use `head` to show the first few lines of one file.

Example:

    head FILE　　# Display the first 10 lines of file "FILE"
    head -n 30 FILE  # Display the first 30 lines of file "FILE"

#### tail

Use `tail` to show the last few lines of one file.

Example:

     tail FILE　　# Display the last 10 lines of file "FILE"
     tail -f FILE　　# Display the last 10 lines of file "FILE"; if file content is changed, update display accordingly

#### rm

Use `rm` to remove file or directory

Example:

     rm <FILE>　　# Delete file "FILE" under current directory
     rm -rf <DIR>　# Delete directory "DIR" recursively, i.e. remove "DIR" and all its content

#### tar

Use `tar` to pack files.

Example:

    tar cf FILE.tar FILES # Pack "FILES" into "FILE.tar" without compressing
    tar xf FILE.tar　# Unpack package "FILE.tar"
    tar czf FILE.tar.gz FILES　# Pack "FILES" into "FILE.tar.gz" with gzip compressing
    tar xzf FILE.tar.gz　# Unpack package "FILE.tar.gz"
    tar cjf FILE.tar.bz2 FILES　# Pack "FILE" into "FILE.tar.bz2" with bzip compressing, which are smaller than gzip package
    tar xjf FILE.tar.bz2　# Unpack package "FILE.tar.bz2"

#### gizp

Use `gizp` to compress files

Example:

     gzip FILE　# Compress "FILE" using gzip
     gzip -d FILE.gz　# Decompress "FILE.gz"

#### find

Use `find` to find for files and directories.

Example::

    find DIR -name FILENAME # Find specified "FILENAME" in current directory
    find / -name xorg.conf  # Find file named "xorg.conf" in whole file system (/)
    find /etc -name xorg.conf* # Find under /etc files or directories whose names begin with "xorg.conf"

#### ln

Use `ln` to craete links between files.

Example:

    ln -s FILE LINK # Create soft link "LINK" as a link pointing to FILE. Soft link is like a "shortcut", and becomes invalid when the source file disapperes
    ln FILE LINK # Create hard link "LINK" to "FILE". Hard liink is like a "synonym". It won't become invalid when "FILE" is removed or moved away. It cannot be created across partitions.


#### grep

Use `grep` to search for files.

Example:

    grep PATTERN FILES # Search specified "PATTEN" in file "FILES"
    grep -r PATTERN DIR # Search  specified "PATTEN" recursively in directory "DIR"
    COMMAND | grep PATTERN # Search  specified "PATTEN"  in the output of command "COMMAND"
    lspci | grep VGA   # Search lines containing "VGA" in the output of "lspic"

#### locate

Use `locate` to search for files in database.

Example:

    locate FILE　#Search files named "FILE"

"locate" query file info in special database, thus it can find files quickly. However, the database is updated every 24 hours, so users may not find the latest files.

#### whereis

Use `whereis` to locate special files.

Example:

     whereis BIN　# Search in directories from $PATH and show the path of file "BIN".
     whereis whereis # Show the path of program "whereis"

## References

[维基百科:DEB](http://zh.wikipedia.org/wiki/Deb)

[维基百科:RPM](http://zh.wikipedia.org/wiki/RPM%E5%A5%97%E4%BB%B6%E7%AE%A1%E7%90%86%E5%93%A1)

[51CTO:简要概括常见Linux文件扩展名](http://os.51cto.com/art/200910/157556.htm)

[http://linux-wiki.cn：动态库(.so)](http://linux-wiki.cn/wiki/zh-hans/%E5%8A%A8%E6%80%81%E5%BA%93(.so))

[OKLinux：关于Linux系统文件扩展名含义的介绍](http://www.oklinux.cn/html/Basic/jcrm/20080512/53502.html)

[鸟哥私房菜:文件目录管理](http://vbird.dic.ksu.edu.tw/linux_basic/0220filemanager.php#cd)

[鸟哥私房菜:文件压缩和打包](http://vbird.dic.ksu.edu.tw/linux_basic/0240tarcompress.php#dd)