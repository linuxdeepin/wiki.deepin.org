---
title: CMake编写标准
description: 
published: true
date: 2022-07-13T08:45:46.588Z
tags: 
editor: markdown
dateCreated: 2022-07-13T08:45:44.359Z
---

# CMake编写规范 dde-file-manager
## 1. CMake 函数及关键字遵循小写
```
cmake_minimum_required(VERSION 3.1)
project(dde-file-manager-lib)
```
中 cmake_minimum_required 其中 cmake_minimum_required、project


## 2. CMake 变量大写
```
set(DFM_LIB_LOG_DIR "${CMAKE_CURRENT_SOURCE_DIR}/log")
set(DFM_LIB_VIEWS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/views")
set(DFM_LIB_CONTROLLERS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/controllers")
set(DFM_LIB_MODELS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/models")
set(DFM_LIB_INTERFACES_DIR "${CMAKE_CURRENT_SOURCE_DIR}/interfaces")
set(DFM_LIB_INTERFACES_PLUGINS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/interfaces/plugins")
set(DFM_LIB_INTERFACES_CUST_DIR "${CMAKE_CURRENT_SOURCE_DIR}/interfaces/customization")
set(DFM_LIB_INTERFACES_PRIVATE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/interfaces/private")
set(DFM_LIB_INTERFACES_VFS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/interfaces/vfs")
set(DFM_LIB_INTERFACES_VFS_PRIVATE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/interfaces/vfs/private")
set(DFM_LIB_BLUETOOTH_DIR "${CMAKE_CURRENT_SOURCE_DIR}/bluetooth")
set(DFM_LIB_MATEINFO_DIR "${CMAKE_CURRENT_SOURCE_DIR}/mediainfo")
```
其中 DFM_LIB_LOG_DIR、DFM_LIB_VIEWS_DIR等变量


## 3. 在变量赋值中应当体现字符串和变量的区别
```
message("message output 3DPARTY_DOCTOTEXT_SRC " ${3DPARTY_DOCTOTEXT_SRC})
message("message output 3DPARTY_CHARSETDETECT_SRC " ${3DPARTY_CHARSETDETECT_SRC})
message("message output 3DPARTY_UNZIP_SRC " ${3DPARTY_UNZIP_SRC})
message("message output 3DPARTY_MOZILLA_SRC " ${3DPARTY_MOZILLA_SRC})
```
其中 "message output 3DPARTY_MOZILLA_SRC " 与 ${3DPARTY_MOZILLA_SRC}的区别



## 4. 多个变量或文件出现在一行应当换行
```
   add_library (${PROJECT_NAME} SHARED
   ${3DPARTY_DOCTOTEXT_SRC}
   ${3DPARTY_CHARSETDETECT_SRC}
   ${3DPARTY_UNZIP_SRC}
   ${3DPARTY_MOZILLA_SRC}
   ${3DPARTY_WV2_SRC}
   ${3DPARTY_FSEARCH_SRC}
   ${3DPARTY_TEXTSEARCH_SRC}
   ${DFM_LIB_DFM_USERSHARE_DIR}
   ${DFM_LIB_DFM_DIALOGS_DIR}
   ${DFM_LIB_DFM_CNPINYIN_DIR}
   ${DFM_LIB_DFM_UTILS_DIR}
   ${DFM_LIB_DFM_FILEOPERATIONS_DIR}
   ${DFM_LIB_LOG_DIR_SRC}
   ${DFM_LIB_VIEWS_DIR_SRC}
   ${DFM_LIB_CONTROLLERS_DIR_SRC}
   ${DFM_LIB_MODELS_DIR_SRC}
   ${DFM_LIB_SHUTILS_DIR_SRC}
   ${DFM_LIB_INTERFACES_DIR_SRC}
   ${DFM_LIB_INTERFACES_CUST_DIR_SRC}
   ${DFM_LIB_INTERFACES_PLUGINS_DIR_SRC}
   ${DFM_LIB_INTERFACES_PRIVATE_DIR_SRC}
   ${DFM_LIB_PLUGINS_DIR_SRC}
   ${DFM_LIB_GVFS_DIR_SRC}
   ${DFM_LIB_DBUSINTERFACE_DIR_SRC}
   ${DFM_LIB_QRC_FILES}
   ${DFM_LIB_IO_DIR_SRC}
   ${DFM_LIB_VAULT_DIR_SRC}
   ${DFM_LIB_TAG_DIR_SRC}
   ${DFM_LIB_VAULT_QRENCODE_DIR_SRC}
   ${DFM_LIB_IO_PRIVATE_DIR_SRC}
   ${DFM_LIB_DEVICEINFO_DIR_SRC}
   ${DFM_LIB_MATEINFO_DIR_SRC}
   ${DFM_LIB_BLUETOOTH_DIR_SRC}
   ${DFM_LIB_INTERFACES_VFS_PRIVATE_DIR_SRC}
   ${DFM_LIB_INTERFACES_VFS_DIR_SRC}
   )
```
## 5. 优先使用CMake自带宏标识路径
```
set(DFM_LIB_LOG_DIR "${CMAKE_CURRENT_SOURCE_DIR}/log")
set(DFM_LIB_VIEWS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/views")
set(DFM_LIB_CONTROLLERS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/controllers")
set(DFM_LIB_MODELS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/models")
set(DFM_LIB_INTERFACES_DIR "${CMAKE_CURRENT_SOURCE_DIR}/interfaces")
set(DFM_LIB_INTERFACES_PLUGINS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/interfaces/plugins")
set(DFM_LIB_INTERFACES_CUST_DIR "${CMAKE_CURRENT_SOURCE_DIR}/interfaces/customization")
set(DFM_LIB_INTERFACES_PRIVATE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/interfaces/private")
set(DFM_LIB_INTERFACES_VFS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/interfaces/vfs")
set(DFM_LIB_INTERFACES_VFS_PRIVATE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/interfaces/vfs/private")
set(DFM_LIB_BLUETOOTH_DIR "${CMAKE_CURRENT_SOURCE_DIR}/bluetooth")
set(DFM_LIB_MATEINFO_DIR "${CMAKE_CURRENT_SOURCE_DIR}/mediainfo")
```
其中广泛使用的 CMAKE_CURRENT_SOURCE_DIR


## 6. 引用Qt应当多次使用find_package()函数
```
find_package(Qt5 COMPONENTS Core REQUIRED)
find_package(Qt5 COMPONENTS Gui REQUIRED)
find_package(Qt5 COMPONENTS Widgets REQUIRED)
find_package(Qt5 COMPONENTS Network REQUIRED)
find_package(Qt5 COMPONENTS DBus REQUIRED)
find_package(Qt5 COMPONENTS Sql REQUIRED)
find_package(Qt5 COMPONENTS PrintSupport REQUIRED)
find_package(Qt5 COMPONENTS Svg REQUIRED)
find_package(Qt5 COMPONENTS Concurrent REQUIRED)
find_package(Qt5 COMPONENTS LinguistTools REQUIRED)
find_package(Qt5 COMPONENTS Multimedia REQUIRED)
find_package(Qt5 COMPONENTS MultimediaWidgets REQUIRED)
find_package(Qt5 COMPONENTS X11Extras REQUIRED)
find_package(Qt5 COMPONENTS Xml REQUIRED)
```
## 7. 如有相关复杂的配置和生涩的CMake语法，应当存在详尽的注释
```
   #Should You check package version，if want control the any package version. used old pkg-config
   find_package(PkgConfig REQUIRED)
   ```
   ```
   pkg_check_modules(PKGS REQUIRED
   libsecret-1
   gio-unix-2.0
   poppler-cpp
   dtkwidget
   dtkgui
   udisks2-qt5
   disomaster
   gio-qt
   libcrypto
   Qt5Xdg
   libmediainfo
   liblucene++
   liblucene++-contrib
   libxml-2.0
   htmlcxx
   libgsf-1
   glib-2.0
   dframeworkdbus
   )
```   
   
其中解释说明 #Should You check package version，if want control the any package version. used old pkg-config


## 8. 如为通用库文件与接口文件或者应当编写PackageConfig.cmake

## 9. 应当遵守CMake常用编写逻辑

首先CMake和环境配置。
其次设置生成目标。
最后设置安装路径。