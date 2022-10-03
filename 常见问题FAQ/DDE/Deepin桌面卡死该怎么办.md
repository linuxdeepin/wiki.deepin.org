---
title: Deepin桌面卡死该怎么办
description: 
published: true
date: 2022-10-03T18:40:31.623Z
tags: dde桌面环境
editor: markdown
dateCreated: 2022-06-28T02:25:34.050Z
---

全屏游戏卡死无法退出怎么办？

Deepin/UOS可以利用虚拟桌面功能，按 <kbd>⊞</kbd> + <kbd>→</kbd> 切换到第二虚拟桌面，然后在这里打开系统监视器，结束卡死的进程。

------

如果是整个桌面卡死，首先尝试：

按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F2</kbd>，进入tty2
按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F1</kbd>，回到桌面，可能会要求重新登录，卡死缓解。

注意：按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F1</kbd> 回到桌面仅适用于 UOS/Deepin 的 LightDM 登录器，其他发行版或登录器可能需要按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F7</kbd> 或者 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F6</kbd> 等，你可以逐一尝试 <kbd>F1</kbd> 到 <kbd>F7</kbd>。

------

如果没有缓解：

按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F2</kbd>，进入tty2
输入用户名，回车
输入密码，回车
输入以下命令回车：

```bash
killall kwin_x11
```

按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F1</kbd>，回到桌面，可能会要求重新登录，卡死缓解。

注意：`killall kwin_x11` 仅适用于UOS/Deepin的DDE桌面环境，其他发行版或桌面环境需要把 `kwin_x11` 更换为各自采用的桌面合成器。

------

如果没有缓解

按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F2</kbd>，进入tty2，然后执行命令：

```bash
ps ux | less
```

找到引起卡死的进程id，比如1234，然后执行以下命令：

```bash
kill -9 1234
```

如果想使用进程名称，也可以，比如：

```bash
killall -9 TIM.exe
```

注意进程名称区分大小写！Linux中的大部分名称都区分大小写。

然后按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F1</kbd>，回到桌面，可能会要求重新登录，卡死缓解。

------

如果还是没有缓解：

按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F2</kbd>，进入tty2，执行以下命令：

```bash
sudo systemctl restart lightdm
```

按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F1</kbd>，回到桌面，会要求重新登录，所有应用程序，卡死缓解。

------

如果还是没有缓解

按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F2</kbd>，进入tty2
按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>Delete</kbd> 这会重启

------

如果连按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F2</kbd> 都没有反应，无法进入tty2，还可以这样：

按住 <kbd>Alt</kbd> 和 <kbd>PrtSc</kbd> / <kbd>SysRq</kbd> 键（截图键），然后再按以下键可以重启：<kbd>S</kbd> <kbd>U</kbd> <kbd>B</kbd>。

![1](https://hu60.cn/q.php/link.img.html?url64=aHR0cDovL2ZpbGUuaHU2MC5jbi9maWxlL2hhc2gvcG5nLzYyNDcwM2I2MjFlZTIzZmQ3ODdmMDc3YTRlOThmZmQ4MTQ4NjI3LnBuZw..)

也就是依次按下三组快捷键：

<kbd>Alt</kbd> + <kbd>SysRq</kbd> + <kbd>S</kbd>
<kbd>Alt</kbd> + <kbd>SysRq</kbd> + <kbd>U</kbd>
<kbd>Alt</kbd> + <kbd>SysRq</kbd> + <kbd>B</kbd>

就可以重启，对系统没有损害。

上述重启方法对deb系有效（包括UOS/Deepin），对rpm系无效，除非你手动设置内核参数`kernel.sysrq=1`。

------

除了未开启SysRq功能之外（某些发行版默认没开），如果连 <kbd>Alt</kbd> + <kbd>SysRq</kbd> + <kbd>B</kbd> 也没用，按下后系统无法重启，说明内核可能已经崩溃了。你只能用传统手段强制重启或者强制关机了。