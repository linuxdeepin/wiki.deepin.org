---
title: Wired_network
description: 
published: true
date: 2022-05-20T10:41:47.252Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:53.292Z
---

## Summary

The wired network is the mostly used network environment. This entry introduces briefly how to set up a wired connection.

## Configuration

### Automatic configuration

deepin will detect automatically the current network environment, including all wired connection, and will finish the configuration without intervention from users.

### Manual configuration

Sometimes deepin does not detect the wired connection, so we need to provide the parameter of the network card. Just go to the Control Center -> Network -> Wired Network -> Add Settings, and type the MAC address of your network card.

The MAC address can be obtained from the output of command `ifconfig`.

To set static IP address and DNS, click on "Method" in "IPv4" or "IPv6", and choose "Manual", then you can see the input box for manual address configuration.

Click "Save" after you have finished configurations. deepin with attempt to apply these new settings.
