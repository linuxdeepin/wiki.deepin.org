---
title: 待分类/Hardware_Probe
description: 
published: true
date: 2022-05-07T07:47:22.464Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:36:07.911Z
---

Creating a hardware probe allows you to find out the details of the computer's internal structure, check operability of devices and collect logs for the developers to help identify and fix hardware related problems. If the system failed to find a driver for some device in your computer, the probe will suggest the appropriate version of the Linux kernel according to the [LKDDb](https://cateee.net/lkddb/) or third-party drivers.

## Create a probe

Command line to create a probe:

`sudo hw-probe -all -upload`

Output:

`Probe for hardware ... Ok`

`Reading logs ... Ok`

`Uploaded to DB, Thank you!`

`Probe URL: https://linux-hardware.org/?probe=c5063bb936`

## Install

You can install a [Deb package](https://github.com/linuxhw/hw-probe/blob/master/INSTALL.md#install-on-debian) or use universal packages: [AppImage](https://github.com/linuxhw/hw-probe#appimage), [Docker](https://hub.docker.com/r/linuxhw/hw-probe/), [Snap](https://snapcraft.io/hw-probe) or [Flatpak](https://flathub.org/apps/details/org.linux_hardware.hw-probe).

## What can you do with it?

Provide a probe link when asking for support or advice from your friends to avoid a bunch of additional questions about your system setup. All necessary info about your computer configuration and logs is already collected in the probe.

## Hardware database

In addition to simplifying communication with the distribution support team, a public [hardware database](https://linux-hardware.org/?distro=Deepin) is created on the basis of hardware probes from different users, where you can find experience of other users with similar hardware components.

## Statistics

Also, based on hardware probes from different users, a statistical analysis of [poorly supported devices in Linux](https://github.com/linuxhw/HWInfo) and the [reliability of hard drives](https://github.com/linuxhw/SMART) are performed.

## Debugging of the ACPI subsystem

`sudo apt-get install acpica-tools`

`sudo hw-probe -all -upload -decode-acpi`

See *acpidump_decoded* extra log in your probe.

## Privacy

Collected logs are cleared from private info. You can safely share a probe link with anybody.
