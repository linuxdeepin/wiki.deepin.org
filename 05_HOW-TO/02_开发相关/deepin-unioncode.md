---
title: deepin-unioncode 用户使用手册
description: 一款轻量级多语言跨平台兼容的集成开发环境，支持多种语言工程的构建、运行和调试。
published: true
date: 2023-09-04T09:47:34.534Z
tags: deepin-unioncode, unioncode, deepin-ide
editor: markdown
dateCreated: 2023-08-25T10:11:36.026Z
---

## 概述

deepin-unioncode是一款轻量级多语言跨平台兼容的集成开发环境，支持多种语言工程的构建、运行和调试。

## 使用入门

通过以下方式运行或关闭deepin-unioncode，或者创建deepin-unioncode的快捷方式。

### 运行deepin-unioncode

1. 单击任务栏上的启动器图标![deepin_launcher.svg](/05_HOW-TO/deepin-unioncode/deepin_launcher.svg)，进入启动器界面。
2. 上下滚动鼠标滚轮浏览或通过搜索，找到deepin-unioncode图标 <img src="/05_HOW-TO/deepin-unioncode/unioncode@128.png" alt="draw" style="zoom:20%;" />，单击运行。
3. 右键单击<img src="/05_HOW-TO/deepin-unioncode/unioncode@128.png" alt="draw" style="zoom:20%;" />，您可以：

 - 单击 **发送到桌面**，在桌面创建快捷方式。
 - 单击 **发送到任务栏**，将应用程序固定到任务栏。
 - 单击 **开机自动启动**，将应用程序添加到开机启动项，在电脑开机时自动运行该应用。

> ![tips.svg](/05_HOW-TO/deepin-unioncode/tips.svg)窍门：您可以在控制中心中将deepin-unioncode设置为默认的文本查看程序。
>

### 关闭deepin-unioncode

- 在主界面单击![close_icon.png](/05_HOW-TO/deepin-unioncode/close_icon.png)，退出deepin-unioncode。
- 在任务栏右键单击<img src="/05_HOW-TO/deepin-unioncode/unioncode@128.png" alt="draw" style="zoom:20%;" />，选择 **关闭所有**，退出deepin-unioncode。
- 在主界面单击![icon_menu.svg](/05_HOW-TO/deepin-unioncode/icon_menu.svg)，选择 **退出**，退出deepin-unioncode。


## 主界面

![main_interface.png](/05_HOW-TO/deepin-unioncode/main_interface.png)

| 标号 | 名称       | 描述                                                         |
| ---- | ---------- | ------------------------------------------------------------ |
| 1    | 导航栏     | 单击导航图标，快速访问最近打开界面、编辑界面、Git界面、svn界面。 |
| 2    | 菜单栏     | 通过菜单栏，您可以快速使用文件、编译、调试、分析工具、工具、编辑、帮助菜单项中的功能。 |
| 3    | 主要交互区 | 根据导航栏选择的界面，展示各界面的主要功能。                 |

### 最近打开界面

![recent.png](/05_HOW-TO/deepin-unioncode/recent.png)

| <div style="width:50pt">标号</div> | <div style="width:80pt">名称</div>         | 描述                                                         |
| ---- | ------------ | ------------------------------------------------------------ |
| 1    | 最近打开工程 | 双击工程列表可以打开工程并跳转到编辑界面。悬浮可以查看工程构建工具、编程语言、路径等信息。 |
| 2    | 最近打开文档 | 双击文件列表可以打开文件并跳转到编辑界面。悬浮可以查看文件路径信息。 |

### 编辑界面

![edit.png](/05_HOW-TO/deepin-unioncode/edit.png)

| <div style="width:50pt">标号</div> | <div style="width:50pt">名称</div>   | 描述                                                         |
| ---- | ------ | ------------------------------------------------------------ |
| 1    | 工作区 | 查看打开工程的目录树、符号树、包含文件等信息。               |
| 2    | 工具栏 | 单击图标快速使用构建、运行、调试、查找、选项等功能。         |
| 3    | 编辑器 | 展示文件内容，支持基本编辑操作。                             |
| 4    | 交互区 | 通过交互区，您可以方便地使用内置的控制台应用，使用高级查找、性能分析等功能，查看程序输出、调试输出、编译输出、智能检测输出、迁移报告、问题列表等信息。 |

### Git界面

![git.png](/05_HOW-TO/deepin-unioncode/git.png)

- OPEN按钮可以打开一个已存在的仓库。
- CLONE按钮可以在本地复制一个远程仓库。
- NEW按钮可以创建一个新的仓库。
![git_log.png](/05_HOW-TO/deepin-unioncode/git_log.png)

### SVN界面

![svn.png](/05_HOW-TO/deepin-unioncode/svn.png)

- 单击<img src="/05_HOW-TO/deepin-unioncode/burger_menu.svg" style="zoom:5%;" />可以执行打开仓库和检出仓库功能。
![svn_log.png](/05_HOW-TO/deepin-unioncode/svn_log.png)

## 基本功能

### 打开文件

1. 在菜单栏中，单击**文件**。
2. 在下拉菜单中选择 **打开文件**。
3. 在文件选择对话框中选择想要打开的文件。

![tips.svg](/05_HOW-TO/deepin-unioncode/tips.svg)窍门：您可以在文件管理器中，直接将文件拖放至编辑器区域，实现打开文件。


### 打开工程

1. 在菜单栏中，单击**文件**。
2. 在下拉菜单中选择 **打开工程**。
3. 在下拉菜单中选择打开工程的语言。
4. 选定语言后，在下拉菜单中选择构建工具。
5. 在文件选择对话框中选择想要打开的工程。

![open_project.png](/05_HOW-TO/deepin-unioncode/open_project.png)

### 新建文件


1. 在菜单栏中，单击**文件**。
2. 在下拉菜单中选择 **新建文件或工程**。
3. 在对话框中选择想要新建的文件的语言类型。
4. 输入文件名和创建路径，点击创建。

![new_file.png](/05_HOW-TO/deepin-unioncode/new_file.png)

### 新建工程

1. 在菜单栏中，单击**文件**。
2. 在下拉菜单中选择 **新建文件或工程**。
3. 在对话框中选择想要新建的工程的构建类型。
4. 输入文件名和创建路径，点击创建。

![new_project.png](/05_HOW-TO/deepin-unioncode/new_project.png)

## 编辑功能

### 文本编辑

在编辑器中，你可以进行以下基本编辑操作：

1. 输入文本。
2. 按**Bckspace**键删除文本。
3. 鼠标单击，将光标移动至鼠标点击文本处。
4. 鼠标双击，选中整个单词。
5. 使用快捷键**Ctrl + A**选择全部文本。
6. 使用快捷键**Ctrl + C**复制选中文本。
7. 使用快捷键**Ctrl + V**在光标闪烁处粘贴已经复制的文本。
8. 使用快捷键**Ctrl + X**剪切选中的文本。
9. 使用快捷键**Ctrl + Z**撤销上一步操作，包括：

   - 删除已新增的文本。
- 恢复已删除的文本。
10. 使用快捷键**Ctrl + Shift + Z**恢复之前撤销的内容。

### 查找/替换

#### 当前文件查找/替换

1. 在编辑界面，使用快捷键**Ctrl + F**或者在菜单栏**编辑**项中选择**查找/替换**打开查找工具栏。
2. 在查找工具栏的查找文本框中，输入想要查找的文本，单击按钮进行查找：
   - 向前查找：从当前选中位置向前查找匹配的文本。
   - 向后查找：从当前选中位置向后查找匹配的文本。
   - 高级查找：打开交互区的高级查找窗口。
3. 若想要替换文本，在替换文本框中，输入想要替换的文本，单击按钮进行替换：
   - 替换：将已选中的查找框中的文本替换成替换框中的文本。
   - 替换并查找：将已选中的查找框中的文本替换成替换框中的文本，并跳转到当前文件中下一个查找到的位置。
   - 替换全部：将当前文件中所有的查找框中的文本替换成替换框中的文本。

![findtoolbar.png](/05_HOW-TO/deepin-unioncode/findtoolbar.png)

#### 高级文件查找/替换

1. 在菜单栏**编辑**项中选择**高级查找**或点击查找工具栏中**高级查找**按钮打开高级查找窗口。
2. 选择查找范围：
   - 所有工程：所有已打开的工程。
   - 当前工程：当前激活的工程。
   - 当前文件：当前编辑的文件。
3. 在搜索框中输入待查找的文本，设置过滤选项：
   - 区分大小写：勾选后英文字母会区分大小写。
   - 完整匹配：勾选后文本必须匹配整个单词。
4. 输入包含的文件，输入后将仅限定包含的文件中查找，支持正则表达式。若不输入，默认所有文件。
5. 输入排除的文件，输入后查找范围将排除该文件，支持正则表达式。若不输入，默认不排除文件。

![advanced_find.png](/05_HOW-TO/deepin-unioncode/advanced_find.png)

### 文本格式化

文本格式化功能可以自动调整文本格式。

选中文本后单击右键，在菜单中选择**选中范围格式化**。
<div align=center><img src="/05_HOW-TO/deepin-unioncode/select_rightbutton.png"></div>


### 重命名

1. 选中文本后单击右键，在菜单中选择**重构**。
1. 在下拉菜单中选中重命名。

### 引用查找

1. 选中文本后单击右键，在菜单中选择**查找用法**。
2. 交互区代码信息指示器会列出该类、方法、函数、变量在工程中用到的文件的信息。
3. 双击文件即可打开文件并跳转到引用的行。

### 定义跳转

按住**Ctrl**键的同时，鼠标左键点击函数名、变量名、类名等，可以跳转到定义处。


## 编译功能

### 环境配置

1. 在菜单栏中，单击**工具**。
2. 在下拉菜单中选择 **选项**，打开选项对话框。
<div align=center><img src="/05_HOW-TO/deepin-unioncode/toolmenu.png"></div>

1. 在选项对话框中，找到需要配置的构建工具。这里以CMake为例。
2. 可以对编译器、调试器、CMake工具进行配置：
   - 编译器选项：选择代码模型使用的编译器。C编译器包括clang和gcc，C++编译器包括clang和g++。注意：不同的平台（x86/x64）应该使用不同的编译器，否则会编译报错。
   - 调试器选项：选择代码调试运行时使用的调试器。CMake工程包括lldb和gdb。
   - CMake工具：选择工程构建时使用的构建工具。
3. 点击应用保存当前配置并应用到当前已打开工程中。
![option_cmake.png](/05_HOW-TO/deepin-unioncode/option_cmake.png)

### 构建工程

- 在工程树中右键点击根目录，选择**工程属性**，打开工程属性对话框。

![projecttree.png](/05_HOW-TO/deepin-unioncode/projecttree.png)

- 在对话框中进行编译配置和环境变量配置:
  - 编译配置：包括Debug和Release两种模式。
  - 输出目录：工程编译输出文件存放的位置。
  - 工具参数：执行构建命令时自定义添加的命令参数。

![properties.png](/05_HOW-TO/deepin-unioncode/properties.png)

- 点击<img src="/05_HOW-TO/deepin-unioncode/build.png" alt="1|mian" style="zoom: 40%;" />构建按钮或使用快捷键**Ctrl+B**完成工程编译。

### 重新编译

1. 在菜单栏中，单击**编译**。
2. 在下拉菜单中选择 **重新编译**，将清空编译输出目录下经**构建工程**生成的文件，并重新启用构建套件进行编译。

### 清除

1. 在菜单栏中，单击**编译**。
2. 在下拉菜单中选择 <img src="/05_HOW-TO/deepin-unioncode/clean.svg" alt="1|mian" style="zoom: 50%;" />**清除**，将清空编译输出目录下经**构建工程**生成的文件。

## 调试功能

### 断点标记

1. 在编辑器左侧行标处，鼠标单击，出现红点标记即打上断点。
2. 鼠标单击红点即可取消断点标记。

![point.png](/05_HOW-TO/deepin-unioncode/point.png)

### 运行

在工具栏单击<img src="/05_HOW-TO/deepin-unioncode/play.png" alt="1|mian" style="zoom: 40%;" />**运行**图标，工程代码将跳过断点，直接运行。
<div align=center><img src="/05_HOW-TO/deepin-unioncode/run2.png"></div>

### 调试

1. 在需要中断运行的地方打上断点。
2. 在工具栏中点击<img src="/05_HOW-TO/deepin-unioncode/debugger_start.png" alt="1|mian" style="zoom: 40%;" />**开始调试**，程序会运行到第一个断点处，并打开变量监视器。变量监视器视图会记录变量的名称和在当前步骤时的值。

![debug.png](/05_HOW-TO/deepin-unioncode/debug.png)

调试过程中可以进行停止调试、继续调试、单步跳过、单步进入、单步跳出等操作：
<div align=center><img src="/05_HOW-TO/deepin-unioncode/debugging.png"></div>

- 停止调试：单击<img src="/05_HOW-TO/deepin-unioncode/debugger_stop.png" alt="1|mian" style="zoom: 40%;" />按钮，停止此次调试过程，程序会终止运行。
- 继续调试：单击<img src="/05_HOW-TO/deepin-unioncode/debugger_continue.png" alt="1|mian" style="zoom: 85%;" />按钮，程序将会继续执行直到下一个断点或程序结束。
- 单步跳过：单击<img src="/05_HOW-TO/deepin-unioncode/debugger_stepover.png" alt="1|mian" style="zoom: 100%;" />按钮，执行当前语句并跳过当前语句中的函数调用，直接执行下一条语句。
- 单步进入：单击<img src="/05_HOW-TO/deepin-unioncode/debugger_stepinto.png" alt="1|mian" style="zoom: 100%;" />按钮，执行当前语句并进入当前语句中的函数调用，逐步执行函数内部的语句。
- 单步跳出：单击<img src="/05_HOW-TO/deepin-unioncode/debugger_stepout.png" alt="1|mian" style="zoom: 100%;" />按钮，跳出当前函数调用，执行函数调用的下一条语句。

## 反向调试

### 记录

记录之前请确保已经打开一个C/C++工程，并且能够编译成功。

1.在菜单栏单击**工具**。

2.在下拉菜单中选择**反向调试**>**记录**。
![reverse_debug_config.png](/05_HOW-TO/deepin-unioncode/reverse_debug_config.png)

3.根据需要配置事件列表和其它参数。

4.点击**确认**启动反向调试。

5.此时应用启动运行，运行结束后弹出记录操作完成提示框。

### 回放

1.在菜单栏单击**工具**。

2.在下拉菜单中选择**反向调试**>**重放**。

3.弹出重放配置界面。
![reverse_debug_replay_config.png](/05_HOW-TO/deepin-unioncode/reverse_debug_replay_config.png)

4.点击**确定**，打开回放界面。
![reverse_debug_event_list.png](/05_HOW-TO/deepin-unioncode/reverse_debug_event_list.png)

### **调试**

1.时间轴的竖线表示一个具体事件，不同事件用不同颜色区分。

2.双击一个事件，进入到调试模式。
![reverse_debug_mode.png](/05_HOW-TO/deepin-unioncode/reverse_debug_mode.png)

3.此时进入到普通的调试模式，可以看到堆栈和变量视图。

## 代码迁移

### 迁移

使用该功能前，请确保IDE中已经激活一个C/C++工程。

1.在菜单栏单击**工具**。

2.在下拉菜单中选择**代码迁移**。

3.弹出代码迁移配置界面。
![code_porting_config.png](/05_HOW-TO/deepin-unioncode/code_porting_config.png)

选择需要迁移的工程，并设置源 CPU 架构和目标 CPU  架构。

4.点击**迁移**，开始迁移过程。
![code_porting_output.png](/05_HOW-TO/deepin-unioncode/code_porting_output.png)

### 报告

1.手动切换到**迁移报告**页面。
![code_porting_report.png](/05_HOW-TO/deepin-unioncode/code_porting_report.png)

2.双击一个条目，编辑器打开对应的源码。
![code_porting_codeline.png](/05_HOW-TO/deepin-unioncode/code_porting_codeline.png)

3.按照建议确定是否修改对应的代码。

## 常用工具

### 用户行为分析

用户行为分析针对用户编程时的命名规范做出智能提示。

1. 在工具栏单击**工具**。
2. 在下拉菜单中勾选**用户行为分析**。
3. 若要取消提示，单击**用户行为分析**取消勾选。

### 智能检测工具

#### 内存泄漏检测

1. 在工具栏单击**工具**。
2. 在下拉菜单中单击**Valgrind内存检测**，当前已激活工程会编译并运行。
3. 程序运行结束后，交互区会切换到Valgrind窗口，展示检测结果。

![valgrind.png](/05_HOW-TO/deepin-unioncode/valgrind.png)

结果查看：

- 问题列表展开会列出引发问题的函数名、文件路径、行号等信息。
- 位置列表会提供文件路径链接，单击即可打开该文件。



#### 死锁检测

1. 在工具栏单击**工具**。
2. 在下拉菜单中单击**Valgrind死锁检测**，当前已激活工程会编译并运行。
3. 程序运行结束后，交互区会切换到Valgrind窗口，展示检测结果。

注意：死锁检测结果查看同内存泄漏检测。



### 二进制工具

提供用户自主配置和使用二进制应用的视图。

1. 在工具栏单击**工具**。
2. 在下拉菜单中单击**二进制工具**，打开对话框。

![binarytool.png](/05_HOW-TO/deepin-unioncode/binarytool.png)

运行配置功能说明：

- 添加按钮：新增二进制应用。
- 删除按钮：删除当前配置页面的二进制应用。
- 重命名按钮：重命名当前配置页面的二进制应用。
- 组合按钮：组合多个二进制应用，级联执行。

参数配置说明：

- 命令标签：展示当前二进制应用的命令行执行参数。
- 工具参数：用户使用二进制应用时自定义的参数。
- 可执行文件：为当前二进制应用设定可执行文件路径。
- 工作目录：使用当前二进制应用时所在的工作路径。

环境变量说明：

- 添加按钮：新增环境变量。
- 删除按钮：删除选中项变量。
- 重置按钮：将环境变量重置为系统环境变量。

## 版本控制工具

### Git

#### 打开仓库

1. 在git界面，单击**OPEN**按钮。
2. 打开文件管理器，选择待打开的工程。

>  ![tips.svg](/05_HOW-TO/deepin-unioncode/tips.svg)窍门：Recent列表和Most used列表中选择最近打开的工程或最多使用的工程。

#### 克隆仓库

1. 在git界面，单击**CLONE**按钮。
2. 打开配置界面，输入配置参数：
   - Repository destination：代码仓库的目标存储位置。
   - URL：要克隆的远程仓库的地址。
   - Repository name：仓库名称。
   - Git user name：用户名。如果代码仓库需要身份验证，需要提供。
   - Git user email：用户邮箱。如果代码仓库需要身份验证，需要提供。
3. 勾选配置项：
   - Use as default clone directory：将Repository destination作为默认的存储地址。
   - Open repository after clone：克隆结束后打开仓库。
   - Config Git user for this repo：为 Git 仓库配置用户信息。
4. 单击**Accept**按钮进行克隆仓库。

![git_clone.png](/05_HOW-TO/deepin-unioncode/git_clone.png)

#### 新建仓库

1. 在git界面，单击**NEW**按钮。
2. 打开配置界面，输入配置参数：
   - Repository destination：代码仓库的目标存储位置。
   - Repository name：仓库名称。
   - Git user name：用户名。如果代码仓库需要身份验证，需要提供。
   - Git user email：用户邮箱。如果代码仓库需要身份验证，需要提供。
3. 勾选配置项：
   - Use as default clone directory：将Repository destination作为默认的存储地址。
   - Open repository after init：创建成功后打开仓库。
   - Config Git user for this repo：为 Git 仓库配置用户信息。
4. 单击**Accept**按钮进行新建仓库。

![git_new.png](/05_HOW-TO/deepin-unioncode/git_new.png)

### SVN

#### 检出仓库

1. 在**SVN**界面，单击<img src="/05_HOW-TO/deepin-unioncode/burger_menu.svg" alt="tips" style="zoom:5%;" />，选择**检出仓库**。
2. 打开配置界面，输入配置参数：
   - 远程仓库：要克隆的远程仓库的地址。
   - 目标路径：代码仓库的目标存储位置。
   - 用户：用户名，如果代码仓库需要身份验证，需要提供。
   - 密码：用户密码，如果代码仓库需要身份验证，需要提供。
3. 单击**确定**按钮进行克隆仓库。

![svn_clone.png](/05_HOW-TO/deepin-unioncode/svn_clone.png)

#### 打开仓库

1. 在**SVN**界面，单击<img src="/05_HOW-TO/deepin-unioncode/burger_menu.svg" alt="tips" style="zoom:5%;" />，选择**打开仓库**。
2. 在文件管理器中，选择待打开的工程。

## 帮助菜单

### 关于

1. 在菜单栏中，单击**帮助**。
2. 在下拉菜单项中，选择 **关于**。
3. 查看deepin-unioncode的版本和介绍。

### 关于插件

1. 在菜单栏中，单击**帮助**。
2. 在下拉菜单项中，选择 **关于插件...**。
3. 查看deepin-unioncode的插件管理对话框。
![plugin_manager2.png](/05_HOW-TO/deepin-unioncode/plugin_manager2.png)

插件对话框页面包含左侧的插件概览窗口，右侧的插件详情界面。单击插件概览窗口的插件项，即可在插件详情界面看到该插件的详细说明。
插件概览窗口说明：
- 名称列表：插件的二进制文件名称。
- 加载状态列表：插件是否加载，您可以通过勾选该选项管理插件的加载和卸载。


### 报告Bug

1. 在菜单栏中，单击**帮助**。
2. 在下拉菜单项中，选择 **报告Bug**。
3. 跳转到官网bug提交页面。

### 帮助文档

1. 在菜单栏中，单击**帮助**。
2. 在下拉菜单项中，选择 **帮助文档**。
3. 跳转到官网帮助文档页面。

## 流程示例

以deepin-draw（画板）工程为例，展示一个工程从打开到编译再到运行调试的全流程。
### 打开工程

1. 选择待打开的工程类型。deepin-draw的构建工具为CMake，选择cmake。

2. 在文件管理器中选择需要打开的工程，单击打开。

   注意：待打开的工程类型需要和选择的工程类型一致。
![example_open.png](/05_HOW-TO/deepin-unioncode/example_open.png)

### 编译工程

1. 单击工具栏中的编译按钮。
2. 查看交互区的编译输出框打印的信息。
![example_build.png](/05_HOW-TO/deepin-unioncode/example_build.png)

### 调试工程

1. 单击文件编辑框左侧行号处设置断点。
2. 单击工具栏中的调试按钮，程序会运行至断点处停止。
3. 交互区中的应用程序输出窗口会打印程序输出日志。
4. 编辑框右侧的变量监视视图会显示程序的变量名及在断点处的值。
![example_debug.png](/05_HOW-TO/deepin-unioncode/example_debug.png)

### 运行工程

1. 跳过所有断点后，deepin-draw工程正常运行。
2. 在deepin-draw应用中执行相关操作，可以在应用程序输出窗口中看到相应的日志输出。
![example_run.png](/05_HOW-TO/deepin-unioncode/example_run.png)

## 维护支持
- 维护者
luzhen@uniontech.com
hongjinchuan@uniontech.com

- 修改日期：2023.8.31
- 版本：1.0
- 软件仓库地址
Github：https://github.com/linuxdeepin/deepin-unioncode
Gitee：https://gitee.com/deepin-community/deepin-unioncode