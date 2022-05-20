---
title: Dial-up_network
description: 
published: true
date: 2022-05-20T10:36:19.764Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:55:09.541Z
---

## Summary

The dial-up network is commonly used in families. This entry introduces briefly how to set up a dial-up (PPPoE) network.

## Configuration

### Using a graphical interface

Just go to the Control Center -> Netwrok -> DSL -> Create PPPoE Connection. The fields we need to fill are:

* Name: An arbitrary name for this connection

* Username: Account name provided by your ISP

* Service: An arbitrary name for the ISP of this connection

* Password: Account password provided by your ISP

To set a static IP or DNS, click "Method" in "IPv4", and choose "Manual", then you can see the input box for manual address configuration.

Click "Save" after you have finished configurations. deepin with attempt to apply these new settings.

### Using commands

To connection to ADSL modem using PPPoE, execute in terminal:

    sudo pppoeconf

and follow the instruction. In most cases, when account name and password are provided, the default settings should be nough for you to deal with a common dial-up network.

## Some useful commands

To bring up a configured ADSL connection:

    sudo pon dsl-provider

To disconnect a ADSl connection:

    sudo poff dsl-provider
