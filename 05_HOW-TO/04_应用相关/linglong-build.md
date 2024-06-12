---
title: 从源代码开始构建玲珑格式应用
description: 如何制作玲珑格式应用安装包
published: true
date: 2024-06-12T02:58:58.278Z
tags: 玲珑, 玲珑包, 玲珑格式, 制作玲珑, 打包玲珑
editor: markdown
dateCreated: 2024-06-12T02:58:58.278Z
---

玲珑入门教程：从源代码开始构建玲珑格式应用
- 请首先阅读玲珑官方文档 [ll-builder简介 | 玲珑](https://linglong.dev/guide/ll-builder/introduction.html)
- 本文以构建 [desktop-entry-editor](https://github.com/zty199/desktop-entry-editor) 为例，该项目依赖较为简单，仅需玲珑官方文档中默认提供的基础运行环境即可成功构建运行

# 第一步：前期准备
在终端中执行
`sudo apt install linglong-builder --no-install-recommends`

安装 `ll-builder` 工具

![image](https://storage.deepin.org/thread/20240605121436813_1.png)

由于推荐安装依赖较多，此处跳过推荐依赖不进行安装。如有需要，去除命令中 `--no-install-recommends` 参数即可

# 第二步：创建项目
由于需要从源代码构建玲珑格式应用，故可以跳过 `ll-builder create` 操作，无需创建玲珑对应文件夹，直接在工程源代码顶层目录编写 `linglong.yaml `文件即可（可使用官网提供的完整的 [模板文件](https://linglong.dev/guide/ll-builder/create.html#%E5%AE%8C%E6%95%B4%E7%9A%84linglong-yaml%E9%85%8D%E7%BD%AE) ）。

# 第三步：编辑 `linglong.yaml`
`version: "1"                            # linglong.yaml 文件语法的版本

package:                                # 软件包元信息配置
id: com.github.desktop-entry-editor     # 软件包 appid，类似 deb 格式软件包包名，区分不同玲珑格式软件包
name: desktop-entry-editor              # 应用名称
version: 1.4.6.1                        # 软件包版本
kind: app                               # 软件包类型，多数应用为 app，基础环境和运行时为 runtime
description: |                          # 软件包描述
   desktop entry editor for deepin os.

command:                                # 容器内可执行程序启动命令
 - /opt/apps/com.github.desktop-entry-editor/files/bin/desktop-entey-editor

base: org.deepin.foundation/23.0.0      # 基础环境
runtime: org.deepin.Runtime/23.0.1      # 运行时

source:                                 # 构建来源
 kind: local                           # 由于直接在工程源代码中构建，此处选择 local 即可，无需从 git 仓库拉取代码

build: |                                # 构建
 rm -rf build-linglong
 mkdir -p build-linglong

 qmake BUILD_VERSION=1.4.6 \
       PREFIX=${PREFIX} \
       LIB_INSTALL_DIR=${PREFIX}/lib/${TRIPLET} \
       INSTALL_ROOT=${PREFIX} \
       -spec linux-g++ CONFIG+=qtquickcompiler \
       -o build-linglong \
       desktop-entry-editor.pro

 make -C build-linglong -j$(nproc)
 make -C build-linglong -j$(nproc) install`


- 以此 [linglong.yaml](https://github.com/zty199/desktop-entry-editor/blob/main/linglong.yaml) 为例，软件包元信息按需填写即可。
- 由于项目较为简单，仅需要基础的 Qt 和 Dtk 环境即可构建，使用模板文件中所示的 base 和 runtime 即可。更复杂的构建环境可参考构建 [计算器](https://linglong.dev/guide/ll-builder/manifests.html#%E8%AE%A1%E7%AE%97%E5%99%A8)，在构建应用前拉取所需依赖代码，优先构建所需依赖。
## 理解安装位置“前缀”
以我们所熟知的方式来理解，一般可执行文件需要放到 `/usr/bin` 文件夹下，在终端尝试执行时才能被找到；`.desktop` 文件提供了启动的入口，想在启动器中看到应用图标，一般放到 `/usr/share/applications` 文件夹下；而图片文件会放到 `/usr/share/icons` 文件夹中具体的图标主题及尺寸和分类文件夹下。这其中，`/usr` 就是所有文件安装位置的 前缀。

根据 [GNU 编码标准](https://www.gnu.org/prep/standards/html_node/Directory-Variables.html)，前缀 的默认值一般为 `/usr/local`；而构建 deb 格式软件包时，一般会使用 `/usr` 前缀。

而玲珑容器启动时，会将容器内容 files 文件夹 挂载至 `/opt/apps/${appid}` 文件夹下，故可以近似认为 前缀 为 `/opt/apps/${appid}/files`。以此类推，可执行文件的实际位置为 `/opt/apps/${appid}/files/bin/`可执行文件名称，所以 `linglong.yaml` 中 `command` 启动指令部分也需要如此填写，而不是常见的 `/usr/bin/`可执行文件名称。

## 检查修改工程源代码
理解了 前缀 的概念，就要检查工程源代码的安装位置前缀了。qmake 工程中，默认安装位置前缀一般为 /opt/$${TARGET} 文件夹，如可执行文件就被放在 /opt/$${TARGET}/bin 文件夹中。此处可能需要修改 .pro 文件，将前缀修改为 /opt/apps/${appid}/files 文件夹，保证玲珑容器启动后文件位置正确。

或者如构建 [计算器](https://linglong.dev/guide/ll-builder/manifests.html#%E8%AE%A1%E7%AE%97%E5%99%A8) 所示，通过 `qmake` 参数从外部传入安装位置前缀，并在 `.pro` 文件中解析 `${PREFIX}` 以实现控制文件安装位置。

# 第四步：构建应用
在工程源代码顶层目录（`linglong.yaml` 同级目录）打开终端，执行 `ll-builder build` 命令即可。

![image](https://storage.deepin.org/thread/20240605121452715_2.png)

![image](https://storage.deepin.org/thread/202406051215034759_3.png)


首次构建时需下载指定的 `base` 和 `runtime`，耗时较长，需要耐心等待。

# 第五步：测试运行应用
在工程源代码顶层目录（`linglong.yaml` 同级目录）打开终端，执行 `ll-builder run --exec` 可执行程序名称，可测试在玲珑容器环境内启动应用。

![image](https://storage.deepin.org/thread/202406051215131349_4.png)

若找不到可执行程序，请检查工程源代码中文件安装位置是否正确。可查看工程顶层目录下 `linglong/output/runtime/files/bin` 文件夹中是否生成可执行文件。

若启动应用失败，参考 文档 在容器内进行调试。

# 第六步：导出 layer 文件
在工程源代码顶层目录（`linglong.yaml` 同级目录）打开终端，执行 `ll-builder export` 命令，即可在目录中生成 `${appid}_${version}_${arch}_develop.layer` 和 `${appid}_${version}_${arch}runtime.layer` 文件。

![image](https://storage.deepin.org/thread/202406051215218081_5.png)

# 第七步：测试安装 layer 文件
在工程源代码顶层目录（`linglong.yaml` 同级目录）打开终端，输入 `ll-cli install` 并输入空格分隔后，将文件夹中的 `runtime.layer` 文件拖入终端，按回车执行，将 `layer` 文件安装至本地玲珑环境中。

![image](https://storage.deepin.org/thread/202406051215271077_6.png)

安装成功后，应该可以在启动器中看到该应用，并进行启动和使用测试。

![image](https://storage.deepin.org/thread/202406051215317887_7.png)

若启动器中无法找到该应用，请检查工程源代码中文件安装位置是否正确。可查看工程顶层目录下 `linglong/output/runtime/entries/share/applications` 文件夹中是否生成 `.desktop` 文件。

若存在启动失败情况，可查看对应 `.desktop` 文件并复制 `Exec` 字段内容至终端中执行，观察启动输出日志。若提示缺少玲珑对应版本的 `base` 或 `runtime` 环境，需手动执行 `ll-cli install` 环境名称/版本号 进行安装以排除故障（e.g. `ll-cli install org.deepin.Runtime/23.0.1`）。

# 总结
至此从源代码构建玲珑格式应用已完成。

本文举例的项目使用 `qmake` 构建，依赖简单，且支持构建时外部传入 `${PREFIX}` 设置文件安装位置，构建为玲珑格式应用非常简单。若项目使用 `cmake` 或其他工具构建，同理需在工程内修改安装前缀，或在 `linglong.yaml` 构建部分传入参数进行设置，否则即使构建成功，也会出现容器内无法找到可执行文件，或安装后启动器没有图标等各种问题。

---
作者：忘记、过去
原帖：[玲珑入门教程：从源代码开始构建玲珑格式应用](https://bbs.deepin.org/post/273497)