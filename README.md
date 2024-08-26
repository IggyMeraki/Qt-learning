## .ui文件转py文件

```bash
pyuic5 -o qt_ui.py qt.ui
```

## 主页面初始化

```python
class MyMainForm(QMainWindow, Ui_MainWindow):
    def __init__(self):
        super(MyMainForm, self).__init__()
        self.setupUi(self)
        ## 初始化文本框
        self.communicator = Communicator(self) # 通过self 将Ui_MainWindow 中的按键参传入
```

## 固定的main函数

```python
if __name__ == "__main__":
    #固定的，PyQt5程序都需要QApplication对象。sys.argv是命令行参数列表，确保程序可以双击运行
    app = QApplication(sys.argv)
    #初始化
    myWin = MyMainForm()
    #将窗口控件显示在屏幕上
    myWin.show()
    #程序运行，sys.exit方法确保程序完整退出。
    sys.exit(app.exec_())
```

## 按键检测

```python
self.pushButton_connect_start_audio.clicked.connect(lambda: self.state_change_audio(True))
```

*pushButton_connect_start_audio*: 按键名

*clicked* 按键动作

*lambda: self.state_change_audio(True)* 点击后要执行的东西

## checkbox

```python
self.checkBox.setCheckable(False) 
self.checkBox.setChecked(False)       self.checkBox.stateChanged.connect(self.onCheckBoxStateChanged)
```

setCheckable: 设定是否能被勾选,设定为False时,不检测勾选操作

setChecked: 设定勾选状态

stateChanged: 状态改变时.....

## 子类调用主页面元素

当我们为某个元素创建一个子类来控制它时,我们需要调用到主页面的各个元素,这个时候在主页面我们需要传入一个self,在子类里就可以将其视为main window

例如: 我们需要对combobox_yaml这个元素创建一个控制类,在主页面中只要这么写

```python
self.comboBox_yaml_ctrl = YamlComboBoxContrl(self,self.pushButton_yamldic_refresh,dict_yaml=self.file_path)
```

- YamlComboBoxContrl即为创建的类,初始化传入*main_window*,*refresh_btn*,*dict_yaml*,三个参数
- self 在YamlComboBoxContrl类中即可充当主页面来调用
- self.pushButton_yamldic_refresh 也可以传入主页面中实例化的按钮或其他元素

那么在YamlComboBoxContrl类的初始化我们就可以这么写:

```python
class YamlComboBoxContrl():
    def __init__(self,main_window,refresh_btn,dict_yaml):
        self.dict_yaml = dict_yaml
        self.main_window = main_window
        self.refresh_btn = refresh_btn
      	self.refresh_btn.clicked.connect(self.populate_combobox)
        self.populate_combobox()
```

如果说主页面有个comboBox的名字叫comboBox_yaml,在子类中就可以这样调用

```python
self.main_window.comboBox_yaml.clear()
```

