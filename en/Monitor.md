---
title: Monitor
description: 
published: true
date: 2022-05-07T02:30:14.687Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:56:12.342Z
---

## Overview

The monitor, or the display device, is a kind of output device which displays the information in pictorial form

There are many types of monitors, each with a different technology behind it. Some commonly seen types are:

- Cathode Ray Tube (CRT) monitor
- Liquid Crystal Display (LCD)
- Plasma Display Panel (PDP)
- Projection display
- Stereoscopic imaging display
- Light Emitting Diode (LED) display
- Organic light-emitting diode (OLED) monitors
- E-paper

## View monitor information

Execute in terminal:

    xrandr

You can refer to the document of xrandr to get more information of the usage:

    man xrandr

## Multi-screen display

Sometimes we may use a external display to extend the output of our desktop, and the difference in resolution of these two monitors could be a problem.

Here we take Acer G236HLbd display as an example. The default resolution of this monitor should be 1920*1080, but it turns out to be 1024*768 when connected to deepin, and there is no option for a higher solution in the Control Center. So we need to "create" a new resolution for it, using command "cvt" and "xrandr".

    xrandr --output VGA-1 --right-of LVDS-1    Set the display "VGA-1" (the external display) on the right side of "LVDS-1"
    cvt 1920 1080  ## Show the possible setting related to the resolution "1920*1080"
    sudo xrandr --newmode "1920x1080_60.00″ 173.00 1920 2048 2248 2576 1080 1083 1088 1120 -hsync +vsync"  ## Append the output of the last command to the option "--newmode”
    sudo xrandr --addmode VGA-1 “1920x1080_60.00″  ## Associate the resolution "1920*1080"  with "VGA-1"
    xrandr –output VGA-1 –mode 1920x1080_60.00 –rate 60  ## Switch to the new resolution

Note: The multiplication sign "*" is expressed using "x" here.