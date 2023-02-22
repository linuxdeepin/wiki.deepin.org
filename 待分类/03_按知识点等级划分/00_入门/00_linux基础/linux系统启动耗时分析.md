---
title: linux系统启动耗时分析
description: linux系统启动耗时分析   systemd-analyze
published: true
date: 2023-02-22T02:36:24.177Z
tags: 
editor: markdown
dateCreated: 2023-02-21T10:04:08.266Z
---

# systemd-analyze
> systemd-analyze 命令可以方便的记录系统启动耗时详情

**可以通过：**
```systemd-analyze plot > start_analyze.svg```命令来输出启动耗时的矢量图，如下图所示：

![starttimeanalyze.svg](/starttimeanalyze.svg)
**可以通过：**
``` systemd-analyze blame``` 来获得详细的进程耗时排名信息：
```babyfengfjx@babyfengfjx:~$ systemd-analyze blame
8.382s vmware-USBArbitrator.service
8.140s upower.service
6.655s NetworkManager-wait-online.service
5.138s man-db.service
4.477s deepin-authenticate.service
2.042s plymouth-quit-wait.service
1.222s vmware.service
1.172s udisks2.service
1.138s deepin-accounts-daemon.service
1.122s smartmontools.service
 996ms apt-daily-upgrade.service
 974ms logrotate.service
 876ms systemd-journal-flush.service
 756ms nvidia-persistenced.service
 613ms user@1000.service
 600ms accounts-daemon.service
 600ms ModemManager.service
 594ms NetworkManager.service
 591ms laptop-mode.service
 591ms gpu-manager.service
 582ms vboxdrv.service
 495ms polkit.service
 477ms apparmor.service
 425ms dde-filemanager-daemon.service
 421ms colord.service
 380ms lightdm.service
 341ms systemd-modules-load.service
 315ms ipwatchd.service
 302ms apt-daily.service
 236ms winbind.service
 228ms uengine-container.service
 226ms libvirt-guests.service
 180ms smbd.service
 143ms systemd-fsck@dev-disk-by\x2duuid-4bd99115\x2d99b6\x2d4ab7\x2dae9b\x2d72bfa8c9020d.servic
 142ms dev-nvme0n1p4.device
 131ms lvm2-monitor.service
 126ms nmbd.service
  97ms bluetooth.service
  96ms avahi-daemon.service
  92ms fprintd.service
  81ms systemd-logind.service
  81ms systemd-machined.service
  80ms wpa_supplicant.service
  77ms lxc.service
  68ms libvirtd.service
  51ms home.mount
  51ms data.mount
  50ms systemd-udev-trigger.service
  49ms systemd-udevd.service
  41ms systemd-fsck@dev-disk-by\x2duuid-75d72b1d\x2d874c\x2d4a3a\x2da715\x2d986a996ee814.servic
  39ms rsyslog.service
  38ms systemd-timesyncd.service
  37ms systemd-fsck@dev-disk-by\x2duuid-048fef2c\x2ded60\x2d4e39\x2daeef\x2d0f43b208f9ad.servic
  35ms opt.mount
  32ms systemd-journald.service
  27ms systemd-fsck@dev-disk-by\x2duuid-FD8E\x2d6C7A.service
  24ms root.mount
  21ms plymouth-read-write.service
  20ms systemd-tmpfiles-setup-dev.service
  20ms systemd-tmpfiles-setup.service
  19ms alsa-restore.service
  19ms networking.service
  18ms systemd-binfmt.service
  18ms systemd-random-seed.service
  14ms plymouth-start.service
  13ms systemd-update-utmp.service
  13ms lm-sensors.service
  11ms hddtemp.service
  11ms dev-disk-by\x2duuid-9ee15dc1\x2d16af\x2d4dbb\x2d9161\x2d6b17bee2461e.swap
  10ms pppd-dns.service
   9ms systemd-tmpfiles-clean.service
   9ms dev-hugepages.mount
   9ms proc-sys-fs-binfmt_misc.mount
   8ms dev-mqueue.mount
   8ms sys-kernel-debug.mount
   7ms sys-kernel-tracing.mount
   7ms modprobe@drm.service
   7ms blk-availability.service
   7ms kmod-static-nodes.service
   7ms systemd-remount-fs.service
   6ms modprobe@configfs.service
   6ms user-runtime-dir@1000.service
   5ms lmt-poll.service
   5ms boot.mount
   5ms modprobe@fuse.service
   5ms systemd-sysusers.service
   5ms systemd-user-sessions.service
   5ms systemd-update-utmp-runlevel.service
   5ms recovery.mount
   4ms lxc-net.service
   4ms ufw.service
   3ms dev-loop0.device
   3ms boot-efi.mount
   3ms systemd-sysctl.service
   3ms vboxautostart-service.service
   3ms ifupdown-pre.service
   3ms vboxballoonctrl-service.service
   2ms var.mount
   2ms systemd-rfkill.service
   2ms vboxweb-service.service
   1ms sys-fs-fuse-connections.mount
   1ms deepin-login-sound.service
   1ms sys-kernel-config.mount
```