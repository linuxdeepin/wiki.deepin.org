---
title: PyQt5初步界面
description: 
published: true
date: 2022-08-07T04:32:26.051Z
tags: 
editor: markdown
dateCreated: 2022-08-07T04:32:23.291Z
---

# 安装库
在终端输入
```bash
sudo apt install python3-pyqt5
```

即可安装环境
## Tips
不推荐使用 pip 安装 PyQt5，因为使用这种方式安装的 PyQt5 得不到系统的优化，而且还会干扰到 apt 安装的`python3-pyqt5`，如下：
**未用 pip 安装：**
![2022-8-7_33311.png](/2022-8-7_33311.png)
**用 pip 安装：**
![2022-8-7_1363.png](/2022-8-7_1363.png)
而且还存在无法使用 fcitx 输入法输入等问题
如果真这样操作了怎么办？只需要输入如下命令即可：
```bash
pip3 uninstall pyqt5
# 如果你安装到 root 目录下，可以加 sudo
sudo pip3 uninstall pyqt5
```

# 基础模板
只供参考
```python
#!/usr/bin/env python3
import sys
import PyQt5.QtWidgets as QtWidgets
app = QtWidgets.QApplication(sys.argv)
widget = QtWidgets.QWidget()
widget.show()
sys.exit(app.exec_())
```
![2022-8-7_77785.png](/2022-8-7_77785.png)

# 高级点的示例
```python
#!/usr/bin/env python3
import sys
import PyQt5.QtWidgets as QtWidgets

def HelloWorld():
    QtWidgets.QMessageBox.information(widget, "Tips", "Hello World!")

app = QtWidgets.QApplication(sys.argv)
window = QtWidgets.QMainWindow()
widget = QtWidgets.QWidget()
widgetLayout = QtWidgets.QVBoxLayout()

button = QtWidgets.QPushButton("Hello World!")
button.clicked.connect(HelloWorld)
widgetLayout.addWidget(button)

widget.setLayout(widgetLayout)
window.setCentralWidget(widget)
window.show()
sys.exit(app.exec_())
```
可以实现按下按钮显示对话框的功能
![2022-8-7_55486.png](/2022-8-7_55486.png)