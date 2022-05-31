---
title: Font
description: 
published: true
date: 2022-05-31T03:16:28.773Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:55:39.262Z
---

## Summary

Computer font is a collection of data representing glyphs and characters, usually stored in files.

The term "font" and "typeface" come from the typography and calligraphy. The former refers to a set of shapes that have the same styles and sizes, and the later indicates a collection of one or more fonts in different sizes.

## Basic knowledge of fonts

### Classification of fonts

There are mainly two types of font: the bitmap font and vector font.

#### Bitmap fonts

Characters of each style and of each size consists of matrix of dots or pixels. Because bitmaps cannot be scaled directly, fonts of a specified size can only clearly display in a fixed resolution. This, however, does not prevent Chinese characters to perform well in form of bitmap with size ranging from 12 to 16 px. Commonly used bitmap fonts are bdf, pcf, fnt and hbf.

#### Vector fonts

Characters are described using Bezier curves, drawing directives and mathematical expression, so that they can be scaled to any size.

### Terms used in font styles

- Sans-serif = Bold face (for Asian characters)

- Serif = Light face (for Asian characters)

- Monospace =  All characters have a unique width

### Commonly seen font types

- bdf and bdf.gz: Bitmap Distribution Format. A kind of bitmap font, compressed with gzip.

- pcf and pcf.gz: Portable Compiled Format. A kind of bitmap font, compressed with gzip.

- psf，psfu，psf.gz and psfu.gz: PC Screen Font. A kind of bitmap font, compressed with gzip. The Unicode version cannot be used on X.org.

- pfa and pfb: PostScript Font, for ASCII or Binary. A kind of vector font, with print command included.

- ttf: TrueType Font. A kind of vector font, replacing PostScript font.

- otf: OpenType Font. A kind of TrueType font with print command included.

In most cases, the technical differences between TrueType and OpenType can be ignored. Some extended TrueType fonts are in fact OpenType fonts.

## Installation of fonts

### Quick installation

Double click a font file in the file manager, a window will appear for previewing the font. Click on "Install Font" to install it.

### Copy font files to system directory

Execute in terminal:

    sudo dde-file-manager /usr/share/fonts  

and copy the font file to here.

### Install for repository

You can also use packages provided in repository. For example, to install fonts of WenQuanYi series, execute in terminal:

    sudo apt-get install xfonts-wqy

## Font configurations

### Font and configuration files

Where font files are stored:

- For system fonts: /usr/share/fonts
- For user customized font: ~/.fonts

Where font configurations are stored:

- For system fonts: /etc/fonts/fonts.conf
- For user customized font: ~/.fonts.conf

System configurations will not be used if any user configuration is found. For security and convenience concerns, it is recommended to use user configurations.

### Configuring with graphical tools

Install gnome tweak tool for further configurations. You may also modify the configuration file for a specific font.

To restore the default font configurations, execute in terminal:

    rm -rf ~/.fonts.conf

## Delete fonts

Delete the font files will remove it from the system. You may also uninstall it using apt-get if it was installed from the repository.

## References

[Debian Wiki:Fonts](http://wiki.debian.org/Fonts)

[Ubuntu百科:字体](http://wiki.ubuntu.com.cn/%E5%AD%97%E4%BD%93)

[造字工房系列](http://www.makefont.com/fonts.html)

[文泉驿系列](http://wenq.org/wqy2/index.cgi?%E9%A6%96%E9%A1%B5)
