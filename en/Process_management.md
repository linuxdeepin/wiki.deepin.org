[[zh:进程管理]]


## Summary

All running thins, including tasks of users, programs of monitoring, are presented in form of process. Users do not need to care about how these processes are scheduled, or how Linux kernel allocates resource for them. They just need to know how to well control these processese for obtaining a better service. Thus, it is important to learn how to manage processes in Linux system.

## Process management

### Using graphical interface

Users can benefit from Deepin System Monitor in organizing their processes. Right click on the process you would like to operate on, then click the operation to trigger.

### Using command

#### View processes

Use `ps` to show current status of process

Syntax:

    ps [Options] [--help]
        
Commonly used options:

    -A    List all processes
    -w    Display more information in wider columns
    -au   Display detailed information
    -aux  Show all processes, including those from other users

Use `pstree` to display processes in tree, with the process specified (if PID is given) or init as its root. If user ID is given, only processes belonging to this user are shown.

Syntax:

    pstree [-a] [-c] [-h|-H PID] [-l] [-n] [-p] [-u] [-G|-U] [PID or User]
    pstree -V
        
Options:
    -a    Show complete command lines of processes. If process image is replaced out, then a pair of  parentheses is shown instead.
    -c    If there are duplicated process name, list them separately (the default behavior is to show an asterisk before their name)

Use `top` to show dynamic list of process

Syntax:

    top [-] [d Delay] [q] [c] [S] [s] [i] [n] [b]

Options:

    -d    Change the speed of refreshing
    -q    Refresh without delay. If top is executed by superuser, then it will run in highest priority
    -c    Change display mode. There are two modes: showing full path of executables, and showing executable names only
    -S    Accumulating mode, add up CPU time of all finished / dead child processes
    -s    Safety mode. Disable interactive commands to avoid potential risks
    -i    Do no display any idle or zombie process
    -n    Number of refreshing. After finishing specified iterations of refreshing, top will quit
    -b    Batch mode, used with "-n" option to redirect the output of top command to files

#### Terminate process

Use `Kill` to terminate running process or task. Kill can send special signals to the specified process, and the default signal is SIGTERM(15), which will terminate this process. If this process still does not end, users can try to send SIGKILL(9) to force killing it. Users need to know the PID of that process (by using `ps` or `job`) before they can send signals to it.

It is often used together with `ps` or `pgrep`.

Syntax:

    kill [-s <Signal name>] PID
    kill [-l <Signal number>] PID

Commonly used options:

    -s    Specify the number of signal to send; if no number is given, then all available signals are listed
    -l    Display the signal name of given signal number
    -9    Equals to "-s KILL", force terminating the process

Note: If both signal name and signal number can be omitted.

Example:

To view processes of httpd server:

        $ ps auxf |grep httpd
        root 4939 0.0 0.0 5160 708 pts/3 S+ 13:10 0:00 \_ grep httpd
        root 4830 0.1 1.3 24232 10272 ? Ss 13:02 0:00 /usr/sbin/httpd
        apache 4833 0.0 0.6 24364 4932 ? S 13:02 0:00 \_ /usr/sbin/httpd
        apache 4834 0.0 0.6 24364 4928 ? S 13:02 0:00 \_ /usr/sbin/httpd
        apache 4835 0.0 0.6 24364 4928 ? S 13:02 0:00 \_ /usr/sbin/httpd
        apache 4836 0.0 0.6 24364 4928 ? S 13:02 0:00 \_ /usr/sbin/httpd
        apache 4840 0.0 0.6 24364 4928 ? S 13:02 0:00 \_ /usr/sbin/httpd

You can also use `pgrep -l httpd` for the same purpose.

In the second column, i.e. PID column of the output above, process 4830 is the parent process of httpd, and process 4833, 4834, 4835, 4836, 4840 are all child processes of httpd. If we kill process 4830, all its child processes will die.

        kill 4840 ## Kill process 4840
        ps -auxf | grep httpd ## You can check if httpd still run after executing this command
        kill 4830 ## Kill the parent process of httpd
        ps -aux | grep httpd ## You can check if httpd still run after executing this command

For zombie process, use `kill -9` to force terminating it.

When a program dies and cannot be kill with SIGTERM, then it is better to send SIGKILL to it, perhaps its parent process as well:

        $ ps aux | grep gaim
        beinan 5031 9.0 2.3 104996 17484 ? S 13:23 0:01 gaim
        root 5036 0.0 0.0 5160 724 pts/3 S+ 13:24 0:00 grep gaim
        
        $ pgrep -l gaim
        5031 gaim
        
        $kill -9 5031

The mechanism of kill command

kill sends a signal number and a PID to Linux kernel. Kernel then finish the operation to specified process.

Sometimes we see to much processes in the output from top command. Kill command is then needed to terminate processes to retrieve some system resource. It is also possible that a program get stuck in a virtual terminal, so we need to switch to another virtual terminal and run "kill" to end it.

Use `killall` to send specified signal to all processes that have the specified name. The default signal sent is SIGTERM.

Users can use either the name or the number of the signal. Use number "0" to check if certain process exists.

If the name of process contains slash ("/"), then processes executing that particular file will be  selected for killing, independent of their name.

killall returns a zero return code if at least one process has been killed for each listed command, and a non-zero value if no process is found to kill. Note that killall will never kill itself, but one killall process can kill other killall processes).

killall can also be used with `ps` or `pgrep`.

Syntax:

        killall [-egiqvw] [-SignalNumber] ProcessName ... 
        killall -l 
        killall -V 
        
        
Options:

        -e    Require an exact match for very long names. If a command name is longer than 15 characters, the full name may be unavailable. In this case, killall will kill everything that matches within the first 15 characters.
        -g    Kill the process group to which the process belongs. The kill signal is only sent once per group, even if multiple processes belonging to the same process group were found.
        -i    Interactively ask for confirmation before killing.
        -l    List all known signal names.
        -q    Do not complain if no processes were killed.
        -v    Report if the signal was successfully sent.
        -w    Wait for all killed processes to die.

Example:

        $ pgrep -l gaim
        2979 gaim
        
        $ killall gaim

Use `pkill` to kill running program. pkill can also be used to kill all processes that belongs to specified user, and kick this user out from system.

Syntax:

    pkill ProcessName
    pkill UserName

Example:

        $ pgrep -l gaim
        2979 gaim
        
        $ pkill gaim
        $ pkill linux ## Kill all processes belonging to user "linux", and kick he/she out

Use `xkill` to terminate desktop applications.

For example, to kill firefox brower when it gets stuck, execute in terminal:

         xkill

When xkill window appears, click on the window that you would like to close. Right click to exit xkill.

#### Adjusting process

Use `nice` to adjust the priority of process. If not process is specified, then show the current default nice, with range from -20 (highest priority) to 19 (lowest priority).

Syntax:

    nice [-n Adjustment] [--help] [--version] [Command] [Arguments]

Example: to increase the nice value of command "ls" by 1, execute in terminal:

    nice -n 1 ls
    
Note: The exact priority of running process is determined by Linux kernel, and nice value is taken in to concern when using "round-robin" algorithm to calculate the priority. A process with higher priority gains more CPU time.

### Running tasks regularly

#### Crontab

crontab is used to execute Linux program regularly, or in a specified time. It is like the timetable of users.

Syntax:

        crontab [ -u User ] TableFile
        crontab [ -u User ] { -l | -r | -e }

Options:

        -u    Set timetable for specified user. Only root can set others' timetable. If no user are specified, then the operator itself is implied.
        -e    Use default text editor to set timetable. The default text editor is usually VI. If you would like to use other editor, please set "VISUAL" environment variable before executing crontab (e.g. export VISUAL=gedit)
        -r    Remove current timetable
        -l    List current timetable

The timetable has the format of:

        Field1    Field2    Field3    Field4    Field5    Program

Where Field1 is seen as minutes, Field2 as hours, Field3 as days of year, Field4 as months, Field5 as days of week. Multiple values are seperated using comma. Program is the executable to run.

When an single asterisk appers in Field1 ~ Field5, it matches all possible value of that field, so that the program is executed every one minute (or hour, or day, etc.).

When an "-" appers in Field1 ~ Field5, the program is executed in specified time range.

When the Field1 ~ Field5 have format "*/n", the program is executed every n minutes (or n hours, or n days, etc).

Note: Users can also put configurations in a file, and submit this file to crontab.

To start crond service:

         service crond start|stop|restart|reload

To edit configurations of cron for iptables:

         crontab /root/desktop/iptables.cron

Create a crontab file:

         crontab crontabfile

or
        crontab -e

Example 1: Execute "/bin/ls" in every first minute of a hour:

         0 * * * * /bin/ls

Example 2: Execute "/usr/bin/backup" every 20 minutes from 6 a.m. to 12 p.m. in December:

         0 6-12/3 * 12 * /usr/bin/backup

Note: After the command has been executed, system will send you a message of the output of that program. Append `> /dev/null 2>&1` to avoid receiving messages of this kind.

#### at

Use `at` to run specified command at a certain time. The format of the time can be either "HH:MM", or "midnight", "noon".

Users can also specify date as "today" or "tomorrow". After type "Enter", at will provide a interactive mode for entering command or program name. Please press Ctrl-D after entering all commands. The output of the commands will be sent to your account.

Syntax:

         at -V [-q Queue] [-f File] [-mldbv] Time
        
Options:

        -q    Use specified queue to store tasks. There are 52 queues for tasks in total, a, b, c, ..., y, z, A, B, C,  ..., Y, Z
        -m    Send message to user even there is no output after executing
        -f    Read commands from specified file, instead of using interactive mode
        -l    List all assignment (equals to command "atq")
        -d    Delete assignments (equals to command "atrm")
        -v    List all assignment that have been finished but not deleted yet)

Example 1: Execute "/bin/ls" at 5 p.m. three days later:

         at 5pm +3 days /bin/ls

Example 2: Execute "/bin/ls" at 5 p.m. three weeks later:

         at 5pm +2 weeks /bin/ls

Example 3: Execute "/bin/date" at 17:20 tomorrow:

         at 17:20 tomorrow /bin/date

Example 4: Print "the end of world !" at 23:59 in the last day of 1999:

         at 23:59 12/31/1999 echo the end of world !

Example: Touch "at.txt" in the given time:

         $ at 9:20 4/13/2011
         at> touch /root/desktop/at.txt
         at> <EOT> # Press Ctrl-D to finish

## Reference

[鸟哥私房菜:程序管理](http://vbird.dic.ksu.edu.tw/linux_basic/0440processcontrol.php#process)

[Linux进程控制](http://210.44.193.6/linux/07.htm)

[Slackware手册：进程控制](http://docs.slackware.com/slackbook:process_control)

[Linux 系统 kill、killall、pkill、xkill](http://blog.sina.com.cn/s/blog_8ace4a0401014fql.html)