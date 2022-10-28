---
title: debuginfod 介绍
description: 
published: true
date: 2022-10-25T03:22:36.547Z
tags: 
editor: markdown
dateCreated: 2022-09-09T07:57:01.248Z
---

# debuginfod

软件的bug通常是不可避免的，开发者通常使用gdb，systemtap这些使用工具加载debuginfo来定位问题。

## debuginfo是什么?

一般源代码是人类可读的，机器不可读，而二进制文件是机器可读，人类不可读的。当程序遇到问题时，机器只知道运行的指令是什么，人类一般无法通过二进制指令来快速定位问题。此时就需要将此处的指令和源代码关联起来，当运行的指令出现bug时，可以快速的关联到源代码的位置上。debuginfo就是起到这个作用，是二进制指令到源代码的一个桥梁。调试信息一般就是变量名，变量类型，函数名，函数参数，函数的地址范围，行号和地址的对应关系等等，这些内容按照一定的格式写入到编译的文件中。现在最常用的格式是 DWARF(Debugging With Attributed Record Formats) 。有关`DWAERF`（https://dwarfstd.org）的介绍请见(https://dwarfstd.org/doc/Debugging%20using%20DWARF-2012.pdf)。



## debuginfod 介绍

   debuginfod 是一个用于传输调试信息资源的http文件服务器。

   debuginfod会定期扫描指定路径下的内容，比如deb和rpm打包文件等。从中提取基于`Build-id`的调试信息和可执行文件，并使用`sqlite`来保存这些信息。

   当用户使用客户端工具访问已知的`Build-id`的调试信息时，debuginfod就会返回对应的资源。支持获取`debuginfo`，`executable`和`source file`。

   文件的`Build-id`可以使用`readelf`命令来获取，比如获取`dde-dock=5.5.66-1`的`Build-id`。

   ```shell
    ❯ readelf -n /usr/bin/dde-dock | grep Build.ID
       Build ID: 3f958ed5f3924da2e3ba2e07a9e065ecbba59437
   ```

   

## debuginfod如何搭建

   debuginfod支持扫描deb，rpm和ELF/DWARF文件格式。下面是一些简单搭建使用的参数，更多参数请参考debuinfod的man手册。

   ```
   -U path/to/debs/ # 扫描的deb文件路径。
   -R path/to/rpms/ # 扫描的rpm文件路径。
   -I REGEX --include=REGEX -X REGEX --exclude=REGEX # 扫描过程中排除和包含文件的正则规则。
   -p port # 启动的端口。
   -L # 是否处理symlink，当目录中包含软连接时，需要开启这个参数。
   ```

   在deepin上，使用下面命令快速启动一个`debuginfod`服务器。默认端口为8002

   ```shell
   sudo apt insatll debuginfod
   debuginfod -U path/to/debs/
   ```

   可以通过编写service文件使用systemd 来对debuginfod的服务做管理。

   ```
   [Unit]
   Description=Debuginfod Service
   Documentation=man: debuginfod(8)
   After=network.target nginx.service
   
   [Service]
   EnvironmentFile=/etc/sysconfig/debuginfod
   ExecStart=/usr/bin/debuginfod --port=${PORT} --database=${DATABASE_FILE} -U ${REPO_PATH}
   
   [Install]
   wantedBy=multi-user.target
   ```

   

## debuginfod服务如何使用

   配置`DEBUGINFOD_URLS`环境变量指向上游的debuginfod服务器，比如在deepin上使用deepin提供的debuginfod服务器。如果长期使用，可配置到`.bashrc`文件中。

   ```shell
   export DEBUGINFOD_URLS=https://debuginfod.deepin.com
   ```

   然后使用对debuginfod支持的客户端来获取调试信息。比如`debuginfod-find`,`gdb`,`systemtap` ,`perf`等。

   deepin 20通过gdb加载debuginfod的调试信息：

   ```shell
   sudo apt install gdb
   
   export DEBUGINFOD_URLS=https://debuginfod.deepin.com
   
   gdb /usr/bin/gdb
   # 出现下面提示即从debuginfod服务器下载并加载调试信息
   Reading symbols from /usr/bin/dde-dock...
   Downloading separate debug info for /usr/bin/dde-dock...
   Reading symbols from /home/tsic/.cache/debuginfod_client/3f958ed5f3924da2e3ba2e07a9e065ecbba59437/debuginfo...
   .......
   Starting program: /usr/bin/dde-dock
   .......
   Downloading separate debug info for /lib/x86_64-linux-gnu/libdtkwidget.so.5...
   .......
   Downloading separate debug info for /lib/x86_64-linux-gnu/libsystemd.so.0...
   ```
## 注意：
1.  debuginfod对deb打包方式获取源码的支持有限，请见[can't download source code](https://wiki.debian.org/Debuginfod#The_service_isn.27t_working.21__I_can.27t_download_the_source_code_while_debugging_a_package.21)

2.  各个client对debuginfod的支持情况（2022.08）以下数据来自[debuginfod](https://sourceware.org/elfutils/Debuginfod.html)

| tool                                                         | status                                                       |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [elfutils](https://sourceware.org/elfutils/index.html)       | [released](https://sourceware.org/ml/elfutils-devel/2019-q4/msg00219.html) in version 0.178, 2019-11 |
| [systemtap](https://sourceware.org/systemtap/)               | automatic via elfutils                                       |
| [dwarves](https://github.com/acmel/dwarves)                  | automatic via elfutils                                       |
| [dwgrep](https://pmachata.github.io/dwgrep/)                 | automatic via elfutils                                       |
| [ltrace](https://gitlab.com/cespedes/ltrace)                 | automatic via elfutils                                       |
| [libabigail](https://sourceware.org/libabigail/)             | automatic via elfutils                                       |
| [binutils](https://sourceware.org/binutils/)                 | [released](https://sourceware.org/ml/binutils/2020-02/msg00006.html) in version 2.34, 2020-02 |
| [gdb](https://sourceware.org/gdb/)                           | [released](https://lists.gnu.org/archive/html/info-gnu/2020-10/msg00009.html) in version 10.1, 2020-10 |
| [dyninst](https://www.dyninst.org/dyninst)                   | [released](https://github.com/dyninst/dyninst/pull/736) in version 11.0 2021-04 |
| [valgrind](https://www.valgrind.org/)                        | [released](https://sourceware.org/git/?p=valgrind.git;a=commit;h=fd4e3fb0ffa3f751f0e7f2e7f4639d4c3773305d) in version 3.17.0, 2021-03 |
| [annocheck](https://fedoraproject.org/wiki/Toolchain/Watermark) | [released](https://sourceware.org/git/?p=annobin.git;a=commitdiff;h=3d27b86eb088bd23c5908a9d78c733417ade79be) in version 9.03, 2020-01 |
| [delve](https://github.com/go-delve/delve)                   | [released](https://github.com/go-delve/delve/pull/2670) in version 1.7.2, 2021-09 |
| [llvm](https://llvm.org/)                                    | symbolizer [merged](https://reviews.llvm.org/D113717), server [merged](https://reviews.llvm.org/D114846), lldb [help wanted](https://reviews.llvm.org/D75750), [see also](https://github.com/schultetwin1/gdbundle-debuginfod) |
| [bpftrace](https://github.com/iovisor/bpftrace)              | [released](https://github.com/iovisor/bpftrace/issues/1774) in version 0.21.0, 2021-07 |
| [perf](https://perf.wiki.kernel.org/index.php/Main_Page)     | released in linux 5.10, 2021-01                              |
| [systemd-coredumpd](https://www.freedesktop.org/software/systemd/man/systemd-coredump.html) | [help wanted](https://github.com/systemd/systemd/issues/14711) |
| [retrace/abrt/faf](https://github.com/abrt/faf/wiki/Retracing) | [in](https://github.com/abrt/faf/issues/952) [progress](https://github.com/abrt/faf/issues/951) `amerey@redhat.com` |
| [vtune](https://software.intel.com/content/www/us/en/develop/tools/oneapi/components/vtune-profiler.html) | [help wanted](https://community.intel.com/t5/Intel-oneAPI-Base-Toolkit/RFE-debuginfo-access-linux-via-debuginfod-protocol/m-p/1252914#M1032) |
| [pixie](https://pixielabs.ai/)                               | [help wanted](https://github.com/pixie-labs/pixie/issues/262) |
| [sentry symbolicator](https://sentry.io/)                    | partly [released](https://github.com/getsentry/symbolicator/pull/142) partly [help wanted](https://github.com/getsentry/symbolicator/issues/445) |
| [VS Code](https://code.visualstudio.com/)                    | [partly automatic via gdb](https://github.com/microsoft/MIEngine/issues/1142) |
| [WinDbg](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-download-tools) | [released](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/source-code-extended-access) in version 1.2104.13002.0, 2021-04 |
| [UDB](https://undo.io/)                                      | [released](https://docs.undo.io/) in version 6.5             |
