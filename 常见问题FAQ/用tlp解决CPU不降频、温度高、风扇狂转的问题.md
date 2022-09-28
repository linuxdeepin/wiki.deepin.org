---
title: 用tlp解决CPU不降频、温度高、风扇狂转的问题
description: 
published: true
date: 2022-09-28T09:08:54.082Z
tags: cpu降频, tlp
editor: markdown
dateCreated: 2022-09-28T09:08:04.659Z
---

# 用tlp解决CPU不降频、温度高、风扇狂转的问题
- 原文来自于社区论坛作者[rekees2020](https://bbs.deepin.org/user/247659)的发帖[《用tlp解决CPU不降频、温度高、风扇狂转的问题》](https://bbs.deepin.org/zh/post/222504)

> 本文只针对英特尔CPU，主要是要调CPU scaling governor。其他CPU也能用tlp，但是CPU相关的设置不同，可以自行研究打开tlp的配置文件。

### 步骤1：确保CPU使用Intel P-state，这样需要的时候可以睿频、不需要的时候可以降频：

终端内输入命令 `cat /sys/devices/system/cpu/intel_pstate/status`

如果输出结果不是active，做如下操作：

1. 在grub里添加intel_pstate：终端内输入 `sudo deepin-editor /etc/default/grub`;

 打开的文件内 GRUB_CMDLINE_LINUX_DEFAULT后面的引号里加上intel_pstate=force  点文本编辑器针对当前文件的x，选择"保存"

2. `sudo update-grub2`重启，再在终端内查看`cat /sys/devices/system/cpu/intel_pstate/status`输出结果;

3. 如果还不是active，创建txt文件，里面写入：
```
echo 用户密码|sudo -S sudo ls

echo active|sudo tee /sys/devices/system/cpu/intel_pstate/status
```
点文本编辑器针对当前文件的x，选择"保存"；

4. 右击该文件，选择"属性"，允许作为程序运行；

5. 在~/.config/autostart里创建文本文件，修改后缀为.desktop, 文件内容为：
```
[Desktop Entry]

Name=ForcePState

Type=Application

Exec=上面那个txt文件的绝对路径

Hidden=false
```
点文本编辑器针对当前文件的x，选择"保存"

 (也可以用其他的方法开机自动以sudo运行echo active|sudo tee /sys/devices/system/cpu/intel_pstate/status，我试了几种方法都没凑效，只能这样粗暴；~/.config/autostart就是当前用户的home下的.config内的autostart) 。

### 步骤2：安装tlp：终端内sudo apt install tlp

### 步骤3：修改tlp参数：终端内输入`sudo deepin-editor /etc/default/tlp`

   在打开的文件内设置如下参数值--
```
CPU_SCALING_GOVERNOR_ON_AC=powersave

CPU_SCALING_GOVERNOR_ON_BAT=powersave

ENERGY_PERF_POLICY_ON_AC=default

ENERGY_PERF_POLICY_ON_BAT=power

CPU_HWP_ON_AC=default

CPU_HWP_ON_BAT=power
```
点文本编辑器针对当前文件的x，选择"保存"。

### 步骤4：运行ltp，终端中输入`sudo tlp start` 
1. 不建议禁用Intel P-state，那相当于阉割CPU的睿频功能，并且也只能将CPU频率限制在固有频率，不保证低负载时能继续往下降；

2. 系统自带laptop-mode-tools，和tlp是一样性质的，应该也能达到相同作用；不过我一开始就只用tlp，没研究过laptop-mode-tools；安装tlp后建议将laptop-mode-tools卸载：终端内 sudo apt purge laptop-mode-tools，电源管理会由tlp接管，不会乱的；

3. tlp设置基本原则是  “不插电情况下选最省电的值，插电情况下选第二高性能，即第二不省电的值”；tlp设置文件里有详细的注释；保存tlp设置文件后，终端内运行sudo tlp start重新载入tlp；

4. 设置CPU scaling governor后，如果问题解决，其他参数保持默认就好；如果插电情况下CPU频率仍然居高不下，CPU_SCALING_GOVERNOR_ON_AC之外的几个带AC的参数再试第三高性能、即第三不省电的值；不同的机器上需要的设置可能不同，尝试一下就好；

5. 通过任何能查看CPU实时频率的方法看CPU频率的变化；期望的结果是，插电、什么也不干的情况下，CPU频率就只几百MHz或1点几GHz，负荷上去后CPU频率跟着飙升，最高能达到睿频上限；

6. 给CPU施加压力的简单方法，算圆周率：在终端内 time echo “scale=5000; 4*a(1)” | bc -l -q               (5000表示算到小数点后5000位)。