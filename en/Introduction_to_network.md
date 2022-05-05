---
title: Introduction_to_network
description: 
published: true
date: 2022-04-21T03:56:07.531Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:56:05.440Z
---



## Summary

This entry gives brief introduction to computer network.

## Terms

### Network types

There are generally two types of network: the LAN and the Internet.

#### LAN

The Local Area Network (LAN) refers to the network consisting of several computers in an area. Different size of LAN are categorized into different subtypes, like Personal Network (PAN), Metropolitan Area Network (MAN) and Wide Area Network (WAN).

#### Internet

The Internet is the huge network formed by connections among LANs. These LANs shares a group of common protocols, forming a gigantic logical network.

### Types of network terminals

There are generally three types of terminals on the Internet:

* Server: A computer, usually of high-level equipment, that provides services for other devices

* Workstation: A computer that provides service for other computers

* Client: A computer that demands services from servers; usually a personal computer

## TCP/IP

TCP/IP is a set of protocol that the whole Internet is based one. These protocol origin from the ARPA network project by DOD of the United States, thus the TCP/IP model is sometimes called DOD model. It also indicates literally the two main protocols: the Transmission Control Protocol (TCP) and the Internet Protocol (IP).

In the January 1, 1983, TCP/IP replaced the old Network Control Protocol (NCP) in the ARPA network (the predecessor of the Internet), becoming the foundation of the Internet today. The TCP/IP was first designed by Vinton Cerf and Robert E. Kahn, then won out from the other network scheme like OSI model suggested by the ISO. The birth of important and reliable tools like HTML and Mosaic made it possible for the Internet to gain huge progress in the middle of the 90s. And with the increasing size of the global network, the popular IPv4 protocol has already reached it limit of capacity.


## IP address

The IP address is the addressing scheme for computers on the Internet. It also refers to the unique address assigned to each computer. There are two common protocols for assignment of IP address:

- IPv4 (Internet Protocol version 4): the fourth revision of the Internet protocol, and the first version that has been widely deployed.
- IPv6 (Internet Protocol version 6): The protocol for data gram exchanging in the network layer of the OSI model, also the next generation protocol replacing IPv4.

### Obtaining of IP address

Static address: The address predefined by administrator and assigned to the specified device. Unless modified by the administrator, it will never change, and maintain during rebooting or refreshing of network interfaces.

Dynamic address: Obtained after finishing a certain process during connecting to the network. Devices may get different addresses every time they are connected.

###The format of IP address

An IP address consists of 32 bits, divided into 4 segments. Each segment is represented by a decimal number ranging for 0 to 255, and segments are separated by dots. For example: 159.226.1.1.

An IP address can also be treated in two part: the network address and the host address, according to the type it belongs to.

There are five types of IP, ranging from A to E.

* A-type: The first segment ranges from 1 to 127. There are 128 A-type IP groups, with each group available to 16387064 hosts
* B-type: The first segment ranges from 128 to 191. There are 64 B-type IP groups, with each group available to 64516 hosts
* C-type: The first segment ranges from 192 to 223. There are 32 C-type IP groups, with each group available to 254 hosts
* D-type: The first segment ranges from 224 to 239. They are mainly used for transmission to multi-target
* E-type: The first segment ranges from 240 to 254. There are reserved for network experiment and evelopment

### Subnetwork mask

A subnetwork mask is a bit mask used to specify the range of the bits used for determine the host address in an IP. The default subnetwork mask for different types of IP address are:

- A-type IP: 255.0.0.0
- B-type IP:255.255.0.0
- C-type IP: 255.255.255.0

### Private networks

A private network is a network that uses private IP addresses while adopting RFC 1918 and RFC 4193.

A private network cannot be connected to the Internet without address forwarding. However, it reduces the cost when constructing a local network, as private addresses are free to use. Typical cases for private networks include:

* IP addresses that should not be accessed by computers on the Internet for security concerns
* LANs for families, schools and businesses.
* Private IP addresses used with NAT to compensate the lack of public IP addresses

The available private IPs are:

- 10.0.0.0 – 10.255.255.255 for A-type IP
- 172.16.0.0 – 172.31.255.255 for B-type IP
- 192.168.0.0 – 192.168.255.255 for C-type IP

## Gateway and router

### Gateway

Gateways are used to connect networks (either LANs or WANs) using different protocols of high layer. They serve as the translator of protocols, data, languages or even structures of different kinds. They may also re-pack the information received to meet different needs of the target system, while filtering the insecure data. In fact, they are the most complex network devices on the Internet.

### Router

Routers are devices for transfering data from source to destination. They work at the third layer (i.e. the network layer) of the OSI model.

## DNS service

Domain Name System (DNS) is a core service for mapping domains to IP address. It allows humans to access servers with their "names", rather the assigned IP addresses.

## MAC address

Media Access Control (MAC) Address, also known as the hardware address, is used to define the location of network devices.