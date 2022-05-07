---
title: Hard_Disk
description: 
published: true
date: 2022-05-07T02:15:50.649Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:36:01.854Z
---

## Definitioin 
A hard disk drive (sometimes abbreviated as Hard drive, HD, or HDD) is a non-volatile memory hardware device that permanently stores and retrieves data on a computer.

## Components

- The head actuator
- Read/write actuator arm
- Read/write head
- Spindle
- Platter.

On the back of a hard drive is a circuit board called the disk controller or interface board and is what allows the hard drive to communicate with the computer.

![1|hard-disk-components](/images/8/88/Hard-disk-components.png)

<br> Note: The above picture is an example of a traditional hard drive and not an SSD.</br>

## Location

All primary computer hard drives are found inside a computer case and are attached to the computer motherboard using an ATA, SCSI, or SATA cable, and are powered by a connection to the PSU (power supply unit).

<br> Note: Some portable and desktop computers may have newer flash drives that connect directly to the PCIe interface or another interface and not use a cable.</br>

## Size

The hard drive is typically capable of storing more data than any other drive, but its size can vary depending on the type of drive and its age.

<br> Older hard drives had a storage size of several hundred megabytes (MB) to several gigabytes (GB). </br>

<br> Newer hard drives have a storage size of several hundred gigabytes to several terabytes (TB). Each year, new and improved technology allows for increasing hard drive storage sizes.</br>

<br> Note: If you are trying to find the physical dimensions of a hard drive, their physical sizes are typically either 3.5" for desktop computers or 2.5" for laptops. SSDs range from 1.8" to 5.25".</br>

## How It Works

Data sent to and read from the hard drive is interpreted by the disk controller, which tells the hard drive what to do and how to move the components in the drive. 

<br> When the operating system needs to read or write information, it examines the hard drive's File Allocation Table (FAT) to determine file location and available write areas.</br>

<br> Once they have been determined, the disk controller instructs the actuator to move the read/write arm and align the read/write head. Because files are often scattered throughout the platter, the head needs to move to different locations to access all information.</br>

![1|Read_and_write](/images/6/6b/Read_and_write.png)

ll information stored on a traditional hard drive, like the above example, is done magnetically. After completing the above steps, if the computer needs to read information from the hard drive, it would read the magnetic polarities on the platter. One side of the magnetic polarity is 0, and the other is 1. Reading this as binary data, the computer can understand what the data is on the platter. For the computer to write information to the platter, the read/write head aligns the magnetic polarities, writing 0's and 1's that can be read later.