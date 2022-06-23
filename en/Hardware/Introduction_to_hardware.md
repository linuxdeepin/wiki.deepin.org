---
title: Introduction_to_hardware
description: 
published: true
date: 2022-05-15T02:00:08.069Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:56:01.902Z
---

## Computer

Computers refer to modern electronic devices that can run according to the program, and process data with high speed. A computer consists of the hardware and the software. A computer without software is called a bare-metal computer.

According to the size and the mobility of computers, they can be divided into several types:

* Desktop computer

* Workstation: Personal computers of high-level, generally connected to the network

* Mobile computer: Similar to the laptop

* Laptop: Small and portable personal computers

* Netbook: Small and cheap laptops designed for web browsing

* Ultrabook: Thin and light laptops with long battery lives

* Tablet: Derivatives of laptops, with mouses replaced by touch screens

* Personal digital assistant (PDA): Pocket PCs

* Wearable computer: Conceptual products

This entry mainly covers the desktop computer and the laptop.

## Hardware component

A modern (desktop) computer generally consists of following components:

* [Monitor](Monitor)

* [Motherboard](Motherboard)

* [CPU](CPU)

* [RAM](RAM)

* PCI

* [Power Supply](Power_Supply)

* [Optical Disk Driver](Optical_Disk_Driver)

* [Hard Disk](Hard_Disk)

* [Keyboard](Keyboard)

* [Mouse](Mouse)

## Nomination of hardware in Linux

In Linux, each device is regarded as a file. For example, the legacy hard disk drive with IDE interface are named as hda, hdb, hdc or hdd, all located in the directory "/dev". You may also find other devices here like

* sda, sdb,... (for SCSI compatible disks)

* fd0, fd1, ... (for floppy disks)

* lp0, lp1, ... (for printers using parallel interface)

* eth0, eth1, ... (for Ethernet cards)

* tr0, tr1, ... (for ring network cards)

Network interfaces, unlike other devices, are not seen as a fixed file in /dev, but added to and removed from /dev dynamically according to their presence. When a network interface, saying a Ethernet card, is detected by the kernel, it is assigned with the first available name of that type of interface, usually "eth0".

## View hardware information

### Using graphical interfaces

[Hardinfo](Hardinfo): A software for viewing system information in Linux. Recommended.

[CPU-G](CPU-G): A utility for viewing hardware information, similar to the CPU-Z in Windows.

### Using Web interface

[[Hardware Probe]]: List hardware, check operability and find drivers. Global database of Deepin installations.

### Using command

Execute in terminal:

        dmesg ## View kernel messages related to hardware
        lspci ## List all PCI devices
        uptime ## Show general information of the running state of the system
        inxi -F ## List all information of your hardware

## View hardware temperature

[Psensor](Psensor): A utility for monitoring hardware temperature.

## References

[Wikipedia: Personal Computer](https://en.wikipedia.org/wiki/Personal_computer)

[Wikipedia: History of computing hardware](https://en.wikipedia.org/wiki/History_of_computing_hardware)

[It's Me,It's Fine](http://dofine.blogbus.com/logs/59190496.html)
