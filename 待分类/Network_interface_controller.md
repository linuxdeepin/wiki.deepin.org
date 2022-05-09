---
title: 待分类/Network_interface_controller
description: 
published: true
date: 2022-05-07T07:48:24.821Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:39:05.995Z
---

##Introduction
A network interface controller (NIC, also known as a network interface card, network adapter, LAN adapter or physical network interface,[1] and by similar terms) is a computer hardware component that connects a computer to a computer network.[2]

Early network interface controllers were commonly implemented on expansion cards that plugged into a computer bus. The low cost and ubiquity of the Ethernet standard means that most newer computers have a network interface built into the motherboard.

Modern network interface controllers offer advanced features such as interrupt and DMA interfaces to the host processors, support for multiple receive and transmit queues, partitioning into multiple logical interfaces, and on-controller network traffic processing such as the TCP offload engine.
##Purpose

A Madge 4/16 Mbit/s Token Ring ISA-16 NIC
The network controller implements the electronic circuitry required to communicate using a specific physical layer and data link layer standard such as Ethernet, Fibre Channel, Wi-Fi or Token Ring. This provides a base for a full network protocol stack, allowing communication among small groups of computers on the same local area network (LAN) and large-scale network communications through routable protocols, such as Internet Protocol (IP).

The NIC allows computers to communicate over a computer network, either by using cables or wirelessly. The NIC is both a physical layer and data link layer device, as it provides physical access to a networking medium and, for IEEE 802 and similar networks, provides a low-level addressing system through the use of MAC addresses that are uniquely assigned to network interfaces.

Although other network technologies exist, IEEE 802 networks including the Ethernet variants have achieved near-ubiquity since the mid-1990s.

##Implementation

12 early ISA 8 bit and 16 bit PC network cards. The lower right-most card is an early wireless network card, and the central card with partial beige plastic cover is a PSTN modem.
Whereas network controllers used to operate on expansion cards that plugged into a computer bus, the low cost and ubiquity of the Ethernet standard means that most new computers have a network interface built into the motherboard. Newer server motherboards may even have dual network interfaces built-in. The Ethernet capabilities are either integrated into the motherboard chipset or implemented via a low-cost dedicated Ethernet chip, connected through the PCI (or the newer PCI Express) bus. A separate network card is not required unless additional interfaces are needed or some other type of network is used.

The NIC may use one or more of the following techniques to indicate the availability of packets to transfer:

Polling is where the CPU examines the status of the peripheral under program control.
Interrupt-driven I/O is where the peripheral alerts the CPU that it is ready to transfer data.
Also, NICs may use one or more of the following techniques to transfer packet data:

Programmed input/output is where the CPU moves the data to or from the NIC to memory.
Direct memory access (DMA) is where some other device other than the CPU assumes control of the system bus to move data to or from the NIC to memory. This removes load from the CPU but requires more logic on the card. In addition, a packet buffer on the NIC may not be required and latency can be reduced. There are two types of DMA: third-party DMA in which a DMA controller other than the NIC performs transfers, and bus mastering where the NIC itself performs transfers.
An Ethernet network controller typically has an 8P8C socket where the network cable is connected. Older NICs also supplied BNC, or AUI connections. A few LEDs inform the user of whether the network is active, and whether or not data transmission occurs. Ethernet network controllers typically support 10 Mbit/s Ethernet, 100 Mbit/s Ethernet, and 1000 Mbit/s Ethernet varieties. Such controllers are designated as "10/100/1000", meaning that they can support a notional maximum transfer rate of 10, 100 or 1000 Mbit/s. 10 Gigabit Ethernet NICs are also available, and, as of November 2014, are beginning to be available on computer motherboards.[3][4]

##Performance and advanced functionality

An ATM network interface.

Intel 82574L Gigabit Ethernet NIC, a PCI Express ×1 card, which provides two hardware receive queues[5]
Multiqueue NICs provide multiple transmit and receive queues, allowing packets received by the NIC to be assigned to one of its receive queues. Each receive queue is assigned to a separate interrupt; by routing each of those interrupts to different CPUs/cores, processing of the interrupt requests triggered by the network traffic received by a single NIC can be distributed among multiple cores, bringing additional performance improvements in interrupt handling. Usually, a NIC distributes incoming traffic between the receive queues using a hash function, while separate interrupts can be routed to different CPUs/cores either automatically by the operating system, or manually by configuring the IRQ affinity.[6][7]

The hardware-based distribution of the interrupts, described above, is referred to as receive-side scaling (RSS).[8]:82 Purely software implementations also exist, such as the receive packet steering (RPS) and receive flow steering (RFS).[6] Further performance improvements can be achieved by routing the interrupt requests to the CPUs/cores executing the applications which are actually the ultimate destinations for network packets that generated the interrupts. That way, taking the application locality into account results in higher overall performance, reduced latency and better hardware utilization, resulting from the higher utilization of CPU caches and fewer required context switches. Examples of such implementations are the RFS[6] and Intel Flow Director.[8]:98,99[9][10][11]

With multiqueue NICs, additional performance improvements can be achieved by distributing outgoing traffic among different transmit queues. By assigning different transmit queues to different CPUs/cores, various operating system's internal contentions can be avoided; this approach is usually referred to as transmit packet steering (XPS).[6]

Some NICs[12] support transmit and receive queues without kernel support allowing the NIC to execute even when the functionality of the operating system of a critical system has been severely compromised. Those NICs support:

Accessing local and remote memory without involving the remote CPU.
Accessing local and remote I/O devices without involving local/remote CPU. This capability is supported by device-to-device communication over the I/O bus, present in switched-based I/O interconnects.
Controlling access to local resources such as control registers and memory.
Some products feature NIC partitioning (NPAR, also known as port partitioning) that uses SR-IOV to divide a single 10 Gigabit Ethernet NIC into multiple discrete virtual NICs with dedicated bandwidth, which are presented to the firmware and operating system as separate PCI device functions.[13][14] TCP offload engine is a technology used in some NICs to offload processing of the entire TCP/IP stack to the network controller. It is primarily used with high-speed network interfaces, such as Gigabit Ethernet and 10 Gigabit Ethernet, for which the processing overhead of the network stack becomes significant.[15]

Some NICs offer integrated field-programmable gate arrays (FPGAs) for user-programmable processing of network traffic before it reaches the host computer, allowing for significantly reduced latencies in time-sensitive workloads.[16] Moreover, some NICs offer complete low-latency TCP/IP stacks running on integrated FPGAs in combination with userspace libraries that intercept networking operations usually performed by the operating system kernel; Solarflare's open-source OpenOnload network stack that runs on Linux is an example. This kind of functionality is usually referred to as user-level networking.