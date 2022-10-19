---
title: Dock插件开发
description: 
published: true
date: 2022-10-03T16:13:31.550Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:33:23.990Z
---

## 简介

深度桌面环境中Dock除了高度可定制化的外观，同时对外提供了API文档，各位社区的开发者可以根据自己的喜好，对Dock开发插件进行扩展，让Dock更加丰富起来。

一个插件由图标、Tooltip、Popup和菜单等几部分组成。

通过以上几部分的组合和搭配来实现一个插件的完整功能；一个标准且兼容性良好Dock插件不仅需要提供用户期望的功能，同时也需要在体验上跟Dock提供的原生插件（电量、声音、网络等）相近。其推荐的交互为用户在图标上左键单击时会弹出Popup、在图标上右键单击时会弹出菜单、在图标上停留会有提示性的Tooltip显示等；

以下将说明如何开发出一个高质量的插件的过程。

## 准备工作

在开发插件之前，需要先安装一些必须的包和工具来帮助进行开发工作，在终端执行如下命令:

```bash
sudo apt-get install dde-dock-dev  build-essential qt5-qmake qt5-default qtcreator
```

其中qt5-default包是可选的，主要用作配置qt5为默认的开发环境而不是qt4，如果不太理解，建议直接安装qt5-default包即可。

## 图文介绍

1. 打开QtCreator，创建一个的“Qt Plugin”新项目，例如：名称叫“helloworld”。  

2. 创建一个类叫“HelloWorldPlugin”作为插件的入口。 

3. 创建项目后，需要对生成的项目模板进行一定的修改。

 首先，打开helloworldplugin.h和helloworldplugin.cpp更改类继承关系，从QGenericPlugin换成QObject和PluginsItemInterface，

```cpp
#include <dde-dock/constants.h>
#include <dde-dock/pluginsiteminterface.h>
class HelloWorldPlugin : public QObject, PluginsItemInterface
{
    Q_OBJECT
#if QT_VERSION >= 0x050000
    Q_INTERFACES(PluginsItemInterface)
    Q_PLUGIN_METADATA(IID "com.deepin.dock.PluginsItemInterface" FILE "helloworld.json")
#endif // QT_VERSION >= 0x050000
```

> 注意：添加了Q_INTERFACES(PluginsItemInterface)这行，并且修改了IID为"com.deepin.dock.PluginsItemInterface"。
{.is-info}

然后，删除helloworld.pro中的DESTDIR一项，它被默认指向了系统目录，不删除编译的时候会报错。

现在需要让插件实现所有PluginItemInterface的方法了，因为目前仅需要pluginName、init、itemWidget和itemTipsWidget三个方法，其他的函数都简单实现为空或者空返回：

```cpp
const QString HelloWorldPlugin::pluginName() const
{
    return "helloworld";
}
```
pluginName返回插件的名字，需要保证唯一性；

```cpp
QWidget *HelloWorldPlugin::itemWidget(const QString &itemKey)
{
    Q_UNUSED(itemKey);
    return m_icon;
}
```
itemWidget用于提供Dock插件的图标，为了保证一定的灵活性返回QWidget；其中的itemKey，是因为一个插件可以提供不限制数量的图标、Popup和菜单等组合，因此每一个组合有不同的itemKey，主要针对特别复杂的插件（例如：系统托盘插件），目前不会使用到这个字段（下同）；

```cpp
QWidget *HelloWorldPlugin::itemTipsWidget(const QString &itemKey)
{
    Q_UNUSED(itemKey);
    return m_tooltip;
}
```
itemTipsWidget用于提供Dock插件的Tooltip，一般情况下都是QLabel；这里返回一个带有“Hello World”字样的QLabel：

```cpp
HelloWorldPlugin::HelloWorldPlugin() :
    QObject(nullptr),
    m_icon(new QLabel),
    m_tooltip(new QLabel("Hello World"))
{
    QPixmap pix(":/deepin.png");
    m_icon->setPixmap(pix);
    m_icon->setMargin(6);
    m_icon->setAlignment(Qt::AlignCenter);
    m_icon->setScaledContents(true);
    
    m_tooltip->setStyleSheet("color:white;"
                             "padding:5px 10px;");
}
```

最后，init函数用于初始化Dock的插件，通过PluginProxy告知Dock需要加上图标了：

```cpp
void HelloWorldPlugin::init(PluginProxyInterface *proxyInter)
{
    proxyInter->itemAdded(this, "");
}
```

编译成功后，将生成的libhelloworld.so放到/usr/lib/dde-dock/plugins/目录，重新启动Dock即可看到新开发的插件，把鼠标置于在自定义的图标上，是不是显示出"Hello World"的Tooltip出来呢 :)


### 添加Popup

Popup提供了一种比普通的桌面程序简单而又实用的交互方式，而且插件区别于系统托盘图标，很大程度上取决于插件可以有自己的Popup。

以下说明如何添加一个Popup：

新建一个类“HelloPopup”继承自QWidget，以实现一个简单的功能为例：显示一个Label “click me！”，用户点击后会出现一个把deepin不停加长的字串。

```cpp
HelloWorldPlugin::HelloWorldPlugin() :

    QObject(nullptr),
    m_icon(new QLabel),
    m_tooltip(new QLabel("Hello World")),
    m_applet(new HelloPopup)
```

创建HelloPopup实例，并在itemTipsWidget中返回即可。

```cpp
QWidget *HelloWorldPlugin::itemTipsWidget(const QString &itemKey)
{
    Q_UNUSED(itemKey);
    return m_tooltip;
}
```

再次编译并保存libhelloworld.so文件后，重新启动Dock就可以看到不停加长的“deepin”了。

### 添加右键菜单

Dock提供了相应的API为插件提供菜单项，两个关键的函数是：itemContextMenu和invokedMenuItem

```cpp
const QString HelloWorldPlugin::itemContextMenu(const QString &itemKey)
{
    Q_UNUSED(itemKey)

    QList<QVariant> items;
    items.reserve(1);
    
    QMap<QString, QVariant> open;
    open["itemId"] = MenuIdOpenEyes;
    open["itemText"] = "Open Eyes";
    open["isActive"] = true;
    items.push_back(open);
    
    QMap<QString, QVariant> close;
    close["itemId"] = MenuIdCloseEyes;
    close["itemText"] = "Close Eyes";
    close["isActive"] = true;
    items.push_back(close);
    
    QMap<QString, QVariant> menu;
    menu["items"] = items;
    menu["checkableMenu"] = false;
    menu["singleCheck"] = false;
    
    return QJsonDocument::fromVariant(menu).toJson();
}

void HelloWorldPlugin::invokedMenuItem(const QString &itemKey, const QString &menuId, const bool checked)
{
    Q_UNUSED(itemKey);
    Q_UNUSED(checked);
    if (menuId == MenuIdOpenEyes) {
        QProcess::startDetached("xeyes");
    } else if (menuId == MenuIdCloseEyes) {
        QProcess::startDetached("killall xeyes");
    }
}
```

前者为菜单项提供内容，后者用于插件被通知菜单项的触发事件，本次给菜单添加了两个项目“Open Eyes“和”Close Eyes“，分别为打开和关闭xeyes程序。

如果你没有这个程序，请在终端执行如下命令：

```bash
sudo apt-get install x11-apps
```

仅仅实现以上两个函数并不会给你的插件注册上菜单，还需要在合适的时机（图标上的右键事件）去调用PluginProxy的requestContextMenu方法：

```cpp
bool HelloWorldPlugin::eventFilter(QObject *watched, QEvent *event)
{

    if (watched == m_icon && event->type() == QEvent::MouseButtonPress) {
        QMouseEvent *mouse = static_cast<QMouseEvent*>(event);
        if (mouse->button() == Qt::RightButton) {
            m_proxyInter->requestContextMenu(this, "");
            return true;
        }
    }
    
    return false;
}
```

> 注意：m_proxyInter需要在init函数中进行赋值，重新编译并存放在libhelloworld.so文件后，重新启动Dock，插件右键菜单就会出现，点击查看右键菜单是否正常工作。
{.is-info}

## 结束语

开发一个优秀的插件并不是那么容易，虽然简单的的插件能够正常运行，但是如何去适配Dock的两种模式以及不同大小，都需要社区开发者耐心适配来达到完美。