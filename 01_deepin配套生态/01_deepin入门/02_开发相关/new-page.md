---
title: DTK（developer toolkit）开发者工具套件
description: 
published: true
date: 2022-10-25T07:15:59.704Z
tags: 
editor: markdown
dateCreated: 2022-10-25T07:15:59.704Z
---


# DTK（developer toolkit）开发者工具套件

## 3.1.dtkdeclarative的基础和创新
dtkdeclarative 开发控件库用于构建原生的行云设计风格应用程序。dtkdeclarative 基于 Qt Quick 和 Qt Qml 基础框架全新开发原有 dtkwidget 模块，代码设计借鉴 qtdeclarative 并实现 Qt Quick Controls 2 全控件覆盖，特别为 DTK （developer toolkit，开发者工具套件）拓展颜色风格、视觉特效等方面的特性，比如调色板灵活定制、模糊算法性能调优等。dtkdeclarative 模块架构如下：
## 
3.2.dtkdeclarative 和 dtkwidget 的不同
如果要对比 dtkdeclarative 和 dtkwidget 的不同，不妨先了解下 Qt Quick 和 Qt Widgets 的区别，毕竟，dtkdeclarative 基于 Qt Quick ，dtkwidget 基于 Qt Widgets 。
在开发语言方面，Qt Widgets 使用 C++ ，Qt Quick 使用 QML ；在用户界面渲染方面，Qt Widgets 更适合桌面应用程序开发，基本满足用户界面绘制和动画效果渲染，同时能够兼容性能更低的硬件平台；Qt Quick 不仅可以用于桌面应用程序开发，面向移动端开发更具触控交互、动效体验方面的优势，同理这些优势能够在桌面端适配实现，但是需要更多的本地硬件性能。
dtkdeclarative 和 dtkwidget 同样如此，但这并不是证明二者孰优孰劣，毕竟应用程序开发应以需求为导向，选择合适的开发框架才是最好的。为此，不妨通过编码方面对比，初步学习下 dtkdeclarative 吧。
### 
3.2.1.dtkwidget 和 dtkdeclarative 在渲染方式上的区别
示例：实现显示一个100x100的红色矩形
class Rectangle : public QWidget
{
private:
    void paintEvent(QPaintEvent *) override {
        QPainter pa(this);
        pa.fillRect(0, 0, 100, 100, Qt::red);
    }
};
Rectangle {
    width: 100
    height: 100
    color: "red"
}

### 3.2.2.dtkwidget 和 dtkdeclarative 在布局方式上的区别
示例：实现一个对话框布局
int main(int argc, char *argv[]) {
    QApplication app(argc, argv);
    
    QWidget mainWidget;
    mainWidget.resize(200, 200);
    QHBoxLayout layout1(&mainWidget);
    QPushButton btn1("Button 1");
    layout1.addWidget(&btn1);
    QVBoxLayout layout2;
    QPushButton btn2("Button 2");
    QLineEdit lineEdit;
    layout2.addWidget(&btn2);
    layout2.addWidget(&lineEdit);
    layout1.addLayout(&layout2);
    mainWidget.show();
    
    return app.exec();
}
// 使用锚布局
Rectangle {
    width: 400
    height: 200
    color: "blue"








    Button {
        anchors.left: parent.left
        anchors.verticalCenter: parent.verticalCenter
        text: "Button 1"
    }








    Button {
        anchors.right: parent.right
        anchors.verticalCenter: parent.verticalCenter
        text: "Button 2"
    }








    Label {
        text: "Center"
        color: "white"
        anchors.centerIn: parent
    }
}








// 使用布局器
RowLayout {
    spacing: 20
    Button {
        text: "Button 1"
        // 设置Layout的附加属性
        Layout.fillWidth: true
        Layout.minimumWidth: 50
        Layout.preferredWidth: 100
        Layout.maximumWidth: 300
        Layout.minimumHeight: 150
    }








    Label {
        text: "Center"
    }








    Button {
        text: "Button 2"
        // 设置Layout的附加属性
        Layout.fillWidth: true
        Layout.minimumWidth: 100
        Layout.preferredWidth: 200
        Layout.preferredHeight: 100
    }
}

### 3.2.3.dtkwidget 和 dtkdeclarative 在信号处理上的区别
示例：实现按钮指定的点击事件
int main(int argc, char *argv[]) {
    QApplication app(argc, argv);








    QWidget widget;
    widget.resize(300, 300);
    QPushButton btn("Button", &widget);
    btn.move(50, 50);
    QObject::connect(&btn, &QPushButton::clicked, []() {
        qDebug() << "Button Clicked...";
    });
    widget.show();
}
// 使用 on<Signal> 的方式连接
Button {
    anchors.centerIn: parent
    text: "Button"
    // on<Signal> 进行信号槽连接
    onClicked: {
        console.log("Button clicked.....");
    }
}








// 使用Connections 方式连接
Button {
    id: btn
    anchors.centerIn: parent
    text: "Button"
}








// 5.15以下版本使用
Connections {
    target: btn
    onClicked: {
        console.log("Button clicked...")
    }
}
// 5.15以上版本使用
Connections {
    target: btn
    function onClicked() {
        console.log("Button clicked...")
    }
}
  
  
## 3.3.dtkdeclarative 模块和特性
  
dtkdeclarative 符合可维护、可扩展的架构特性，为不同场景需求分别设计模块，相关模块和介绍如下：
模块	版本	介绍	使用对象
org.deepin.dtk.impl	1.0	基础 c++ 实现	私有
org.deepin.dtk.controls	1.0	自定义空间，重写Qt基础控件	私有
org.deepin.dtk.style	1.0	控件风格，默认为行云设计风格	私有
org.deepin.dtk.settings	1.0	SettingsDialog控件相关接口	公开
org.deepin.dtk	1.0	dtk相关的统一对外接口	公开
基于上述模块，dtkdeclarative 特别为 DTK （developer toolkit，开发者工具套件）拓展颜色风格、视觉特效等方面的特性，比如调色板灵活定制、模糊算法性能调优等，更多特性和示例如下：
  
### 3.3.1.DCI （DSG combined icons，DSG混合图标）
  
DCI图标的实现来源于需求开发问题，用户界面需要展示更多状态图标，现有图标资源加载引擎技术效率不高，为解决对应技术问题，DCI图标技术应运而生。其原理结构如下：

DCI图标一种归档打包格式（后缀名 .dci ），格式名称为 dci (DSG combined icons，DSG[ DSG：Desktop Specification Group, UOS和deepin制作规范的工作组。]混合图标)，mimetype类型为 image/dci ，继承于 application/octet-stream 。magic值为 DCI ，类型为 string ，offset 为 0 。此类型对应的图标名： image-dci 。相应DCI资源路径示例如下：

基于DCI图标技术，用户界面开发能够满足多种交互状态的响应，能够满足多种图标类型的使用，能够满足多种DPI缩放规格的适应，在图标管理上更灵活，在需求实现上更优雅。
  
###   3.3.2.ColorSelector，颜色选择器/颜色控制器
Qt 原生调色板的主旨是让颜色外观管理更易于配置和保持一致，但 QWidgets 和 QML 对于 QPlette 的实现略有不同，比如后者仅实现一套颜色风格，而不再为控件不同状态绑定颜色，表现为需要手动指定颜色以实现多态控件需求。为了实现来自设计师的需求，dtkdeclarative 开发必须考虑颜色管理的可扩展性，解决方案就是专门打造一套取色系统（ColorSelector）用于替代 QPalette ，不过兼容性方面仍然保持 QPalette 可用。
ColorSelector 由调色板、控件属性和取舍器三个层次组成。其原理结构和代码示例如下：

Control {
    id: control
    width: 500
    height: 50
    
    property Palette backgroundColor: Palette {
        normal: "black"
    }
    property Palette textColor: Palette {
        normal: "white"
    }    








    contentItem: Text {
        text: "Test......."
        color: control.ColorSelector.textColor
        horizontalAlignment: Qt.AlignHCenter
        verticalAlignment: Qt.AlignVCenter
    }
    
    background: Rectangle {
        width: 250
        height: 50
        color: control.ColorSelector.backgroundColor
    }
}
那 ColorSelector 是如何实现设计师复杂需求的呢？这里就不得不提下 ColorSelector 的载体 Palette 了。
实现多态控件。通常情况下，控件的四种状态对应颜色的四种类别，只需分别指定相应颜色即可实现，代码示例如下：
Palette {
    id: palette
    normal: "red"
    hovered: "green"
    pressed: "blue"
    disabled: "black"
}
适配暗色主题。Palette 原生区分明暗主题，在暗色主题颜色管理中通过“Dark”标识，代码示例如下：
Palette {
    id: palette
    normal: "red"
    normalDark: "black"
    hovered: "green"
    hoveredDark: "yellow"
}
控制是否可用。默认该属性为启用状态，代码示例如下：
Palette {
    id: palette
    normal: "red"
    enabled: false
}
使用颜色族。提供“Common”和“Crystal”两套颜色族，“Common”为默认颜色族。代码示例如下：
Palette {
    normal {
        common: "#f0f0f0"
        crystal: Qt.rgba(0.20, 0.2, 0.2, 0.1)
    }
    hovered: "#d2d2d2"  //  common family
    pressed.crystal: "#cdd6e0"  //  crystal family
}
兼容 QPalette 。QPalette 中的高亮色（Hightlight）和高亮文本色（HighlightText）可以无缝使用，并且扩展支持通过亮度和不透明度微调颜色，微调范围限制为[-100,100]，代码示例如下：
Palette {
    normal: DTK.makeColor(Color.Highlight)
    hovered: DTK.makeColor(Color.Highlight).lightness(+10)
    pressed: DTK.makeColor(Color.Highlight).opacity(-10)
}
  
  
### 3.3.3.Settings 设置与配置框架
  
原有在 dtkwidget 下开发的 SettingDialog 使用 Json 文件声明布局和控件，这种方式对于布局实现存在一定局限性，不适用高级组合控件，会造成显示重叠、错乱等问题，在代码维护方面更是牵一发而动全身。
  
随着业务和需求的变化，解决融合 QML 开发的问题，借此时机摒弃 Json ，使用 dtkdeclarative 树状视图代码风格，提供一套可维护性和易读性俱佳的统一控件模板。如下即是全新 SettingDialog 的示例代码：
import org.deepin.dtk.settings 1.0 as Settings








Config {  // 创建配置项 用于关联到SettingDialog中
    id: config
    name: "example"
    property string key : "key default" // 配置文件中需要有同名键
}








Settings.SettingsDialog {
    height: 600
    width: 680
    config: config
    
    groups: [  // 创建配置组，管理配置子组和Options
        Settings.SettingsGroup {
            key: "group1"
            name: "group1"
            Settings.SettingsOption {
                key: "key"  // Option中的键可以直接绑定到config中同名的属性值
                name: "ComboBox"
                Settings.ComboBox {  // Settings封装的基础控件类型
                    model: ["first", "sceond", "three"]
                }
            }
            Settings.SettingsOption {
                key: "key2"
                name: "LineEdit"
                Settings.LineEdit {}
            }
            Settings.SettingsOption {
                key: "canExit"
                name: "CheckBox"
                Settings.CheckBox {}
            }
        }
    ]
}
  
#### SettingDialog 相关属性介绍如下：
属性名	子属性	描述
groups	-	配置项列表，用于添加SettingsGroup/SettingsOption配置项
config	-	应用配置管理
container	config	应用配置管理
	navigation Title	导航栏标题，可重载，默认使用Settings.NavigationTitle
	content Title	内容页标题，可重载，默认使用Settings.ContentTitle
	contentBackground	内容页背景，可重载，默认使用Settings.ContentBackground
	groups	配置项列表
navigationView	ListView中的子属性	导航列表
contentView	ListView中的子属性	内容页列表
  
#### 只是重写了 SettingDialog 么？上述代码示例中的 config 属性表示还有“料”。
为了更高效地对接 Config配置方案[ Config配置方案：统一配置文件标准类似于Gsettings的schemes文件，能够解决个性化功能配置的存储、修改等问题。除提供配置文件的读写标准接口，还提供图形化界面管理工具，满足开发者、桌面用户需要，同时能够满足OEM、域管、云桌面等使用方要求。]，将 SettingDialog 中的设置选项与 Config 中的配置选项打通，符合qml语法且使用 dtkdeclarative 开发的全新Settings框架由此诞生，实现“一处声明，全局可用”。
如果你的应用需求类似此场景：用户需要更多设置选项，这些选项不适合全部展示，经过用户调研和需求分析，用户能够接受配置文件方式。那么，Settings框架非常适合，参见下述示例代码，只需要遵循SettingDialog模板和Config配置方案相关规范，你的应用就能完美实现需求，并且无需关注键值同步、信号监听等底层逻辑。
Config {
    id: config
    name: "example"  // 必须指定
    property int key1 : 1
    property string key2 : "key2 default"
    
    onKey2Changed: {
        console.info("Config Value changed...")
    }
}








// 1. 直接进行属性绑定
Label {
    text: "property binding key3:" + exampleConfig.key3
}








// 2. Qt.binding 进行属性绑定
Label {
    text: "config.key2"
    Component.onCompleted: {
        text = Qt.binding(function(){ return config.key2 })
    }
}