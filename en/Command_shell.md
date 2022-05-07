---
title: Command_shell
description: 
published: true
date: 2022-05-07T02:28:32.001Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:54:06.279Z
---

## Summary

Shell is the main interface of text-based operating system. It is also the surface of an operating system, which manages the interaction between users and the OS: waiting for input, inteprete the input and output the result from the OS.

Shell provides methods of communications, which can be either interactive (like keyboard input) or non-interactive (like the shell script, a commbinations of commands).

Shell is in fact a command intepretor, receives commands (such as "ls", "chown" and "fsck") from users, then executes the relative program. 

## Types of shell

Most Linux supports more than one shell. To view all supported shells in deepin, execute in terminal:

    $ cat /etc/shells
    # /etc/shells: valid login shells
    /bin/sh
    /bin/dash
    /bin/bash
    /bin/rbash

Each available shell has its own features, while the most popular shell is bash.

### bash

Executable: /bin/bash

Features:

1. Developped by the Free Software Foundation (FSF) according to the POSIX standards

2. It is the default shell of many Linux distribution, supporting command history and alias

3. Powerful functions supported in shell script

### ksh

Executable: /usr/bin/ksh

Feature: It is a advanced extension of Bsh. The default prompt string is "$".

## Functions of shells

Some basic functions of command shells are briefly introduced below.

### Command completion

As command shell is operated by commands, users will spend a lot of time on typing. Some longer commands may be difficult to remember and use, so the shell provides a completion for the remaining part of a command after users have provided the beginning part of it. You can press Tab key to trigger auto completion.

### Path completion

Similar to command completion, users can input the beginning part of a path, press the Tab key to complete the remaining part of that path.

### Command history

To use the command that has been previously typed, press Up and Down key to view the command in history, then modify it or press Enter to directly run it. You may also use command "history" to show all previous command:

    history [-c | -n]

Options:

* -c: Clear command history
* -n: List the first n commands in history

Each time a user exits the shell, the later will update the command history in file ~/.bash_history, so users can also view the commands executed by checking this file. Thus a password may leak when it is typed directly in command line, for example when users are trying to start connections or services.

- Command alias

To simplify long commands that are frequently used, replace them with short alias. Shell provides command "alias" to define long commands as shorter ones, which is like the definition of variables.

Syntax:

    alias Alias=“A long command”

Note that there is no space around the "=", and the command should be wrapped by qutoes for all parameters to be correctly parsed.

For example, to start Apache (WWW) server that has an executable /usr/src/apache/bin, define an alias for it:

    alias  apache=’/usr/src/apache/bin/apachectl start’

Use

    alias

to see if it has been set correctly. After that, you can use command

    apache

to start Apache server. The result is identical to command “/usr/src/apache/bin/apachectl start”.

To make this alias a permanent one, write the definition into user configuration files in their home directory (generally ~/.bashrc), so that users can use this alias every time when they log in.

deepin has pre-configured some useful alias for users. For example, when execute command "ls" in terminal, files and directories are presented in colors. This is because the alias defined in ~/.bashrc:

    alias ls=’ls –color=auto’

To cancel a command alias, use command:

    unalias SomeAlias

You can check it by executing command "alias" again. To permanently remove this alias, remove or comment out the related lines in configuration files.

## Special characters:

Special characters used in command shells are:

         # ; ;; . , / \ 'string'| ! $ ${} $? $$ $*
         string"* ** ? : ^ $# $@ `command`{} [] [[]] () (()) || &&
         {xx,yy,zz,...}~ ~+ ~- & \<...\> + - %= == != 

### Hashtag (comments)

This is a character that can be seen in almost every shell scripts. Beside from the first line

           #!/bin/bash

hashtags may also appear at the beginning of a line, indicate that this line is a comment and will not be executed.

          # This line is comments.
         echo "a = $a" #a = 0

Thus you can use hashtags to cancel lines that you do not to execute during debugging:

          #echo "a = $a" # a = 0

When quoted or used in directives, or led by a backslash, an hashtag loses its function of commenting and becomes a normal character.

### ~ (Home direcotry)

It represents the home directory of a certain user. When used independently, the current user is implied; other wise the user name followed "~" is taken into concerns. For example: "~user" means "/home/user", "~root" means "/root".

Two special cases:

 ~+    Current working directory, similar to command `pwd`.

    $ echo ~+
    /var/log

~-    Last working directory.

    $ echo ~-
    /etc/httpd/logs

###  ; (Command separator)

The semicolon servers as the associator of commands in shell. An example:

          cd ~/backup ; mkdir startup ;cp ~/.* startup/.

### ;;  (Terminator)

The double semicolon servers as the terminator in options in "case" block.

    case "$fop" in
         help)
            echo "Usage: Command -help -version filename"
            ;;
        version)
             echo "version 0.1"
            ;;
    esac

### . (diirectory or special file)

In Linux shell, one dot represents the current directory, and two dot represent the parent directory. For example:

          CDPATH=.:~:/home:/home/web:/var:/usr/local

The dot after the "=" means the current directory.

Besides, if a file has the name begins with a dot, then this file is a special one that can only be shown with option "-a" in command "ls". 

In regular expressions, a dot can represent a single character except\n".

### ' (single quotes)

Single quotes are used to specify a single string. Any "$" in the string is seen as common characters, so that no variable replacedment occur.

    $ heyyou=home
    $ echo '$heyyou'

In the example above, the output is "$heyyou", rather than "home".

### " (double quotes)

Double quotes allow variable substitution a single string, but do not allow wildcard extension.

    $ heyyou=home
    $ echo "$heyyou"

In the example above,  the output is "home".

### `(backticks)

The backticks are used to specify a command string that is to be run.

    $ fdv=`date +%F`
    $ echo "Today $fdv"

The "date" command in the first line is seen as a command, and the result is given back to variable "fdv".

### , (comma)

Commas are used as "seperators" in operations:

    #!/bin/bash
    let "t1 = ((a = 5 + 3, b = 7 - 1, c = 15 / 3))"
    echo "t1= $t1, a = $a, b = $b"

### /(forward slash)

When used in path, "/" is the seperator of different levels of directory.

    cd /etc/rc.d
    cd ../..
    cd /

A single "/" represents the root directory.

In arithmetic, "/" is the sign of division.

          let "num1 = ((a = 10 / 2, b = 25 / 5))"

### \ (backslash)

The backslash servers as the escape character in interactive mode. When it is put in front of commands, any aliases will be canceld; when put  inf ront of special characters, the later become normal characters; when put after commands, then text of the next line is seen as the continuation of the current line.

    $ type rm
    rm is aliased to `rm -i'
    $ \rm ./*.log

In the example above, command "rm" is escaped by leading backslash, so it is reduced to the original "rm" without option "-i".

    $ bkdir=/home
    $ echo "Backup dir, \$bkdir = $bkdir"
    Backup dir,$bkdir = /home

In the example above, the "$" in "$bkdir" is reduced to a normal character, so the first "$bkdir" gives its literal content, while the second gives the value of variable "bkdir".

### |  (pipeline)

Pipeline is a basic and important concept in Unix system. It connects the standard output of the last command with the standard input of the next command.

          who | wc -l

It is often used to simplify commands in scripts.

### ! (negate or reverse)

It usually represents the negative logic. In condition expression, "!=" means "not equals to".

    if  [ "$?" != 0 ]; then
        echo "Executes error"
        exit 1
    fi

In regular expression, it servers the negative logic. For example:

          ls a[!0-9]

will show all files except a0, a1 .... a9.

### : (colon)

The colon is a built-in directive that means "doing nothing" while the return code is 0.

         $ :
         echo $? ## The output should be "0"
         $ : > f.$$

The third line above equals to

    cat /dev/null >f.$$

while shorten the command and improve the efficiency.

Some times we may see commands like:

          : ${HOSTNAME?} ${USER?} ${MAIL?}

This is to check whether the environmental variables have been set, and show errors when one of them are not set. You may also use command "test" or "if" to do so, but the simplicity and the efficiency are lost.

Another case where colons are mandatory:

          PATH=$PATH:$HOME/fbin:$HOME/fperl:/usr/local/mozilla

This set path list for PATH variable (or any other variables of this kind) in the user configuration file.

###  ? (wild card)

The question mark can be used in file name expansion to represent any single character except the null character ("\0").

    $ ls a?
    a1

Use question marks to match the exact files you want.

### * (wild card)

The asterisk is used to match any single character, including the null character.

    $ ls a*
    a a1 access_log

In mathematical operations, a single asterisk servers as the sign of multiplication.

          let "fmult=2*3"

Note: Besides from build-in command " let", you may use command "expr" to calculate an expression, where asterisks must be escaped with leading backslashes.

Double asterisk mean "power“ in mathematical operations.


    let "sus=2**3"
    echo "sus = $sus" ## sus = 8

### $ (dollar sign) 

It is used in the variable substitution.

    vrs=123
    echo "vrs = $vrs"  ## vrs = 123

Apart from that,  "$" matches an "end-of-line" in a regular expression, which is commonly used in grep, sed, awk and vim (vi).

### ${} (Regular expression of variables)

Bash defines multiple usage of "${}". Some of them are list below.

           ${parameter:-word}   ${parameter:=word}   ${parameter:?word}   ${parameter:+word}     ${parameter:offset}   ${parameter:offset:length}   ${!prefix*}                              ${#parameter}   ${parameter#word}   ${parameter##word}   ${parameter%word}   ${parameter%%word}   ${parameter/pattern/string}                     ${parameter//pattern/string}

### $* (parameters reference)

"$*" is used to refer the parameters passed by caller function, where "*" can be a number (1, 2, 3 ...):

          $*, $0, $1, $2, $3, $4, $5, $6, $7, $8, $9, ${10}, ${11}.....

If the number is greater than 9, then a pair of braces is required. 

"$*" can also be used directly to refer to the string that contains  all parameters. You may need a pair of quote to wrap it:

          echo "$*"

### $@

$@ has the similar function of $*, except that all parameters are given in an "array", not a single string.

### $#

"$#" tells the total number of the parameters passed by caller function.

          echo "$#"

### $? (status variable)

Generally, a Unix process ends with the system call exit(), which return a number as its return code (exit status) that can be collected by parent process to check its status. A return code of 0 usually means a execution failure, and 1 means success.

    tar cvfz dfbackup.tar.gz /home/user > /dev/null
    echo"$?"

### $$ (process ID)

Processes are designed to work independently and should avoid conflict in using of resource. Sometimes a script needs to generate temporary files for storing data, so it have to generate a unique file name for each process that execute this script. A solution is to generate dynamic file names according to the process ID which is unique to each process, with the help of command "$$".

    echo "$HOSTNAME, $USER, $MAIL" > ftmp.$$

Including it in the file name can avoid the condition that the file generated by one process is overwritten by that of another process. This however, does not guarantee the safety of the file after the process exits, as the PID is recycled by system and may be used again by another process.

### ( ) (command group)

A command group is wrapped in a pair of parentheses, like

    (cd ~ ; vcgh=`pwd` ; echo $vcgh)

When parsing a command group, shell opens a new subshell to execute it, so that all variables defined in the command group is confined within this group. For example:

    #!/bin/bash
    a=fsh
    (a=incg ; echo -e "\n $a \n")
    echo $a

The output of this script:

    incg
    fsh

Apart from definition of command groups, parentheses can also be used for definition of arrays or mathematical expressions.

### (( )) (mathematical operations)

Similar to command "let", it is used to calculate an expression. It is also a built-in function of shell, providing more efficiency than "let".

    #!/bin/bash
    (( a = 10 ))
    echo -e "inital value, a = $a\n"
    (( a++))
    echo "after a++, a = $a"

### { } (block of code)

It may appear in script where several commands are separated by semicolons:

    #!/bin/bash
    a=fsh
    {
        a=inbc ;
        echo -e "\n $a \n"
    }
    echo $a

The output of this script:

    inbc
    inbc

The code blocks look like the command groups mentioned above, but they are executed in the current shell, rather in new subshells. Code blocks are also used in definitions of functions, and a code block without a function name is in fact a anonymous function. This can be useful in script programming, as it reduces the complexity of redirection of input and output.

Another kind of usage of braces:

    {xx,yy,zz,...}

This creates a combination of strings. For exmaple:

    mkdir {userA,userB,userC}-{home,bin,data}

we will get new directories named

    userA-home
    userA-bin
    userA-data
    userB-home
    userB-bin
    userB-data
    userC-home
    userC-bin
    userC-data

The above directives are widely used in command shell and scripts, as they can provide both simplicity and efficiency. Like the following example:

    chown root /usr/{ucb/{ex,edit},lib/{ex?.?*,how_ex}}

### [ ] (brackets)

Brackets are generally used to wrap condition expressions in flow control

    if [ "$?" != 0 ]; then
        echo "Executes error"
        exit 1
    fi

They are also used to represent a "range" or a "collection":

          rm -r 200[1234]

In the example above, files named 2001, 2002, 2003, 2004 ... will be deleted.

### [[ ]] (double brackets)

Similar to brackets, with extra parsing of logical operators like || and &&.

    #!/bin/bash
    read ak
    if [[ $ak > 5 || $ak< 9 ]]; then
        echo $ak
    fi

### || and &&

Used to represent logical OR and logical AND.

### & (move to background)

A single "&" at the end of a command will make this command run in background.

          tar cvfz data.tar.gz data > /dev/null &

### \<...\> (word boundary)

It is used in regular expression to define the word boundaries. For example, to search for word "the", we may use command

    grep the Files

Unfortunately, this expression will also match the word "there", as "the" is a part of "there". To avoid this, use command

    grep '\' Files

where the boundary of word "the" is limited by the backslash.

### + (addition)

"+" means additions in mathematical expressions.

          expr 1 + 2 + 3

When used in regular expression, it means the string before "+" are repeated more than one time:

    $ grep '10\+9' Files
    109
    1009
    100009
    100009
    31010009

Note that there must be an backslash in front of "+".

### - (substraction, stdin or directory)

"+" means substractions in mathematical expressions.

          expr 10 - 2

It also indicates the option in a command:

          ls -expr 10 - 2

Some GNU programs use a standalone "-" to  refer to standard input.
       
          tar xpvf -

This tells "tar" to read file content from standard input (stdin).

When used in "cd" command, it represents the last working directory.

          cd -

### % (modulo)

"+" means modulos in mathematical expressions.

    expr 10 % 2

Besides, it is used in regular expression of variables:

  ${Parameter%Word}
  ${Parameter%%Word}

The first line refers to a shortest match of "Word" by "Parameter", and the second line refers to a longest match of "Word".

### = (assignment)

Used in value assignment.

    var a=123
    echo " vara = $vara" 

### == (equals)

Used in condition expression. It means "equals to".

    if [ $vara == $varb ] ...

 ### != (not equals)

Used in condition expression. It means "does not equal to".

    if [ $vara != $varb ] ...

### ^ (beginning of expression)

Used in regular expression to represent "the beginning", or in conditional expression to represent logical NOT (like "!").

### redirection of input/output

 >      >>   <   <<   :>   &>   2&>   2<>>&   >&2   

Bash use a number (starting from 0) called file descriptor to represent all files that have been opened. Commonly used file descriptors are:

| File descriptor | Name | Alias | Default device |
| :------: | :------: |  :------: |  :------: |
| 0 | Standard input | stdin | keyboard |
| 1 | Standard output | stdout | screen |
| 2 | Standard error output | stderr | screen |

When "<" or ">" is used independently, "0<" or "1>" is implied accordingly.

#### cmd > file

Redirect the output of "cmd" to "file. If "file" exists, then clear it first. You can set "noclobber" option in bash to avoid overwrite existing files.

#### cmd >> file

Redirect the output of "cmd" to "file". If "file" exists. append the output to it.

#### cmd < file

Read the content of "file" for "cmd" to use.

#### cmd << text

Read from command line, until another line containing "text" (the delimiter) is met. Use quotes to avoid any variable substitution. If "text" is replaced by "-text", then the delimiter is read from the first line of input with tabulators stripped.

#### cmd <<< word

Feed "cmd" with "word" and the following line break.

#### cmd <> file

Redirect "file" to the input of "cmd", while keeping file not destroyed. This is meaingful only when the command utilize this feature.

#### cmd >| file

Similar to ">", but force overwrite "file" even if "noclobber" option is set. Note that ">|" is the standard form, though csh supports another form of ">!".

#### > file

Truncate "file" to zero size. If "file" does not exist, then create one (similar to command "touch").

#### cmd >&n

Redirect the output of "cmd" to file descriptor "n".

#### cmd m>&n

Redirect the file descriptor "m" of "cmd" to file descriptor "n".

#### cmd >&-

Close the standard input.

#### cmd <&n

Use file descriptor "n" as the input of "cmd".

#### cmd m<&n

Use file descriptor "n" as the "m" for "cmd".

#### cmd <&-

Close the standard output.

#### cmd <&n-

Use file descriptor "n" as the standard input for "cmd", then close "n" for the current shell.

#### cmd >&n-

Use file descriptor "n" as the standard output for "cmd", then close "n" for the current shell.

Note: `>&` causes the duplication of file descriptors, thus the command

    cmd > file 2>&1

is not identical to 

    cmd 2>&1 >file

## Variables

The next part of the entry gives introduction to an important part of shell: the variable.

Variables generally refer to the parameters used to define the environment where the operating system is running, such as the location of temporary directories and system directories.

### Types of variables

There are several types of variables in shell command:

* Local variable: variables that take effect in a environment, usually within the shell that set them.

* Global environment variable: available everywhere in the system.

* Position variable: used to record values of the command and its options, usually read-only.

* Variables in special forms: used to record some special values, usually read-only.

### Types of shells

Shells can be classified into login shell and non-login shell. A login shell is the shell obtained after a successful login, whil a non-login shell refers to any shell opened by a login shell. For example, execute in terminal:

          bash

will give you another shell, which are executed in a existing shell (/bin/bash). All commands executed in the current shell have no relation with the previous shell. To return to the previous shell, type

          exit

As the new shell opened in the previous shell inherits the working environment (including working directory and permissions), we called the new shell a "child shell" and the previous shell a "parent shell".

From this, we can conclude that:

* The login shell is unique while the non-login shells are not

* Any shell can be the parent shell of another non-login shell

* A login shell can never be a child shell

### The difference between login shells and non-login shells

Before discussing, we have to first understand the configuration files used by bash (as well as other types of shell).

* ./etc/bash.bashrc: Configuration file of system environment, defining environment variables and the command prompt

* /etc/profile: Configurations for startup scripts, setting environment variables for system

* ~/.bashrc: Configuration file of the current user, customizing the environment for the user 

* ~/. profile: Script for initializing the environment for current user

It is clear that the first and the second configuration files target all users in system, while the third and the fourth are available only to the current user. The shell will first read the system-wide configurations for every user, then read the user-customized configurations for each user.

For a login shell, it reads configurations in /etc, then reads configurations in ~/.bashrc and ~/.profile. For a non-login shell, it reads only /etc/bash.bashrc and ~/.bashrc.

The following example show the differences between login shell and non-login shell. First append the following line in file /etc/bash.bashrc:

          alias cls=’clear”

Save changes, then execute in terminal:

          cls

You will find the alias does not take effect. This is due to that the login shell has already read the configurations during login.

Try to open another non-login shell, and execute

          cls

You will find the alias work. This is because that non-login shells always read configurations in /etc/bash.bashrc.

Next, remove the alias in /etc/bash.bashrc and add a same definition in /etc/profile. You will find neither login shell nor non-login shell can provide this alias.

Thus, the difference between a login shell and a non-login shell is that they read different sets of configuration files when they are started.

### Define and use a variable

Though system variables and local variables have different scopes, they can be defined in a same way.

#### Definition

To define a variable "name" with value "ubuntu", type in terminal:

          name=”ubuntu”

Now we have got a variable "name". It is worth noting that:

* No space or tabulator is allowed around the equal sign

* The name of the variable consists only alphabets, numbers and underlines, and can not begin with a number

* Generally, variables defined by users use lower-case letters, while system variables use upper-case letter.

#### Usage

After we have defined a variable, we can use command "echo" to display its value:

    $ echo $name
     ubuntu

"echo" is used to display the value of a variable or a string, and the "$" character in front of the variable name means "getting the value of". Thus the value of the variable "name" will fetched out and then sent to screen by command "echo".

#### Exporting

By default, all defined variables are local variable with the scope of the current shell. To make them available elsewhere (as in the subshells), we need to "export" these variables.

The "export" command makes the variable defined in the current shell "visible" to all subshells. For example:

    export name=”ubuntu”

Then we can use variable "name" in a subshell:

    $ bash
    $ echo $name
    ubuntu

Note: The "exportation" is one-way, i.e. the exported variables in subshell can not be used in the parent shell.

#### Unsetting

To release the memory used by an unused variable, use "unset" command:

    unset name

You can verify it again by using

    echo $name


### Position variable

Many shell commands have the format of

    command [options] argument1 argument2 ...

To distinguish each option and argument passed by the caller, shell use position variables to record their positions and the name of the executable file. The names and the values of these variables are read-only.

| Variable | Description |
| :------: | :------ |
| $0 | Executable name of the program run by user |
| $(1-9) | The first to the nineth arguments passed to the executable |
| ${a number more than 9} | Same as $(1-9), except the number has to be wrapped by braces |

### Variables in special forms

Besides the position variable, shell provides many other variables in special forms. All of them consist of "$" and a single character, and are used to identify the the argument position or status of the running process.

| Variable | Description |
| :------: | :------ |
| $# | Numbers of all arguments |
| $* | A collection of all position variables |
| $@ | Same as $* |
| "$@" | A collection of values of each variables |
| $$ | PID of the current process |
| $? | Exit code of the last command |

### Orders of execution of commands

Sometimes we need to run one command only if another command succeeds or fails. For example, to copy directory "/home/test" to "/home/ubuntu" and remove "/home/test", we need to guarantee that the old directory has been moved successfully before removing the old one. Another example is that when we creating a user account, we need to make sure that this user does not exist, so we have to "grep" the file "/etc/passwd", and create an account if the "grep" command fails.

To specify the order of execution of commands:

* Use "&&" if we want the next command executed only if the last command succeeds
* Use "||" if we want the next command executed only if the last command fails
* Use ";" if we want all the following command to be executed uninterruptedly

We will use the examples mentioned above to illustratte the usage of these symbols. 

To copy a directory to a new place and then remove the old one, execute in terminal:

          sudo cp /home/test  /home/ubuntu/bak  && sudo  rm –rf /home/test

If the command "cp" fails, "rm" will not be executed.

To create a user account:

          grep “user1”  /etc/passwd   || sudo useradd –d  /home/user1 –m user1

The user "user1" will be created by command "useradd" only if no output is presented by command "grep".

To execute multiple programs in a single command:

          sudo mkdir /home/bak ; cd /home/bak ; tar czvf passwd.tar.gz /etc/passwd /etc/group

This command calls "sudo mkdir", "cd" and "tar" in turn, first creates a directory named "/home/bak", then switch to the directory, and make a backup of "/etc/passwd" and "/etc/group" as the archive file "passwd.tar.gz".

###  Command substitution

Command substitution can be used to assign the result from a command to a variable. Use a pair of backticks (`) or "$(command)" to do so.

For example, to assign the kernel version to a variable, then print it:

    kernel_version=`uname -r` && echo $kernel_version
    kernel_version=$(uname -r) && echo $kernel_version

The result from command "uname -r" is then saved. This function is commonly used in shell programming.


## Reference

[来源_balaamwe博客:linux特殊符号大全](http://www.cnblogs.com/balaamwe/archive/2012/03/15/2397998.html)