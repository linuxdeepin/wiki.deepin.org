---
title: System_language
description: 
published: true
date: 2022-05-07T02:31:18.739Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:57:24.434Z
---

## Summary

Locale is a set of strings used to define the language environment of users in Linux. The run-time environment, including language used, units, date and text format, is adjusted according to the locale settings.

Locale settings also include the char set setting, so it is important to choose a correct locale for languages that use non-ASCII characters.

## Format

Locales are defined in the following format:

    <lang>_<territory>.<codeset>[@<modifiers>]

There are several types of locale concerning:

* Language class : LC_CTYPE
* Numeric format: LC_NUMERIC
* Habits in comparing and sorting: LC_COLLATE
* Time format: LC_TIME
* Unit of money: LC_MONETARY
* Message format: LC_MESSAGES
* Format used to write names: LC_NAME
* Format used to write addresses: LC_ADDRESS
* Format used to write telephone numbers: LC_TELEPHONE
* Units used in measurement: LC_MEASUREMENT
* Deefault paper size: LC_PAPER
* Summary of this locale: LC_IDENTIFICATION

## View locales

To see all available locale, execute in terminal:

    locale -a

To see the locale used at the time:

    locale

## Configuration

Here we take exmple of setting Chinese locale for the system.

First, edit locale configuration file. Execute in terminal:

    sudo gedit  /var/lib/locales/supported.d/local

This is a list of codes all activated languages.

Append the following content to it:

    zh_CN.UTF-8 UTF-8
    en_US.UTF-8 UTF-8

Then generate the locale information:

    sudo locale-gen --purge

Edit the global locale setting:

    sudo gedit /etc/default/locale

and append following lines to it:

    LANG="zh_CN.UTF-8"
    LANGUAGE="zh_CN:en"
    LC_NUMERIC="zh_CN.UTF-8"
    LC_TIME="zh_CN.UTF-8"
    LC_MONETARY="zh_CN.UTF-8"
    LC_PAPER="zh_CN.UTF-8"
    LC_IDENTIFICATION="zh_CN.UTF-8"
    LC_NAME="zh_CN.UTF-8"
    LC_ADDRESS="zh_CN.UTF-8"
    LC_TELEPHONE="zh_CN.UTF-8"
    LC_MEASUREMENT="zh_CN.UTF-8"

Now we can reboot the computer to see if the system language has been changed to Chinese. We can use command "locale" to verify it:

    $ locale
    LANG=zh_CN.UTF-8
    LANGUAGE=zh_CN:en
    LC_CTYPE="zh_CN.UTF-8"
    LC_NUMERIC=zh_CN.UTF-8
    LC_TIME=zh_CN.UTF-8
    LC_COLLATE="zh_CN.UTF-8"
    LC_MONETARY=zh_CN.UTF-8
    LC_MESSAGES="zh_CN.UTF-8"
    LC_PAPER=zh_CN.UTF-8
    LC_NAME=zh_CN.UTF-8
    LC_ADDRESS=zh_CN.UTF-8
    LC_TELEPHONE=zh_CN.UTF-8
    LC_MEASUREMENT=zh_CN.UTF-8
    LC_IDENTIFICATION=zh_CN.UTF-8

## Frequently asked question

### Gibberish in TTY for Chinese characters

TTY does not have native support for Chinese characters. You may try Fbterm for displaying Chinese in TTY.

## Reference

[Ubuntu中文百科:Locale](http://wiki.ubuntu.org.cn/Locale)
