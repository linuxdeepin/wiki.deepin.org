---
title: org.freedesktop.login1 — systemd-logind 的 D-Bus 接口
description: 
published: true
date: 2022-07-26T05:32:45.736Z
tags: freedesktop
editor: markdown
dateCreated: 2022-07-26T05:32:42.699Z
---

## name

org.freedesktop.login1 — systemd-logind 的 D-Bus 接口

## 简介

[systemd-logind.service (8)](https://www.freedesktop.org/software/systemd/man/systemd-logind.service.html#) 是一个跟踪用户登录和seat的系统服务。

守护进程提供 C 库接口和 D-Bus 接口。库界面可用于内省和观察用户登录和seat^[seat由分配给特定工作场所的所有硬件设备组成。它由至少一个图形设备组成，通常还包括键盘、鼠标。它还可以包括摄像机、声卡等。seat由seat名称标识，这些字符串是字符串（<= 255 个字符），以四个字符 " seat" 开头，后跟至少一个 [a-zA-Z0-9]、" _" 和 " -" 范围内的字符。它们适合用作文件名。seat名称可能稳定，也可能不稳定，如果seat再次可用，则可能会重复使用。]的状态。总线接口提供相同的功能，但也可用于更改系统状态。更多信息请查看[sd-login (3)](https://www.freedesktop.org/software/systemd/man/sd-login.html#)。

## 管理器对象

该服务在总线上的 Manager 对象上公开以下接口：

```text
node /org/freedesktop/login1 {
  interface org.freedesktop.login1.Manager {
    methods:
      GetSession(in  s session_id,
                 out o object_path);
      GetSessionByPID(in  u pid,
                      out o object_path);
      GetUser(in  u uid,
              out o object_path);
      GetUserByPID(in  u pid,
                   out o object_path);
      GetSeat(in  s seat_id,
              out o object_path);
      ListSessions(out a(susso) sessions);
      ListUsers(out a(uso) users);
      ListSeats(out a(so) seats);
      ListInhibitors(out a(ssssuu) inhibitors);
      @org.freedesktop.systemd1.Privileged("true")
      CreateSession(in  u uid,
                    in  u pid,
                    in  s service,
                    in  s type,
                    in  s class,
                    in  s desktop,
                    in  s seat_id,
                    in  u vtnr,
                    in  s tty,
                    in  s display,
                    in  b remote,
                    in  s remote_user,
                    in  s remote_host,
                    in  a(sv) properties,
                    out s session_id,
                    out o object_path,
                    out s runtime_path,
                    out h fifo_fd,
                    out u uid,
                    out s seat_id,
                    out u vtnr,
                    out b existing);
      @org.freedesktop.systemd1.Privileged("true")
      ReleaseSession(in  s session_id);
      ActivateSession(in  s session_id);
      ActivateSessionOnSeat(in  s session_id,
                            in  s seat_id);
      LockSession(in  s session_id);
      UnlockSession(in  s session_id);
      LockSessions();
      UnlockSessions();
      KillSession(in  s session_id,
                  in  s who,
                  in  i signal_number);
      KillUser(in  u uid,
               in  i signal_number);
      TerminateSession(in  s session_id);
      TerminateUser(in  u uid);
      TerminateSeat(in  s seat_id);
      SetUserLinger(in  u uid,
                    in  b enable,
                    in  b interactive);
      AttachDevice(in  s seat_id,
                   in  s sysfs_path,
                   in  b interactive);
      FlushDevices(in  b interactive);
      PowerOff(in  b interactive);
      PowerOffWithFlags(in  t flags);
      Reboot(in  b interactive);
      RebootWithFlags(in  t flags);
      Halt(in  b interactive);
      HaltWithFlags(in  t flags);
      Suspend(in  b interactive);
      SuspendWithFlags(in  t flags);
      Hibernate(in  b interactive);
      HibernateWithFlags(in  t flags);
      HybridSleep(in  b interactive);
      HybridSleepWithFlags(in  t flags);
      SuspendThenHibernate(in  b interactive);
      SuspendThenHibernateWithFlags(in  t flags);
      CanPowerOff(out s result);
      CanReboot(out s result);
      CanHalt(out s result);
      CanSuspend(out s result);
      CanHibernate(out s result);
      CanHybridSleep(out s result);
      CanSuspendThenHibernate(out s result);
      ScheduleShutdown(in  s type,
                       in  t usec);
      CancelScheduledShutdown(out b cancelled);
      Inhibit(in  s what,
              in  s who,
              in  s why,
              in  s mode,
              out h pipe_fd);
      CanRebootParameter(out s result);
      SetRebootParameter(in  s parameter);
      CanRebootToFirmwareSetup(out s result);
      SetRebootToFirmwareSetup(in  b enable);
      CanRebootToBootLoaderMenu(out s result);
      SetRebootToBootLoaderMenu(in  t timeout);
      CanRebootToBootLoaderEntry(out s result);
      SetRebootToBootLoaderEntry(in  s boot_loader_entry);
      SetWallMessage(in  s wall_message,
                     in  b enable);
    signals:
      SessionNew(s session_id,
                 o object_path);
      SessionRemoved(s session_id,
                     o object_path);
      UserNew(u uid,
              o object_path);
      UserRemoved(u uid,
                  o object_path);
      SeatNew(s seat_id,
              o object_path);
      SeatRemoved(s seat_id,
                  o object_path);
      PrepareForShutdown(b start);
      PrepareForSleep(b start);
    properties:
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      @org.freedesktop.systemd1.Privileged("true")
      readwrite b EnableWallMessages = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      @org.freedesktop.systemd1.Privileged("true")
      readwrite s WallMessage = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly u NAutoVTs = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly as KillOnlyUsers = ['...', ...];
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly as KillExcludeUsers = ['...', ...];
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly b KillUserProcesses = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly s RebootParameter = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly b RebootToFirmwareSetup = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly t RebootToBootLoaderMenu = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly s RebootToBootLoaderEntry = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly as BootLoaderEntries = ['...', ...];
      readonly b IdleHint = ...;
      readonly t IdleSinceHint = ...;
      readonly t IdleSinceHintMonotonic = ...;
      readonly s BlockInhibited = '...';
      readonly s DelayInhibited = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly t InhibitDelayMaxUSec = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly t UserStopDelayUSec = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s HandlePowerKey = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s HandlePowerKeyLongPress = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s HandleRebootKey = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s HandleRebootKeyLongPress = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s HandleSuspendKey = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s HandleSuspendKeyLongPress = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s HandleHibernateKey = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s HandleHibernateKeyLongPress = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s HandleLidSwitch = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s HandleLidSwitchExternalPower = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s HandleLidSwitchDocked = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly t HoldoffTimeoutUSec = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s IdleAction = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly t IdleActionUSec = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly b PreparingForShutdown = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly b PreparingForSleep = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly (st) ScheduledShutdown = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly b Docked = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly b LidClosed = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly b OnExternalPower = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly b RemoveIPC = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly t RuntimeDirectorySize = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly t RuntimeDirectoryInodesMax = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly t InhibitorsMax = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly t NCurrentInhibitors = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly t SessionsMax = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly t NCurrentSessions = ...;
  };
  interface org.freedesktop.DBus.Peer { ... };
  interface org.freedesktop.DBus.Introspectable { ... };
  interface org.freedesktop.DBus.Properties { ... };
};
```

### 方法

- `GetSession()`可用于获取具有指定 ID 的会话的会话对象路径。同样，`GetUser()`  `GetSeat()`分别获取用户和Seat对象。`GetSessionByPID()`并 `GetUserByPID()`获取指定 PID 所属的会话/用户对象（如果有）。
- `ListSessions()`返回所有当前会话的数组。数组中的结构由以下字段组成：会话 ID、用户 ID、用户名、Seat ID、会话对象路径。如果会话没有附加Seat，则Seat ID 字段将为空字符串。
- `ListUsers()`返回所有当前登录用户的数组。数组中的结构由以下字段组成：用户 ID、用户名、用户对象路径。
- `ListSeats()`返回所有当前可用Seat的数组。数组中的结构由以下字段组成：seat id、seat对象路径。
- `ListInhibitors()`列出所有当前有效的抑制剂。它返回一个由`what`、`who`、`why`、 `mode`、`uid`（用户 ID）和`pid`（进程 ID）组成的结构数组。
- `CreateSession()`并可`ReleaseSession()`用于打开或关闭登录会话。这些调用**绝不能**由客户端直接调用。创建/关闭会话完全是 PAM 及其 [pam_systemd (8)](https://www.freedesktop.org/software/systemd/man/pam_systemd.html#) 模块的工作。
- `ActivateSession()`将具有指定 ID 的会话带到前台。`ActivateSessionOnSeat()`做同样的事情，但前提是seat ID 匹配。
- `LockSession()`要求具有指定 ID 的会话激活屏幕锁定。`UnlockSession()`要求具有指定 ID 的会话解除活动的屏幕锁定（如果有）。这是通过从会话管理器应该监听的相应会话对象发送 `Lock()` 和 `Unlock()`信号来实现的。
- `LockSessions()`要求所有会话激活其屏幕锁定。这可用于在一个操作中锁定对整个机器的访问。同样，`UnlockSessions()` 要求所有会话停用其屏幕锁定。
- `KillSession()`可用于向会话的一个或所有进程发送 Unix 信号。作为参数，它接受会话 id、字符串“ `leader`”或“ `all`”和一个信号编号。如果“ `leader`”通过，则只有会话“ `leader`”被终止。如果“ `all`”通过，则会话的所有进程都将被终止。
- `KillUser()`可用于向用户的所有进程发送 Unix 信号。作为参数，它需要用户 ID 和信号编号。
- `TerminateSession()`, `TerminateUser()`, `TerminateSeat()`可用于分别强制终止一个特定会话、用户的所有进程以及附加到特定seat的所有会话。会话、用户和seat由它们各自的 ID 标识。
- `SetUserLinger()`启用或禁用用户延迟。如果启用，将保留用户的运行时目录，并且他们可以在注销时继续运行进程。如果禁用，运行时目录会在他们注销后立即消失。`SetUserLinger()` 需要三个参数：UID、一个是否启用/禁用的布尔值和一个控制 [polkit](https://www.freedesktop.org/software/polkit/docs/latest/) 授权交互性的布尔值（见下文）。请注意，用户逗留状态永久存储在磁盘上。
- `AttachDevice()`可用于将特定设备分配给特定seat。该设备由其`/sys/`路径标识，并且必须符合分配seat的条件。`AttachDevice()`接受三个参数：seat ID、sysfs 路径和用于控制 polkit 交互性的布尔值（见下文）。设备分配永久存储在磁盘上。要创建新seat，只需指定以前未使用的seat ID。有关seat分配逻辑的更多信息，请参阅 [sd-login (3)](https://www.freedesktop.org/software/systemd/man/sd-login.html#)。
- `FlushDevices()`删除设备的所有显式seat分配，将所有分配重置为自动默认值。它需要的唯一参数是 polkit 交互性布尔值（见下文）。
- `PowerOff()`, `Reboot()`, `Halt()`, `Suspend()`, 并`Hibernate()`导致系统关机、重启、暂停（不关闭电源关闭）、挂起（系统状态保存到 RAM 并关闭 CPU）或休眠（将内存中数据保存到磁盘并将机器关闭）。`HybridSleep()`导致系统进入混合睡眠模式，即系统既休眠又挂起。`SuspendThenHibernate()`导致系统被暂停，随后使用RTC定时器唤醒并休眠。唯一的参数是polkit交互性布尔值`interactive`（见下文）。这些调用的主要目的是执行polkit政策，因此允许非特权用户关闭/重启/暂停/休眠。它们还为非特权用户执行抑制锁。UIs应该将这些调用作为关闭/重启/暂停/休眠机器的主要机制来公开。方法`PowerOffWithFlags()`,` RebootWithFlags()`,` HaltWithFlags()`, `SuspendWithFlags()`, `HibernateWithFlags()`, `HybridSleepWithFlags()`和`SuspendThenHibernateWithFlags()`添加flag以允许扩展，定义如下。

    `#define SD_LOGIND_ROOT_CHECK_INHIBITORS  (UINT64_C(1) << 0)`
`#define SD_LOGIND_KEXEC_REBOOT           (UINT64_C(1) << 1)`

    当`flags`为 0 时，这些方法的行为就像没有flag的版本一样。设置 (0x01)时`SD_LOGIND_ROOT_CHECK_INHIBITORS`，特权用户也可以使用活动抑制器。当`SD_LOGIND_KEXEC_REBOOT` 设置 (0x02)时，`RebootWithFlags()`如果加载了 kexec 内核，则执行 kexec reboot。
- `SetRebootParameter()`为后续重新启动操作设置参数。有关详细信息，请参见[systemctl](https://www.freedesktop.org/software/systemd/man/systemctl.html#) [(1)](https://www.freedesktop.org/software/systemd/man/systemctl.html#)和 [reboot](http://man7.org/linux/man-pages/man2/reboot.2.html) [(2)](http://man7.org/linux/man-pages/man2/reboot.2.html)中 的**reboot**描述 。
- `SetRebootToFirmwareSetup()`, `SetRebootToBootLoaderMenu()`, 和`SetRebootToBootLoaderEntry()` 配置重启后从引导加载程序执行的操作：分别进入固件设置模式、引导加载程序菜单或特定引导加载程序条目。对应的命令行界面见 [systemctl (1)](https://www.freedesktop.org/software/systemd/man/systemctl.html#)。
- `CanPowerOff()`, `CanReboot()`, `CanHalt()`, `CanSuspend()`, `CanHibernate()`, `CanHybridSleep()`, `CanSuspendThenHibernate()`, `CanRebootParameter()`, `CanRebootToFirmwareSetup()`, `CanRebootToBootLoaderMenu()`, `CanRebootToBootLoaderEntry()`测试系统是否支持相应的操作，是否允许调用用户执行。返回“ `na`”、“ `yes`”、“ `no`”和“ `challenge`”之一。如果返回“`na`”，则该操作不可用，因为硬件、内核或驱动程序不支持它。如果`yes`返回“ ”，则表示支持该操作，用户无需进一步认证即可执行该操作。如果`no`返回“”，则表示该操作可用，但不允许用户执行该操作。如果 ”`challenge`"返回，操作可用，但必须经过授权。
- `ScheduleShutdown()`在自UNIX纪元以来的微秒时间usec安排一个关机操作类型。类型可以是 "`poweroff`"、"`dry-poweroff`"、"`reboot`"、"`dry-reboot`"、"`halt` "和 "`dry-halt` "之一。`CancelScheduledShutdown()`取消一个预定的关机。如果计划了关机操作，输出参数cancelled为真。
- `SetWallMessage()`设置墙消息（关机瀑布流）。（将发送到所有终端并存储在 [utmp (5)](http://man7.org/linux/man-pages/man5/utmp.5.html)记录中的消息）以进行后续计划的关闭操作。该参数`wall_message`指定将包含在关闭消息中的关闭原因（并且可能为空）。该参数 `enable`指定是否在关机时打印墙消息。

`Inhibit()`创建一个抑制锁。它有四个参数： `what`、`who`、`why`和 `mode`:
  - `what`是“ `shutdown`”、“ `sleep`”、“ `idle`”、“ `handle-power-key`”、“ `handle-suspend-key`”、“ `handle-hibernate-key`” 、“ `handle-lid-switch`”中的一个或多个，用冒号分隔，用于禁止关机/重启、挂起/休眠、自动空闲逻辑或硬件密钥处理。
  - `who`应该是一个简短的人类可读字符串，用于标识获取锁的应用程序, 若未设置， 则使用被执行的命令行字符串。
  - `why` 应该是一个简短的人类可读字符串，用于标识锁定原因。
  - `mode`是“ `block`”或“`delay`“它编码是否应将禁止视为强制性或是否应将操作延迟到某个最大时间。该方法返回一个文件描述符。在此文件描述符及其所有副本关闭时释放锁。有关更多信息关于抑制逻辑，请参见 [Inhibitor Locks](https://www.freedesktop.org/wiki/Software/systemd/inhibit)。

### 信号

每当禁止状态或空闲提示发生变化时，`PropertyChanged` 都会发送客户端可以订阅的信号。

每次创建或删除会话、用户登录或注销或添加或删除seat时都会发送`UserRemoved`、`SeatNew`、`SessionNew`、`SeatRemoved`、`SessionRemoved`、 `UserNew`和信号 。它们每个都包含对象的 ID 和对象路径。

`PrepareForShutdown()`和`PrepareForSleep()`信号在系统关闭之前（使用参数“ ” `true`）或之后（使用参数“ `false`”）发送，分别用于重新启动/关机和挂起/休眠。应用程序可以使用它来将数据保存在磁盘上、释放内存或执行其他应在关机/睡眠前不久完成的工作，以及延迟抑制锁。完成这项工作后，他们应该释放他们的抑制锁，以免进一步延迟操作。有关详细信息，请参阅 [抑制器锁](https://www.freedesktop.org/wiki/Software/systemd/inhibit)。

### 属性

大多数属性仅反映配置，请参阅 [logind.conf (5)](https://www.freedesktop.org/software/systemd/man/logind.conf.html#)。这包括：`NAutoVTs`, `KillOnlyUsers`, `KillExcludeUsers`, `KillUserProcesses`, `IdleAction`, `InhibitDelayMaxUSec`, `InhibitorsMax`, `UserStopDelayUSec`, `HandlePowerKey`, `HandleSuspendKey`, `HandleHibernateKey`, `HandleLidSwitch`, `HandleLidSwitchExternalPower`, `HandleLidSwitchDocked`, `IdleActionUSec`, `HoldoffTimeoutUSec`, `RemoveIPC`, `RuntimeDirectorySize`, `RuntimeDirectoryInodesMax`, `InhibitorsMax`, 和 `SessionsMax`.

- `IdleHint`属性反映系统的空闲提示状态。如果系统空闲，它可能会根据配置进入自动挂起或关闭状态。
- `IdleSinceHint`并分别`IdleSinceHintMonotonic`编码空闲提示布尔值的最后一次更改的时间戳，in`CLOCK_REALTIME`和 `CLOCK_MONOTONIC`时间戳，以自uinx纪元以来的时间以微秒为单位。
- `BlockInhibited`和`DelayInhibited`属性对各个模式的当前活动锁进行编码。它们是冒号分隔的“ `shutdown`”、“ `sleep`”和“ `idle`”列表（见上文）。
- `NCurrentSessions`和`NCurrentInhibitors`包含当前注册的会话和抑制器的数量。
- `BootLoaderEntries`属性包含引导加载程序条目的列表。这包括配置中定义的引导加载程序条目以及引导加载程序报告的任何其他加载程序条目。有关更多信息，请参见 [systemd-boot (7)](https://www.freedesktop.org/software/systemd/man/systemd-boot.html#) 。
- `PreparingForShutdown`和`PreparingForSleep`布尔属性在两个`PrepareForShutdown`和`PrepareForSleep`信号的间隔期间分别为真。注意，这些属性不会发出PropertyChanged信号。
- `RebootParameter`属性显示使用上述 `SetRebootParameter()`方法设置的值。
- `ScheduledShutdown`显示了使用上述方法设置的值对 `ScheduleShutdown()`。
- 当用`SetRebootToFirmwareSetup`、`SetRebootToBootLoaderMenu`或`SetRebootToBootLoaderEntr`选择了正确的重启后操作时，`RebootToFirmwareSetup`、`RebootToBootLoaderMenu`和`RebootToBootLoaderEntr`为真。
- `WallMessage`和`EnableWallMessages`属性反映了关机原因和墙面信息启用开关，可以通过上述`SetWallMessage()`方法进行设置。
- `Docked`如果机器连接到扩展坞/usbhub，则为真。 
- `LidClosed`当（笔记本电脑的）盖子关闭时为真。 
- `OnExternalPower`当机器连接到外部电源时为真。

## 安全


一些操作是通过polkit特权系统保护的。

- `SetUserLinger()` 需要 `org.freedesktop.login1.set-user-linger` 权限。
- `AttachDevice()` 需要 `org.freedesktop.login1.attach-device`
- `FlushDevices()` 需要 `org.freedesktop.login1.flush-devices`
- `PowerOff()`、`Reboot()`、`Halt()`、`Suspend()`、`Hibernate()`需要`org.freedesktop.login1.power-off`、`org.freedesktop.login1.power-off-multiple-sessions`、`org.freedesktop.login1.power-off-ignore-inhibit freedesktop.login1.reboot`, `org.freedesktop.login1.reboot-multiple-sessions`, `org.freedesktop.login1.reboot-ignore-inhibit`, `org.freedesktop.login1.halt, org.freedesktop.login1.halt-multiple-sessions`, `org. freedesktop.login1.halt-ignore-inhibit`, `org.freedesktop.login1.suspend`, `org.freedesktop.login1.suspend-multiple-sessions`, `org.freedesktop.login1.suspend-ignore-inhibit`, `org.freedesktop.login1.hibernate freedesktop.login1.hibernate-multiple-sessions`、`org.freedesktop.login1.hibernate-ignore-inhibit`，

    分别取决于周围是否有其他会话或存在主动抑制。
- `HybridSleep()`和`SuspendThenHibernate()`使用与`Hibernate()`相同的权限。
- `SetRebootParameter()`需要`org.freedesktop.login1.set-reboot-parameter`。
- `SetRebootToFirmwareSetup`需要`org.freedesktop.login1.set-reboot-to-firmware-setup`。
- `SetRebootToBootLoaderMenu`需要`org.freedesktop.login1.set-reboot-to-boot-loader-menu`。
- `SetRebootToBootLoaderEntry`需要`org.freedesktop.login1.set-reboot-to-boot-loader-entry`。
- `ScheduleShutdown`和`CancelScheduledShutdown`需要与立即关机/重启/停止操作相同的权限（上面列出）。
- `Inhibit()`通过 `org.freedesktop.login1.inhibit-block-shutdown`、`org.freedesktop.login1.inhibit-delay-shutdown`、`org.freedesktop.login1.inhibit-block-sleep`、`org.freedesktop.login1.inhibit-delay-sleep inhibit-block-idle`, `org.freedesktop.login1.inhibit-handle-power-key`, `org.freedesktop.login1.inhibit-handle-uspend-key, org.freedesktop.login1.inhibit-handle-hibernate-key`, `org.freedesktop.login1.inhibit-handle-lid-switch`取决于采取的锁类型和方式。
- 交互式布尔参数可用于控制 polkit 是否应在需要时以交互方式要求用户提供认证凭证。

## Seat 对象

```text
node /org/freedesktop/login1/seat/seat0 {
  interface org.freedesktop.login1.Seat {
    methods:
      Terminate();
      ActivateSession(in  s session_id);
      SwitchTo(in  u vtnr);
      SwitchToNext();
      SwitchToPrevious();
    properties:
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s Id = '...';
      readonly (so) ActiveSession = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly b CanTTY = ...;
      readonly b CanGraphical = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly a(so) Sessions = [...];
      readonly b IdleHint = ...;
      readonly t IdleSinceHint = ...;
      readonly t IdleSinceHintMonotonic = ...;
  };
  interface org.freedesktop.DBus.Peer { ... };
  interface org.freedesktop.DBus.Introspectable { ... };
  interface org.freedesktop.DBus.Properties { ... };
};
```

### 方法

`Terminate()`和`ActivateSession()`的工作与`Manager`对象上的`TerminateSeat()`、`ActivationSessionOnSeat()`相似。

`SwitchTo()`切换到虚拟终端vtnr上的会话。 `SwitchToNext()`和`SwitchToPrevious()`按照虚拟终端的顺序，分别切换到seat上的下一个和上一个会话。如果没有活动的会话，它们分别切换到seat上的第一个和最后一个会话。

### 信号

每当`ActiveSession`、`Sessions`、`CanGraphical`、`CanTTY`或空闲状态发生变化时，`propertyChanged`信号就会被发送出去，客户端可以监听这个信号。

### 属性

`Id`属性对seat的ID进行编码。

`ActiveSession`编码当前的活动会话（如果有的话）。它是一个由会话ID和对象路径组成的结构。

`CanTTY`编码会话是否适用于文本登录，`CanGraphical`编码是否适用于图形会话。

`Sessions`属性是这个seat的所有当前会话的数组，每个会话都被编码为由ID和对象路径组成的结构。

`IdleHint`、`IdleSinceHint`和 `IdleSinceHintMonotonic`属性对空闲状态进行编码，与 `Manager`对象上暴露的属性类似，但对这个seat来说是特定的

## 用户对象

```text
node /org/freedesktop/login1/user/_1000 {
  interface org.freedesktop.login1.User {
    methods:
      Terminate();
      Kill(in  i signal_number);
    properties:
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly u UID = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly u GID = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s Name = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly t Timestamp = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly t TimestampMonotonic = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s RuntimePath = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s Service = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s Slice = '...';
      readonly (so) Display = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly s State = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly a(so) Sessions = [...];
      readonly b IdleHint = ...;
      readonly t IdleSinceHint = ...;
      readonly t IdleSinceHintMonotonic = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("false")
      readonly b Linger = ...;
  };
  interface org.freedesktop.DBus.Peer { ... };
  interface org.freedesktop.DBus.Introspectable { ... };
  interface org.freedesktop.DBus.Properties { ... };
};
```

### 方法

`Terminate()` 和`Kill()` 工作方式类似 在manager上的`TerminateUser()` 和 `KillUser()` 方法 

### 信号

每当`Sessions` 或者idle 状态 发生变化时，`propertyChanged`信号就会被发送出去，客户端可以监听这个信号。

### 属性

- `UID`和`GID`属性对用户的Unix UID 和主 GID 进行编码。
- `Name`属性对用户名进行编码。
- `Timestamp`和`TimestampMonotonic`分别在CLOCK_REALTIME和CLOCK_MONOTONIC时钟中以UNIX时间戳微秒为单位编码用户的登录时间。
- `RuntimePath`对用户的运行时路径进行编码，即`$XDG_RUNTIME_DIR`. 有关详细信息，请参阅 [XDG Basedir 规范](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html) 。
- `Service`包含此用户的用户 systemd 服务的单元名称。每个登录的用户都被分配了一个运行用户 systemd 实例的用户服务。这通常是`user@.service`.
- `Slice`包含此用户的用户 systemd 切片的单元名称。每个登录用户都会获得一个私有切片。
- `Display`编码应将哪个图形会话用作用户的主要 UI 显示。它是一个编码会话 ID 和要使用的会话的对象路径的结构。
- `State`对用户状态进行编码，是“ `offline`”、“ `lingering`”、“ `online`”、“ `active`”或“ `closing`”之一。有关状态的更多信息，请参见 [sd_uid_get_state (3)](https://www.freedesktop.org/software/systemd/man/sd_uid_get_state.html#) 。
- `Sessions`是编码用户所有当前会话的结构数组。每个结构都由 ID 和对象路径组成。
- `IdleHint`、`IdleSinceHint` 和 `IdleSinceHintMonotonic`属性对用户的空闲提示状态进行编码，类似于`Manager`的属性，但特定于该用户。
- `Linger`属性显示是否为此用户启用了延迟。

## 会话对象

```text
node /org/freedesktop/login1/session/1 {
  interface org.freedesktop.login1.Session {
    methods:
      Terminate();
      Activate();
      Lock();
      Unlock();
      SetIdleHint(in  b idle);
      SetLockedHint(in  b locked);
      Kill(in  s who,
           in  i signal_number);
      TakeControl(in  b force);
      ReleaseControl();
      SetType(in  s type);
      TakeDevice(in  u major,
                 in  u minor,
                 out h fd,
                 out b inactive);
      ReleaseDevice(in  u major,
                    in  u minor);
      PauseDeviceComplete(in  u major,
                          in  u minor);
      SetBrightness(in  s subsystem,
                    in  s name,
                    in  u brightness);
    signals:
      PauseDevice(u major,
                  u minor,
                  s type);
      ResumeDevice(u major,
                   u minor,
                   h fd);
      Lock();
      Unlock();
    properties:
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s Id = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly (uo) User = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s Name = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly t Timestamp = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly t TimestampMonotonic = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly u VTNr = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly (so) Seat = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s TTY = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s Display = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly b Remote = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s RemoteHost = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s RemoteUser = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s Service = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s Desktop = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s Scope = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly u Leader = ...;
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly u Audit = ...;
      readonly s Type = '...';
      @org.freedesktop.DBus.Property.EmitsChangedSignal("const")
      readonly s Class = '...';
      readonly b Active = ...;
      readonly s State = '...';
      readonly b IdleHint = ...;
      readonly t IdleSinceHint = ...;
      readonly t IdleSinceHintMonotonic = ...;
      readonly b LockedHint = ...;
  };
  interface org.freedesktop.DBus.Peer { ... };
  interface org.freedesktop.DBus.Introspectable { ... };
  interface org.freedesktop.DBus.Properties { ... };
};
```

### 方法

- `Terminate()`, `Activate()`, `Lock()`, `Unlock()`和 `Kill()`的工作类似于对 `Manager`对象的调用。
- `SetIdleHint()`由会话对象调用，以便在会话的空闲状态发生变化时更新它的状态。
- `TakeControl()`允许一个进程对该会话进行独家管理设备访问控制。在任何时候，只有一个D-Bus连接可以成为一个特定会话的控制器。如果设置了`force`参数（仅适用于root），现有的控制器将被踢出并替换。否则，如果已经有一个控制器，这个方法就会失效。注意，这个方法仅限于D-Bus用户，其有效UID设置为会话的用户或根用户。
- `ReleaseControl()`放弃了对一个给定会话的控制。关闭D-Bus连接也会隐式释放控制权。更多信息见`TakeControl()`。这个方法也释放了控制器通过`TakeDevice()`请求所有权的所有设备。
- `SetType()`允许动态地改变会话的类型。它只能由会话的当前控制器调用。如果`TakeControl()`没有被调用，这个方法将失效。此外，一旦控制权被释放，会话类型将被重置为原来的值，无论是通过调用`ReleaseControl()`还是关闭D-Bus连接。这应该有助于防止会话进入不一致的状态，例如在控制器崩溃的时候。唯一的参数`type`是新的会话类型。
- `TakeDevice()`允许会话控制器获得一个特定设备的文件描述符。输入字符设备的大号和小号，`systemd-logind`将返回该设备的文件描述符。目前只支持有限的设备类型（但可能会被扩展）。如果会话处于非活动状态，
- `ystemd-logind`会自动关闭文件描述符，一旦会话被激活，就会恢复。这保证了会话只能在会话激活时访问会话设备。注意，这种撤销/恢复机制是异步的，可能在任何时候发生。这只对附属于给定会话的seat的设备起作用。一个进程不需要直接访问设备节点。`systemd-logind`只要求你是活动的会话控制器（见`TakeControl()`）。还要注意，任何设备只能被请求一次。只要你不释放它，进一步调用`TakeDevice()`就会失败。
- `ReleaseDevice()`再次释放一个设备（见`TakeDevice()`）。这也是由`ReleaseControl()`或关闭D-Bus连接时隐含地完成的。
- `PauseDeviceComplete()`允许会话控制器在收到`PauseDevice("`pause`")`信号后同步暂停一个设备。强制信号（或内部超时后）由`systemd-logind`异步自动完成。
- `SetLockedHint()`可以用来设置 "锁定提示 "为`locked`，即会话是否被锁定的信息。这是为了让桌面环境用来告诉**systemd-logind**会话何时被锁定和解锁。
- `SetBrightness()`可以用来设置显示亮度。这是给桌面环境使用的，允许非特权程序以受控方式访问硬件设置。参数`subsystem`指定了一个内核子系统，可以是"`backlight`"或"`leds`"。参数`name`指定了指定子系统下的设备名称。亮度 "参数指定了亮度。范围由各个驱动定义，见`/sys/class/`subsystem`/`name`/max_brightness`。

### 信号

- 活动的会话控制器专门为它通过`TakeDevice()`请求的任何设备获得`PauseDevice`和`ResumeDevice`事件。每当一个设备被暂停或恢复时，它们都会通知控制器。如果一个设备的会话处于非活动状态，它就不会被恢复。还要注意`PauseDevice`信号在`Active`状态的`PropertyChanged`信号之前发送。反之则是`ResumeDevice`。即使 "会话 "处于活动状态，设备可能由于未知的原因而保持暂停状态。
- 一个`PauseDevice`信号携带主要和次要数字以及描述类型的字符串作为参数。`force`表示设备已经被`systemd-logind`暂停，该信号只是一个异步通知。`pause`表示`systemd-logind`给予你有限的时间来暂停设备。你必须通过`PauseDeviceComplete()`来回应。这种同步暂停机制是为了向后兼容VT，`systemd-logind`可以不使用它。如果你没有及时响应（或出于其他原因），它也可以自由地发送一个强制的`PauseDevice`。`gone`意味着该设备被从系统中拔出，你将不再收到任何关于它的通知。不需要调用`ReleaseDevice()`。如果有一个新的设备被分配到主要+次要组合，你可以再次调用`TakeDevice()`。
- `ResumeDevice`在会话激活和设备恢复时被发送。它携带主/次数字作为参数，并提供一个新的开放文件描述符。你应该切换到新的描述符并关闭旧的描述符。它们在内核中不保证有相同的底层开放文件描述符（除了一组有限的设备类型）。
- 每当 `Active`或空闲状态改变时，`PropertyChanged`信号就会发出，客户端可以监听。
- 当会话被要求锁定/解锁屏幕时，`Lock`/`Unlock`被发送。会话的会话管理器应该监听这个信号并采取相应的行动。这个信号是作为`Lock()`和`Unlock()`方法的结果而分别发送的。

### 属性

- `Id`编码会话的ID。
- `User`编码这个会话所属用户的用户ID。这是一个由Unix UID和对象路径组成的结构。
- `Name`对用户名进行编码。
- `Timestamp`和`TimestampMonotonic`分别以`CLOCK_REALTIME`或`CLOCK_MONOTONIC`编码创建会话时以微秒表示的UNIX时间戳。
- `VTNr`编码会话的虚拟终端号，如果有的话，否则为0。
- `Seat`编码这个会话所属的seat（如果有）。这是一个由ID和seat对象路径组成的结构。
- `TTY`编码会话的内核TTY路径，如果这是一个文本登录。如果不是的话，这是一个空字符串。
- `Display`编码X11显示名称，如果这是一个图形登录。如果不是，这就是一个空字符串。
- `Remote`编码会话是本地还是远程。
- `RemoteHost`和`RemoteUser`编码远程主机和用户，如果这是一个远程会话，则为空字符串。
- `Service`编码注册该会话的PAM服务名称。
- `Desktop`描述会话中运行的桌面环境（如果已知）。
- `Scope`包含这个会话的systemd范围单位名称。
- `Leader`编码了注册会话的进程的PID。
- `Audit`编码该会话的内核审计会话ID（如果有审计的话）。
- `Type` 编码会话类型。它是"`unspecified`"（用于cron PAM会话等）、"`tty`"（用于文本登录）或"`x11`"/"`mir`"/"`wayland`"（用于图形登录）之一。
- `Class` 编码会话类别。它是"`user`"（用于普通用户会话）、"`greeter`"（用于显示管理器伪会话）或"`lock-screen`"（用于显示锁定屏幕）中的一个。
- `Active`是一个布尔值，如果会话处于激活状态，即当前处于前台，则为真。由于`State`的存在，这个字段是半冗余的。
- `State`编码会话状态，是 "在线"、"活动 "或 "关闭 "之一。参见[sd_session_get_state(3)](https://www.freedesktop.org/software/systemd/man/sd_session_get_state.html#)了解更多关于状态的信息。
- `IdleHint`, `IdleSinceHint`, 和`IdleSinceHintMonotonic`封装了这个会话的空闲提示状态，类似于Manager对象上的各自属性对整个系统的作用。
- `LockedHint`显示这个会话的锁定提示状态，如上面描述的`SetLockedHint()`方法所设置。
当会话被要求锁定/解锁屏幕时，`Lock`/`Unlock`被发送。会话的会话管理器应该监听这个信号并采取相应的行动。这个信号是作为`Lock()`和`Unlock()`方法的结果而分别发送的。

## 例子

**Example 1. Introspect the logind manager on the bus**

$ gdbus introspect --system --dest org.freedesktop.login1 \

  --object-path /org/freedesktop/login1

or

$ busctl introspect org.freedesktop.login1 /org/freedesktop/login1

**Example 2. Introspect the default seat on the bus**

$ gdbus introspect --system --dest org.freedesktop.login1 \

  --object-path /org/freedesktop/login1/seat/seat0

or

$ busctl introspect org.freedesktop.login1 /org/freedesktop/login1/seat/seat0

Seat "`seat0`" is the default seat, so it'll be present unless local configuration is made to reassign all devices to a different seat. The list of seats and users can be acquired with **loginctl list-sessions**.

**Example 3. Introspect a single user on the bus**

$ gdbus introspect --system --dest org.freedesktop.login1 \

  --object-path /org/freedesktop/login1/user/_1000

or

$ busctl introspect org.freedesktop.login1 /org/freedesktop/login1/user/_1000

**Example 4. Introspect ****`org.freedesktop.login1.Session`**** on the bus**

$ gdbus introspect --system --dest org.freedesktop.login1 \

  --object-path /org/freedesktop/login1/session/45

or

$ busctl introspect org.freedesktop.login1 /org/freedesktop/login1/session/45

