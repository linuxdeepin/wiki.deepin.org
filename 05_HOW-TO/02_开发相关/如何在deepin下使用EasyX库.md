---
title: 如何在deepin下使用EasyX库
description: EasyX库是一个C++的简单图形库
published: true
date: 2023-03-09T02:47:00.044Z
tags: 
editor: markdown
dateCreated: 2023-03-09T02:38:47.384Z
---

# GUN/Linux 下使用 EasyX

## 一、EasyX 与 CLion 简介


### （一）、EasyX

EasyX，全名：“EasyX Graphics Library for C++”。由于其采用静态编译，并不依赖任何 dll，超低的学习成本，深受许多开发者喜欢，在下面列举一些 EasyX 的特点：

-   EasyX 是针对 C++ 的图形库，可以帮助 C/C++ 开发者快速上手图形设计。
-   由于其简单的操作，在进行一些 C/C++、图形学等课程设计实验时，可以专注在课程知识上，不被绘图部分牵扯太多精力。
-   EasyX 能够在 Visual Studio 上完美使用，为很多开发者节约适配时间。
-   EasyX 在 C/C++ 学习、编写小游戏、图形学、粒子系统、物理模拟等多种场景下均有运用。「[EasyX 作品](https://codebus.cn)」

> EasyX 具有如此多的优点，但是在早期仅支持 MVSC 编译器。对于使用 GNU/Linux 发行版的开发的开发者来说造成很多的困扰。终于，在 2022 年 6 月 10 号，EasyX 提供适配 MinGW 的 [库文件](https://easyx.cn/download/easyx4mingw_20220610.zip)，从这开始 EasyX 正式支持使用 minGW 编译器，这就为使用 GNU/Linux 开发的开发者提供切入点！

### （二）、CLion

CLion 是由 JetBrains 出品的专门为开发 C 以及 C++ 所设计的跨平台集成开发环境（IDE）。其以 IntelliJ 为基础设计，包含许多智能功能来提高开发者的生产力。同时其强大的跨平台能力使之 CLion 能够在 GNU/Linux、OS X 和 Windows 上开发 C 和 C++。

> 注意：在这里笔者仅仅以 CLion 为 IDE 开发环境开发 EasyX，但同时在 VScode 上也能成功开发 EasyX。其根本是 EasyX 支持使用 MinGW 编译器，与所使用的 C/C++ 开发工具无关。

那么接下来就开始学习如何通过 MinGW 与 CLion 使用 EasyX 吧！

---

## 二、GNU/Linux 下的 MinGW

### （一）、安装 MinGW

假如所使用的 GNU/Linux 发行版为 Debian 系，及 Debian、Ubuntu 等，则可直接使用以下命令安装 MinGW 编译器。

```bash
sudo apt-get update
sudo apt-get install mingw-w64
```

若您使用的是其他 GNU/Linux 发行版，则可以参考 MinGW 官网「[MinGW](https://www.mingw-w64.org/downloads/)」

### （二）、使用 MinGW-W64 编译器

MinGW，即 “Minimalist GNU for Windows”。译为适用于 Windows 的极简主义 GNU。

它是一个可自由使用和自由发布的 Windows 特定头文件和使用 GNU 工具集导入库的集合，允许在 GNU/Linux 和 Windows 平台生成本地的 Windows 程序而不需要第三方 C 运行时（C Runtime）库。[^1]

简单来说：它实际上是将经典的开源 C 语言编译器 GCC 通过添加 Windows 独有的头文件、Win32 API 等其他集合，从而移植到 Windows 上。使之在 Windows 上也可以使用 GCC。

在这里，我们使用 MinGW-w64 编译器，区别于过去的只能编译 32 位可执行文件的 MinGW。MinGW-w64 既可编译 32 位可执行文件也可编译 64 位可执行文件。[^2] 现如今使用的 MinGW 已经被 MinGW-w64 所取代, 所以在下面笔者所述的 MinGW 皆为 MinGW-w64。

MinGW 特点

-   MinGW-w64 位开源软件 [^3]。
-   MinGW-w64 拥有一个活跃的开源社区。
-   支持 C、C++、Ada, Obj-C, Obj-C++, OCaml 在内的一系列语言。
-   MinGW-w64 直接使用 Windows 的 C 语言运行库，编译下的可执行程序可以直接在 Windows 下运行。
-   许多 IDE 都支持 MinGW-w64，比如 Dev-Cpp、CLion、Code::Blocks、CFree 等。

那么在上一步我们已经成功安装了 MinGW，该如何去使用它呢？

#### 1、CLion 构建工作链

> CLion 工具链简介：
> 对于 CLion 中的 CMake 项目，工具链是构建和运行应用程序所需的所有必要工具的集合：工作环境，CMake 可执行文件，make 和编译器以及调试器。
> 您始终可以为一个项目拥有多个工具链，并在需要时在它们之间轻松切换。
> 当您开始使用 CLion 时，您已经有了默认的工具链。尽管可以在开发中使用它，但您可能还需要根据项目需要调整工具集（例如，更改工作环境或切换到其他编译器）。[^4]

首先，我们需要去 `/usr/bin` 目录下寻找一下两个程序：

-   x86_64-w64-mingw32-gcc
-   x86_64-w64-mingw32-g++

如果成功找到，那么接下来笔者将通过在 CLion 上设置 MinGW 工具链的方式在 CLion 上使用 MinGW。

如图：

![img](https://s3.bmp.ovh/imgs/2023/03/06/e03d67d75769249e.png)

<center> 图 1</center>

> 解释：
> 1、首先点击 “+” 创建新的工具链
> 2、将工具链名设置为 “mingw”
> 3、设置 C/C++ 编译器，如图所示。
> 注意：
> 在 CLion 中已经自带有构建工具 ninja、CMake 和调试器 GDB。如果读者在其他开发环境使用，如 nvim、VScode 等，需要自行安装。在接下来的使用中会用到上文程序。

#### 2、CMake 构建编译环境

> CMake 简介：
> CMake 是一个跨平台的安装（编译）工具，可以用简单的语句来描述所有平台的安装 (编译过程)。
> 他能够输出各种各样的 makefile 或者 project 文件，能测试编译器所支持的 C++ 特性, 类似 UNIX 下的 automake。只是 CMake 的组态档取名为 CMakeLists.txt。Cmake 并不直接建构出最终的软件，而是产生标准的建构档（如 Unix 的 Makefile 或 Windows Visual C++ 的 projects/workspaces），然后再依一般的建构方式使用。
> 这使得熟悉某个集成开发环境（IDE）的开发者可以用标准的方式建构他的软件，这种可以使用各平台的原生建构系统的能力是 CMake 和 SCons 等其他类似系统的区别之处。[^5]

![img](https://s3.bmp.ovh/imgs/2023/03/06/4302e44b2eee429c.png)

<center> 图 2</center>

> 解释：
> 1、首先如上文步骤一样，点击 “+” 创建新的 CMake
> 2、修改 CMake 文件名
> 3、修改工具链，在 GNU/Linux 下默认使用 gcc，这里使用在上文构建的工具链。

至此，我们已经在 CLion 上做好的 MinGWd 的基础环境配置。那么开始码代码吧:)

---

## 三、CMake 构建开发环境

> 注意：在接下来所介绍的 CMake 使用皆为依据笔者所使用的 CMakeList 文件内容，简单介绍 CMake 命令。若想要深入理解 CMake，可以阅读「[CMake Reference Documentation](https://cmake.org/cmake/help/latest/)」，望见谅。

### （一）、理解 CMake 基本工作原理

首先，我们在 CLion 新建项目，其基本结构如下：

<div align=center><img src="https://s3.bmp.ovh/imgs/2023/03/06/76eb5d330bb99538.png"></div>
<center> 图 3</center>

> 解释：
> 第一项：在 CLion 官方文档下描述为 “在这里指定生成的 CMake 文件所需的位置”。在原生的 CMake 编译下通过 CMakeLists.txt 文件生成 CMakeFile 等一系列文件，CLion 则直接将其汇合为一个文件夹 “Build directory”。注意：这里仅代表 CLion，若在 VScode 下使用则还需更加深入了解 CMake 工作原理。
> 第二项：为 CMake 配置文件

了解了，基本项目架构，那么我们便开始简单解释 CMake 工作原理吧！:-D

首先，我们打开 `CMakeLists.txt` 文件，其原生内容（这里指 CLion 初始化内容）如下：

```cmake
cmake_minimum_required(VERSION 3.24)
project(Test)

set(CMAKE_CXX_STANDARD 17)

add_executable(Test main.cpp)
```

> 解释：
> 第一项参数：`cmake_minimum_required`，原意为 “cmake 最低要求”。指在您所构建的环境下，cmake 所使用版本必须大于或等于您所设置的 cmake 版本。
> 第二项参数：`project` 指的是该项目名，注意该项参数很重要。后续许多命令都会使用该参数。
> 第三项参数：`set` 作用是用来给变量设置值。变量一般分为三种：
>
> > -   一般变量
> > -   缓存变量
> > -   环境变量
> >
> > 在这里我们通过 `set()` 命令设置 C++ 标准为 C++17
> > 第四项参数：`add_executable` 原意为 “添加可执行文件”，作用为：使用指定的源文件来生成目标可执行文件。这里 CLion 自动生成 main.cpp 文件，并设置运行 / 调试程序名为 Test（即是前面说设置的 project 项目名）

其次，我们再来看看 `main.cpp` 文件：

```cpp
#include <iostream>

int main() {
  std::cout << "Hello, World!" << std::endl;
  return 0;
}
```

这仅一个生成 “Hello,World!” 的简单代码。
我们关注其生成原理：

在 CLion 中，我们只需点击 “绿色三角形按键” 即可。
![img](https://s3.bmp.ovh/imgs/2023/03/06/93250e3b1973656a.png)

<center> 图 4</center>

若使用命令行方式，这执行：

```bash
cmake .
make
./Test
```

具体过程：

```bash
$ ls
CMakeLists.txt  main.cpp
$ cmake .
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: ~/Downloads
$ make
[50%] Building CXX object CMakeFiles/Test.dir/main.cpp.o
[100%] Linking CXX executable Test
[100%] Built target Test
$ ls
CMakeCache.txt  CMakeFiles  cmake_install.cmake  CMakeLists.txt  main.cpp  Makefile  Test
$ ./Test
Hello, World!
```

即可得到 “Hello World!” O(∩_∩)O 哈哈~

OK!，我们已经成功通过 cmake 编译生成 C++ 可执行文件了，那么我们开始进阶操作吧！↖(^ ω ^)↗

### （二）、通过 CLion 与 CMakeLists 创建项目开发环境

我们分别在 `Test` 项目目录下创建以下文件夹：

-   bin
-   include
-   lib
-   src

> 解释：
> bin：bin 是 Binaries (二进制文件) 的缩写，用于存放 cmake 编译源代码生成的可执行文件。
> include：用于存放 C++ 头文件
> lib：lib 是 library（库文件）的缩写，用于存放库文件，比如我们之后需要使用的 “libeasyx.a” 静态链接库
> src：src 是 source（源文件） 的缩写，用于存放源文件。
> 注意：不设置这些文件，依旧可以使用 cmake 编译源文件，但是为了项目的完整性和可持续发展，笔者建议读者尽量遵守项目开发基本规则。

创建结果如下：

<div align=center><img src="https://s3.bmp.ovh/imgs/2023/03/06/6d84e1acc3bedf03.png"></div>
<center> 图5</center>
准备好对应文件夹，我们终于可以开始接触 EasyX 了，是不是感觉经历九九八十一难？但请注意，这最后一难往往最为重要！接下来，我们便开始出发吧。芜湖~~

---

## 四、安装 EasyX 库

首先，我们需要安装的是「[easyx-for-mingw](https://codebus.cn/bestans/easyx-for-mingw)」。目前，截止到现在（2023.2.13）最新的版本为 20220901。[点击这里下载 easyx4mingw_20220901](https://easyx.cn/download/easyx4mingw_20220901.zip)

下载完成，我们可以看到 easyx 文件夹内具体内容为：

```bash
├── include
│   ├── easyx.h             // 头文件（提供当前最新版本接口）
│   └── graphics.h          // 头文件（在 easyx.h 基础上，保留若干旧接口）
├── lib32
│   └── libeasyx.a          // 针对 TDM-GCC 4.8.1 及以上版本的 32 位库文件
├── lib64
│   └── libeasyx.a          // 针对 TDM-GCC 4.8.1 及以上版本的 64 位库文件
├── lib-for-devcpp_5.4.0
│   └── libeasyx.a          // 适用于 DevCpp 5.4.0 GCC MinGW 4.7.2 和 C-Free 5.0
└── readme.txt
```

将 `easyx.h` 和 `graphics.h` 分别拷贝到我们之前新建项目的 `include` 目录，如果您想要使用 64 位的库文件，则将 `libeasy.a` 文件拷贝到 `lib` 目录，同理，使用 32 位库文件一样。

至此，EasyX 已经安装完成，下面便是最重要的 CMakeLists 配置！

---

## 五、CMakeLists 配置

> 注意：
> 在接下来，笔者仅以自己的配置为例，如果读者想要实现其他功能，则参考「[CMake Reference Documentation](https://cmake.org/cmake/help/latest/)」。

配置如下：

```cmake
cmake_minimum_required(VERSION 3.24)
project(Test)

set(CMAKE_CXX_STANDARD 17)

set(CMAKE_EXE_LINKER_FLAGS "-static-libgcc -static-libstdc++")

SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

link_directories(${PROJECT_SOURCE_DIR}/lib)

set(SOURCES
       src/main.cpp
        )

#------------------------------------
add_executable(Test ${SOURCES})

target_link_libraries(Test easyx)

target_include_directories(Test
        PRIVATE
        ${PROJECT_SOURCE_DIR}/include
        )
```

> 解释：
> 前三项我们已经解释了，那么从第四项开始吧
> `set()` 命令的用法我们已经介绍过了，简单来说就是为变量赋值。
> i. 命令 `set(CMAKE_EXE_LINKER_FLAGS "-static-libgcc -static-libstdc++")` 用意就是让 cmake 采用静态编译的方式，否则单纯运行 exe 文件并不能运行。有关静态编译命令的简介可看「[cmake 静态编译 简介](https://blog.csdn.net/whatday/article/details/118071243)」
> ii. 命令 `SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)` 作用：设置 `bin` 目录为可执行文件存放目录，其中 `${PROJECT_SOURCE_DIR}` 表示项目目录，即上文设置的 `Test` 项目目录。
> iii. 命令 `link_directories(${PROJECT_SOURCE_DIR}/lib)` 作用为为 cmake 提供存放库文件存放目录的地址，即上文设置的 `lib` 目录。
> iv. 命令：
>
> > ```cmake
> > set(SOURCES
> >     src/main.cpp
> >      )
> > ```
> >
> > 表示将 src 下所有文件共用一个名字 `SOURCES`，这样后面我们如果再想要添加新的源文件，可以直接接着 `src/main.cpp` 写，比如：笔者希望添加 `test.cpp` 源文件，这可以这么写：
> >
> > ```cmake
> > set(SOURCES
> >     src/main.cpp
> >     src/test.cpp
> >      )
> > ```
> >
> > 但是，请注意：一个 `SOURCES` 下只能存在一个 `main` 程序。
> >
> > 接着往下看：
> > v. 命令 `add_executable(Test ${SOURCES})`，作用在上文我们说过，这里我们直接将 `src` 目录下所有源程序都使用一个运行 / 调试程序 `Test` 来运行，提高开发效率。
> > vi. 命令 `target_link_libraries(Test easyx)`，`target_link_libraries` 命令用来链接导入库，即按照 header file + .a + .so 方式隐式调用动态库的. a 库。详细介绍可以查看官方文档 「[target_link_libraries](https://cmake.org/cmake/help/latest/command/target_link_libraries.html#command:target_link_libraries)」
> > 注意：
> > `target_link_libraries` 必须用在 `add_executable` 之后。
> > 使用该命令对应的库文件名是相对随意的，cmake 会自动去寻找您存放在项目下的 lib 文件。
> > 比如：
> >
> > ```cmake
> > # 链接 libeasyX.so 库
> > # 以下写法都可以：
> > target_link_libraries(Test easyx)
> > target_link_libraries(Test libeasyx.a)  # 显示指定链接静态库
> > target_link_libraries(Test libeasyx.so) # 显示指定链接动态库，这里只是作比较，其实并没有 libeasyx.so 动态库
> >
> > # 再比如：
> > target_link_libraries(Test libeasyx.so)
> > target_link_libraries(Test easyx)
> > target_link_libraries(Test -leasyx)
> > ```
> >
> > vii. 命令：
> >
> > ```cmake
> > target_include_directories(Test
> >      PRIVATE
> >       ${PROJECT_SOURCE_DIR}/include
> >       )
> > ```
> >
> > 为 cmake 提供头文件存放位置，该命令类似于上面的 `target_link_libraries`。其中用到了关键词 `PRIVATE`，除此之外还有 `INTERFACE`、`PUBLIC`。

OK!, 很棒 \\(^ o ^)/YES!
我们总算可以开始使用 EasyX 库了！
开始吧！

---

## 六、使用 EasyX 库

首先，我们还是从代码出发吧！

我们修改 `main.cpp` 文件中的内容为：

```cpp
#include<graphics.h>
#include<conio.h>

int main()
{

    initgraph(666, 666);  // 初始化为 666*666 的画布
    /*    circle    */
    setcolor(BLUE);   //circle 的线条为蓝色
    setfillcolor(RED);  //circle 内红色填充
    setbkcolor(GREEN);
    fillcircle(100, 100, 20); //circle center 为（100，100）半径 20


    getch();     // 按任意键继续
    closegraph();    // 关闭图形界面
    return 0;
}
```

> 解释：
> 这里笔者仅仅只是通过绘画出一个圆的方式，来简单使用 EasyX 库。

如果在 CLion 中出现：

![img](https://s3.bmp.ovh/imgs/2023/03/06/d807d0df5958c1a4.png)

<center> 图 6</center>
则证明，cmake 已经成功编译，那么我们便可以在 `bin` 目录中找到生成的 `Test.exe` 文件。
如图：
<div align=center><img src="https://s3.bmp.ovh/imgs/2023/03/06/affa2e9f7831f56a.png"></div>
<center> 图 7</center>

我们可以通过 `wine` 程序运行该 `exe` 文件。

运行结果如下：

<div align=center><img src="https://s3.bmp.ovh/imgs/2023/03/06/af7efb4922abc584.png"></div>
<center> 图 8</center>

> 解释：
> 笔者使用的操作系统为 ==Deepin==，所以笔者使用的 `wine` 为 `deepin-wine6-stable`，当然 Debian 系也可以直接通过 `sudo apt update && sudo apt install wine` 安装 `wine`。至于其他操作系统笔者并未接触，望谅解。
> 可以通过查看「[WineHQ](https://wiki.winehq.org/Download)」，获得最新的 `wine`。

[^1]: [MinGW 详细介绍](https://blog.csdn.net/qq_38880380/article/details/78441943)
[^2]: [各种编译器的介绍](https://www.cnblogs.com/AlexSun-2021/p/16026917.html)
[^3]: [MinGW-w64](https://github.com/mingw-w64)
[^4]: [CLion Documentation](https://www.jetbrains.com/help/clion/how-to-create-toolchain-in-clion.html#cmake-toolchain)
[^5]: [cmake](https://baike.baidu.com/item/cmake/7138032)

感谢社区开发者[AaronLi](https://github.com/Free-Aaron-Li)的贡献
