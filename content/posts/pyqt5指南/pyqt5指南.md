+++
title = 'Pyqt5指南'
date = 2023-11-25T11:48:21+08:00
draft = true
+++

使用python我们也可以开发桌面应用（在pc上运行的软件，pyqt5好像还不支持手机app）平时做做小工具还是挺方便的。这次我们一起学习一下pyqt5

## 环境安装

```bash
pip install pyqt5
pip install pyqt5-tools
```

python 版本不要过高， 需要适配pyqt5 ，像3.8，3.9 是可以的，高版本可能会出现适配问题

## 开发流程

整体开发逻辑可以类比网页，有很多相似之处：

- 各种控件 相当于 html的各种标签
  - 常用控件
    - button
    - checkbox
    - radio button
    - lineedit
    - combo box
    - label
  - 布局
    - 水平
    - 垂直
    - 网格
- qss 相当于 css
  - 用于设置颜色
- 信号和槽函数 相当于 js
  - 使设计的界面动起来

### 使用designer.exe绘制应用的界面，保存为*.ui文件

在下载pyqt5的虚拟环境目录搜索设计师应用的关键字 **designer**, 打开这个应用就可以设计界面了

![VeryCapture_20221007201335](../imgs/VeryCapture_20231125121014.jpg)

绘制界面的流程如下：

1. 拖拽各个使用的控件
2. 选择n个控件， 设置它们的布局（水平， 垂直， 网格）
3. 给需要控制的控件设置**objectName** ,方便后续写代码寻找这个控件

### 使用命令把*.ui 转为*.py文件

```console
pyuic5 ui文件名 -o py文件名
```

### 继承生成的*.py文件的类, 进行功能开发

1. 搭建结构使gui运行

   ```python
   from PyQt5.QtCore import Qt
   from PyQt5.QtWidgets import QApplication, QMainWindow, QCheckBox, QMessageBox, QInputDialog, QLineEdit
   from PyQt5.QtGui import QIcon
   from my_clock import Ui_MainWindow
   import sys
   
   # 1. 继承*.py文件的类Ui_MainWindow
   class MyGui(QMainWindow, Ui_MainWindow):
   
        def __init__(self) -> None:
            super().__init__()
            self.setupUi(self)
            # 设置窗口最大最小值和关闭按钮
            self.setWindowFlags(Qt.WindowMinMaxButtonsHint | Qt.WindowCloseButtonHint)  
    
            # 2. 设置信号和槽函数的连接: clicked是一个信号, 设置它连接槽函数self.run
            self.pushButton_run.clicked.connect(self.run)  
           
        # 槽函数
        def run(self):
           pass
   
   
   if __name__ == '__main__':
        app = QApplication(sys.argv)
        app.setWindowIcon(QIvon('logo.png'))  # 设置icon

        mygui = MyGui()
        mygui.show()
    
        sys.exit(app.exec_())
   
   ```

2. 进行信号和槽函数的编写

3. 当遇到耗时操作(运行超过1s的代码)，为了防止界面卡死，需要使用**线程**启动

   使用python中的 thread 或者 pyqt5的 Qthread来执行耗时函数

### 项目打包

先安装pyinstaller模块

```console
pip install pyinstaller
```

然后打包为可执行文件

```console
pyinstaller -F -w 执行的py文件
```

### 其他实用功能

- QFileDialog 文件选择
- QMessageBox 弹框

### 项目实战案例

<https://gitee.com/zhenda/random_recipe>
