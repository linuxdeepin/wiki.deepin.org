---
title: Sound_theme
description: 
published: true
date: 2022-05-31T03:17:54.053Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:17.128Z
---

## Summary

Sound effect is a set of audio clips played when the computer is dealing with different task. Different sound effect can bring a different feeling to users.

This gives brief introduction to settings of sound effects (especially the login sound).

## Configuration files

It is convenient to use the auto-start function provided by most desktop environment to play a specified audio during login. The configuration files used are .desktop files located in ~/.config/autostart (for a specified user) and /etc/xdg/autostart (for all users).

## Methods

### Startup sound

First install package gnome-session-canberra:

    sudo apt-get install gnome-session-canberra

Then open the terminal and type:

    gedit $HOME/.config/autostart/loginsound.desktop 

this will create a new file named "loginsound.desktop" in the directory of personal configurations.

Add the following content to it:

    [Desktop Entry]
    Name=GNOME Login Sound
    Type=Application
    Exec=/usr/bin/canberra-gtk-play --file="XXX" --description="GNOME Login sound"
    Comment=Startup Sound

where "XXX" is the path of the audio file you wold like to play after login.
