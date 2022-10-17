---
title: 02_hcitool命令:蓝牙调试工具
description: 简单介绍hcitool命令:蓝牙调试工具
published: true
date: 2022-10-17T06:22:11.960Z
tags: 
editor: markdown
dateCreated: 2022-07-05T09:25:10.991Z
---

# hcitool命令:蓝牙调试工具
hcitool命令用于配置蓝牙连接，并向蓝牙设备发送一些特殊命令。如果没有给定命令，或者使用了选项-h，hcitool会打印一些使用信息并退出。
在终端中输入：`hcitool`
```
thinKinG@thinKinG-PC:~/Desktop$ hcitool 
hcitool - HCI Tool ver 5.50
Usage:
        hcitool [options] <command> [command parameters]
Options:
        --help  Display help
        -i dev  HCI device
Commands:
        dev     Display local devices
        inq     Inquire remote devices
        scan    Scan for remote devices
        name    Get name from remote device
        info    Get information from remote device
        spinq   Start periodic inquiry
        epinq   Exit periodic inquiry
        cmd     Submit arbitrary HCI commands
        con     Display active connections
        cc      Create connection to remote device
        dc      Disconnect from remote device
        sr      Switch master/slave role
        cpt     Change connection packet type
        rssi    Display connection RSSI
        lq      Display link quality
        tpl     Display transmit power level
        afh     Display AFH channel map
        lp      Set/display link policy settings
        lst     Set/display link supervision timeout
        auth    Request authentication
        enc     Set connection encryption
        key     Change connection link key
        clkoff  Read clock offset
        clock   Read local or remote clock
        lescan  Start LE scan
        leinfo  Get LE remote information
        lewladd Add device to LE White List
        lewlrm  Remove device from LE White List
        lewlsz  Read size of LE White List
        lewlclr Clear LE White List
        lerladd Add device to LE Resolving List
        lerlrm  Remove device from LE Resolving List
        lerlclr Clear LE Resolving List
        lerlsz  Read size of LE Resolving List
        lerlon  Enable LE Address Resolution
        lerloff Disable LE Address Resolution
        lecc    Create a LE Connection
        ledc    Disconnect a LE Connection
        lecup   LE Connection Update

For more information on the usage of each command use:
        hcitool <command> --help
```

## hcitool 常用参数
`scan` : 远程扫描设备
在终端中输入：`hcitool scan`
```
thinKinG@thinKinG-PC:~/Desktop$ hcitool scan
Scanning ...
```

`dev` : 显示本地设备
在终端中输入：`hcitool dev`
```
thinKinG@thinKinG-PC:~/Desktop$ hcitool dev
Devices:
        hci0    50:2F:9B:56:EE:3C
```



