---
title: File_permission
description: 
published: true
date: 2022-05-17T04:25:48.936Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:55:31.432Z
---

## Summary

In Linux, each file has three kinds of permission: the first is for its owner, the second is for its group, and the last is for anyone else. These permissions are set independently, and are the core of permission control of Linux system.

## Description of three kinds of permission

### Owner

The owner of a file is generally the creator of this file. He / she can also change its owner by using `chown` command.

### Group

Each Linux user belongs to at least one group. If the owning group of a file contains several person, then they all have the same permission to this file according to its group permission.

### Anyone else

This refers to any other user that is neither its owner or the member of its owning group.

## File and directory permission

### File permission

#### All files

To view file information, type `ls -l` in terminal. For example, to see the information of file "/bin/bash":

         $ ls -l /bin/bash
         -rwxr-xr-x       1         root      wheel      430540    Dec23     18:27    /bin/bash

The meaning of each field is described below.

##### The first field (file type and file permission)

The first character indicates the type of this file. Some common file types are:

         -    Common file
         d    Directory (also seen as a kind of file in Linux)
         l     Link
         b    Block file (special file for device)
         c    Character file (special file for device)
         s    Socket file
         p    Pipe file

In the example above, the character "-" indicates that "/bin/bash" is a common file.

The second to nineth character, from left to right, indicates the permission of its owner, its owning group and anyone else. In the example above:

         rwx ## Owner can: read from (R), write to (W) and execute (X) this file
         r-x  ## Member from its owning group can read from and execute this file, but cannot write to it
         r-x ## Anyone else can read from and execute this file, but cannot write to it

Note: Apart from character form for describing file permission, such as "rwx", it is also possible to use numeric form to do so. The relations of each permission and its numeric form are shown below:

<table class="block1">

    <tbody>
        <tr>
            <td width="40px">Character form</td>
            <td width="40px">Numeric form</td>
        </tr>
      <tr>
            <td width="40px">rwx</td>
            <td width="40px">7</td>
        </tr>
<tr>
            <td width="40px">rw-</td>
            <td width="40px">6</td>
        </tr>
<tr>
            <td width="40px">r-x</td>
            <td width="40px">5</td>
        </tr>
<tr>
            <td width="40px">r--</td>
            <td width="40px">4</td>
        </tr>
<tr>
            <td width="40px">-wx</td>
            <td width="40px">3</td>
        </tr>
<tr>
            <td width="40px">-w-</td>
            <td width="40px">2</td>
        </tr>
<tr>
            <td width="40px">--x</td>
            <td width="40px">1</td>
        </tr>
<tr>
            <td width="40px">---</td>
            <td width="40px">0</td>
        </tr>
    </tbody>

 </table>

Tips for calculating numeric form of permission: for each kind of permission, "r" = 4, "w" = 2, "x"  = 1, "-" = 0. Therefore, "rwxr-xr-x" can be calculated as:

        rwx=4+2+1=7
        r-x=4+0+1=5
        r-x=4+0+1=5

Thus the numeric form for "rwxr-xr-x" is "755".

##### The second field (hard link count)

The second field indicates the count of i-nodes, i.e. count of hard links.

In the example above, "/bin/bash" has only one hard link, thus the second field is "1".

##### The third field (file owner)

The third field indicates the owner of this file.

In the example above, user "root" owns "/bin/bash".

##### The fourth field (owning group)

The third field indicates the group that owns this file.

In the example above, group "wheel" owns "/bin/bash".

##### The fifth field (file size)

The third field indicates the size of  this file. The unit is "byte".

In the example above, "/bin/bash" is 430540 bytes long.

##### The sixth field (modification time

The third field indicates the recent time of modification to  this file. The time is shown in format "Month Day Year", according to the locale settings.

In the example above, "/bin/bash" is last modified in December 23.

##### The seventh field (file or directory name)

The name of the file or directory.

In the example above, the filename of "/bin/bash" is "bash".

#### Executable file

Executable has two extra kinds of permission: SUID and SGID, which stand for "Set User ID" and "Set Group ID". These two permissions only have effect when executable files are executed, so it is of no use to set SUID and SGID for common files.

As it requires advanced knowledge for understanding permission control in UNIX system, the details of SUID and SGID are not introduced here. If you are interested in SUID and SGID, please refer to related articles.

### Directory permission

Although a directory is seen as a kind of file in Linux, there is a little diffference between directory permission and file permission. The first field of file information should be interpreted as:

        r:    Users can list the file in this directory, or view the summary of this directory
        w:  Users can create, delete or modify the files / subdirectories in this directory
        x:    Users can switch to this directory using "cd" command
        -:    No permission

If a user want to enter a directory to see its content, the read (R) and the executing (X) permission must be granted.

## Management of file permission

### Using graphical interfaces

For example, to view any file in filesystem, type in terminal:

         sudo nautilus

This will open file manager Nautilus **with root permission**. You can use right context menu to change the permission of files and directories. Be careful with the powerful root permission!

### Use command

#### Change owner and owning group

To change the onwer or the owning group of a file, use `chown`  or `chgrp`.

Example:

        sudo chown root /etc/passwd ## Change the owner of "/etc/passwd" to "root"
        sudo chgrp wheel /etc/passwd ## Change the owning group of "/etc/passwd" to "wheel"

chown can be used to change owner and owning group at a same time, using ":" to seperate owner and owning group:

        sudo chown root:wheel /etc/passwd ## CChange the owner of "/etc/passwd" to "root", and change the owning group to "wheel"

#### Change owner recursively

Use "-R" option in chown or chgrp to change the ownership of whole directory tree.

Example:

        sudo chown -R root /home/drobbins ## Change the owner of all files under "/home/drobbins" to "root"
        sudo chgrp -R wheel /home/drobbins ## Change the owning group of all files under "/home/drobbins" to "wheel"

### Change file permission

#### Modify file permission

Use `chmod` to change file and directory permission. Both character form and numeric form expression can be used.

Syntax:

         chmod [-cfvR] [--help] [--version] Permission FILES

Use following flags to specify the range of the given permission:

        u：Grant this permission to file's owner
        g：Grant this permission to file's owning group
        o：Grant this permission to any other person
        a：Grant this permission to all users (implies "u", "g" and "o")

The expression of permission is similar to the description above:

        r:     Read permission, equals to "4"
        w:    Write permission, equals to "2"
        x:    Executing permission, equals to "1"
        -:    No permission, equals to "0"
        s:    Used by SUID and SGID
        t:    Sticky bit. Used for directory only

For more details about chmod, type in terminal `man chmod`.

Some examples:

Example 1

        chmod ugo+r file1.txt ## Set file "file1.txt" readable to everyone
 
       chmod a+r file1.txt　 ## Set file "file1.txt" readable to everyone
    
        chmod ug+w,o-w file1.txt file2.txt ## Set file "file1.txt" and "file2.txt" writable to their owners and members from their onwning group, and unwritable to anyone else

        chmod u+x ex1　 　## Set file "ex1" executable to its onwer only

        chmod -R a+r *　　 ## Set all files and subdirectories under current directory readable to everyone

        chmod u+s sqlplus　　## Set the identity of  user executing "sqlplus" to "oracle" temporarily (SUID)

Example 2

        chmod a=rwx file ## Equals to "chmod 777 file"
        chmod ug=rwx,o=x file ## Equals to "chmod 771 file"
        chmod 4755 filename ## Equals to "chmod u+s,a+x filename"

Example 3

Type in terminial:

        cd /media/amasun/java/develop/array
        chmod 777 ./

This will make directory "/media/amasun/java/develop/array" readable, writable and executable to everyone.

#### Reset file permission

Use "=" operator in chmod command to set specified permssion while revoking unwanted permission.

Example 1

        chmod =rx scriptfile.sh ## Set file "scriptfile.sh" readable and executable to everyone, but not writable

        chmod u=rx scriptfile.sh ## Set file "scriptfile.sh" readable and executable to its owner, but not writable to its owner

Example 2

        $ chmod 0755 scriptfile.sh ## Equals to "chmod u=rwx,go=rx scriptfile.sh"
        $ ls -l scriptfile.sh
        -rwxr-xr-x 1 drobbins drobbins 0 Jan 9 17:44 scriptfile.sh

Here we use numeric form "0755" for permission "rwxr-xr-x".

## Default file permission of system

### umask

When a process create a new file, it also grants a permission for this file. This permission is usually 0666 (readable and writable to everyone), thus it is not secure. However, Linux provides also a mechanism called umask, which can be used to specify the default permission of new file. Use `umask` command to see current umask settings:

        $ umask 
        0022

The default setting for umask is 0022: the first digit is used for special permissions, and the second to fourth digits represents permission mask for owner, owning group and anyone else.

For example, using default umask 0022:

* The highest permission of files created is 0666, substract it with 0022, we get 0666 - 0022 = 0644, thus the default permission of files created is 0644

* The highest permission of directories created is 1777, substract it with 0022, we get 1777 - 0022 = 0755, thus the default permission of files created is 1755

Note: When creating files, they do not own executable permission, thus the highest permission is "-rw-rw-rw" (0666); as for directories, they need to be accessed by all users, thus the highest permission is "rwxrwxrwx" (1777).

### umask setting

To improve the security of files created, we can change umask to 0077 by typing in terminal:

        umask 0077

This will change the umask for directories to 0077 and umask for files to 0066. After that, the default permission is 1700 for directories and 0600 for files.

## Reference

[鸟哥的 Linux 私房菜 -- Linux 的文件权限与目录配置](http://vbird.dic.ksu.edu.tw/linux_basic/0210filepermission_2.php)

[Gentoo百科:The Linux permissions model](http://www.gentoo.org/doc/en/articles/lpi-101-intermediate-p3.xml#doc_chap3)

[关于UNIX和Linux系统下SUID、SGID的解析](http://www.linuxeden.com/html/unix/20071031/36892.html)
