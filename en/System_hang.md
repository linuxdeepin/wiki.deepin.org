---
title: System_hang
description: 
published: true
date: 2022-04-21T03:57:23.415Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:20.716Z
---



## Summary

In computing, a hang or freeze occurs when either a computer program or system ceases to respond to inputs.  Hangs can be categorized into to two types:

* The "real" hang: The computer encounters fatal errors, and cannot continue to work anymore.

* The "fake" hang: This usually occurs when almost all hardware resource are occupied by programs and hardly any response is observed.

The difference of these to types of hangs generally lies in the solution used to deal with them. For a real hang, the operator cannot solve it at the software level, and must do a reboot or change the hardware equipment to bring the computer to a normal state. For a fake hang, the operator may wait for a period before the program finishes its task and release the resource so that everything is under the control, again.

However, there is sometimes no way to determine if a hang is a real one or not. When a fake hang appears, succeeding commands may turn it into a real hang. Besides, if the operator cannot wait such a long time for hangs, he or she may decide to try a solution at the hardware level, thus it is impossible to determine the type of that hang from its original state.

For most cases, fake hangs are observed as the user cannot move the cursor or click, but can switch to TTYs using Ctrl-Alt-F1 (or other Fn keys).

## Possible reasons

Some hangs may be caused by hardware defects. Others may due to bugs or inappropriate actions.

* Closing lid too quickly: If the system cannot resume after closing its lid, you can step to the Control Center -> Power Management, and uncheck the option "Suspend on lid close".

* No opertion after logout for a long time: Avoid to left the computer open after the logout.

## How to deal with hangs

First, we try to detect if it is a real hang. Press Ctrl+Alt+F2 and see if a terminal appears. If so, it is more likely to be a fake hang. Otherwise, a real hang can be assumed.

In both cases, you may submit a feedback to deepin teams by running

    sudo deepin-feedback-cli

in terminal and upload the report generated to http://feedback.deepin.org/.

## Recover from a fake hang

Note: the methods described here cannot be used to deal with a real hang.

### Unsafe ways

#### Reboot

Switch to a TTY by pressing Ctrl-Alt-F2, log in with your account and execute

    sudo reboot

#### Restart the login manager

Switch to a TTY by pressing Ctrl-Alt-F2, log in with your account and execute

    sudo restart lightdm 

### Safe ways

Use SysRq functions provided by Linux kernel, so that the commands of low-level can be issued directly to the kernel regardless of the userspace status. It is commonly used to recover from the hang of X server, or reboot system without causing damages to the file system.

#### Safe rebooting

To minimize the data loss when system hangs, press the following keys:

*Alt-SysRq* + R + S + E + I + U + B

The combination of Alt and SysRq should be held when the other 6 keys are pressed in turn, and remember to wait for one or two second before pressing the next key. If it does not take effect, try to increase the time interval between key pressing.

To remember this key combination easily, you may remember another sentence: **R**aising **S**kinny **E**lephants **I**s **U**tterly **B**oring.

#### Safe shutdown

To shutdown safely using SysRq function, press the following keys:

*Alt-SysRq* + R + S + E + I + U + O

This is similar to the operation of a safe reboot, except the last key press is "O" rather than "B".

Here are the descrptions of these key press:

* (r)aw: Switch the keyboard from raw mode to XLATE mode
* (s)ync: Sync all mounted filesystems
* t(e)rminate: Send the SIGTERM signal to all processes except init (PID 1)
* k(i)ll: Send the SIGTERM signal to all processes except init (PID 1)
* (u)mount: Remount all mounted filesystems in read-only mode
* re(b)boot: Immediately reboot the system
* (o)ff: Shut off the system


## References

[Wikipedia: Hang (Computing)](https://en.wikipedia.org/wiki/Hang_(computing))

[MagicSysRq keys for assistance with Ubuntu troubles](http://ubuntuforums.org/showthread.php?t=617349)

[Ubuntu的桌面死机后重启桌面方法](http://linux.net527.cn/Ubuntu/Ubuntuanzhuangyuyingyong/18698.html)