---
title: VPN_Configuration
description: 
published: true
date: 2022-05-20T10:41:04.335Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:36.510Z
---

## Summary

VPN network is the most common network environment for private networks. This entry gives a brief introduction to configurations of VPN network environment.

## Configuration

Go to the Control Center -> Network -> VPN -> Create VPN, and select a VPN type (PPTP is the most commonly used type). The detailed options may vary according to the VPN type you choose, but there are some common parameters you need to provide:

* Name: An arbitrary name for this VPN connection

* Gateway: The gateway address (obtained from the VPN provider)

* Username: The account name used for authentication

* Password: The account password used for authentication

To set a static IP address, click on "Method" in "IPv4", and select "manual", then you can see the input box for manual address configuration.

Click "Save" after you have finished configurations. deepin with attempt to apply these new settings.
