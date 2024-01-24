---
title: On-board GPU support for deepin on RISC-V
description: This project aims to provide support for the on-board GPU (Imagination PVR) driver on most RISC-V development boards based on the deepin V23, and try to merge the relevant patches into the deepin mainline repository.
published: true
date: 2024-01-24T03:36:20.735Z
tags: 
editor: markdown
dateCreated: 2024-01-24T03:19:53.370Z
---

## Project Title/Description

On-board GPU support for deepin on RISC-V
  
### Detailed Project Description (2-5 sentences)

This project aims to provide support for the on-board GPU (Imagination PVR) driver on most RISC-V development boards based on the deepin V23, and try to merge the relevant patches into the deepin mainline repository.

### Background

Imagination PVR is a major GPU vendor on the RISC-V platform, and most RISC-V development boards use its solutions. However, Imagination's GPU drivers have not yet been mainlined, and most mainline linux distros still use software rendering based on llvmpipe/swrast on RISC-V development boards.

One of the goals of deepin V23 is to complete its RISC-V support. As a linux distribution focused on the desktop experience, GPU driver adaptation is an important part of multi-architecture support.

### Expected Outcomes

The possible outcomes include:
- Contributions to a part of the deepin main repository
- A seperated repository for non-mainline packages
- A deepin v23 disk image that can be displayed on RISC-V devices, with packages that follow the deepin mainline as much as possible
- Community sharing, blog posts, reports, or articles related to the project

Recommended RISC-V devices:
- Sipeed LicheePi 4A (TH1520)
- StarFive VisionFive 2 (JH7110)

### What the intern will learn

The intern will learn how to:
- Familiarize themselves with the packaging of debian-based distros, master how to debug packaging issues, and become a contributor to the deepin community
- Understand the driver adaptation process of development boards for linux distros, and contribute to the RISC-V ecosystem
- Have a basic understanding of the linux graphics stack and RISC-V architecture

### Skills Required/Preferred

Required skills for intern.

- Experience using linux and its desktop environment (daily user is preferred)
- Ability to communicate in English, primarily in writing
- Interest in open source software and the linux desktop operating system
- Familiarity with at least one programming languages

Preferred skill for intern.

- Packaging for linux distros (especially debian-based distros) or contributing to any linux distro
- Knowledge of linux graphics stack, e.g. OpenGL ES and Vulkan.
- Experience in adapting RISC-V and its development boards to linux operating systems, especially desktop environments

### Possible Mentors

- [YukariChiba@GitHub/Telegram](https://github.com/YukariChiba)

### Expected Size of Project (90, 175, or 350 hours)

350 hours

### Difficulty Rating (Easy, Medium, Hard)

Hard