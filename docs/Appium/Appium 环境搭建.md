# adb 的基本操作

**获取设置应用程序的包名和界面名**

- 获取包名和界面名

  - mac

  ```
  adb shell dumpsys window windows | grep mFocusedApp``
  ```

  - windows

  ```
  adb shell dumpsys window windows | findstr mFocusedApp
  ```

- 打开设置应用程序

- 输入命令

- 得到结果

> mFocusedApp=AppWindowToken{ed72aac token=Token{9d3605f ActivityRecord{cfae0fe u0 com.android.settings/.Settings t7}}}

包名:`com.android.settings`
界面名:`.Settings`
注意：界面名在有些文章中可能会被叫做启动名

**一、adb 文件传输**

**发送文件到手机**
命令格式

```
adb push 电脑的文件路径 手机的文件夹路径
```

示例
将电脑桌面的a.txt 文件推到手机的sd卡

```
C:\Users\Administrator>adb push C:\Users\Administrator\Desktop\a.txt /sdcard
C:\Users\Administrator\Desktop\a.txt: 1 file pushed, 0 skipped. 0.0 MB/s (6 bytes in 0.003s)
```

**从手机中拉取文件**
命令格式

```
adb pull 手机的文件夹路径 电脑的文件路径 
```

示例
从手机的sd 卡的a.txt拉到电脑桌面

```
C:\Users\Administrator>adb pull /sdcard/a.txt  C:\Users\Administrator\Desktop
/sdcard/a.txt: 1 file pulled, 0 skipped. 0.0 MB/s (6 bytes in 0.003s)
```

**二、获取app启动时间**
命令格式

```
adb shell am start -W 包名/启动名   
```

示例
启动设置界面并进入主界面

```
C:\Users\Administrator>adb shell am start -W com.android.settings/.Settings
Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] cmp=com.android.settings/.Settings }
Status: ok
Activity: com.android.settings/.Settings
ThisTime: 1232
TotalTime: 1232
WaitTime: 1248
Complete
```

**ThisTime：** 该界面启动毫秒
**TotalTime：** 应用自身启动耗时= ThisTime+应用application 等资源启动耗时
**WaitTime：** 系统启动应用耗时=TotalTime + 系统资源启动时间
**三、获取手机日志**
命令格式

```
adb logcat   
```

**四、其它命令**

- 安装app 到手机

```
adb install apk路径
```

- 卸载手机上的app

```
adb uninstall apk路径
```

- 查看连接设备的数量及设备号

```
adb devices
```

- 进入到android手机系统内部的命令行中

```
adb shell
```

- 关闭adb服务

```
adb kill-server
```

- 开启adb 命令

```
adb start-server
```

- 查看adb帮助

```
adb --help
```

# Appium 基本使用

## 打开一个应用程序

1. 打开手机模拟器
2. 打开appium 工具
3. 创建一个python 项目
4. 创建一个demo.py文件
5. 写入以下代码

```
from appium import webdriver
import time
# 创建一个字典,字典里至少写入以下5个参数，这是appium 的必备参数
desired_cap = dict()
# 需要连接的手机的平台，不限大小写
desired_cap['platformName'] = 'Android'
# 平台的版本
desired_cap['platformVersion'] = '6.0'
# 设备的名字，可以随便写，但不能乱写
desired_cap['deviceName'] = '192.168.208.101:5555'
# 要打开的应用程序
# desired_cap['appPackage'] = 'com.android.settings'

desired_cap['appPackage'] = 'com.android.messaging'
# 要打开的界面
# desired_cap['appActivity'] = '.Settings'
desired_cap['appActivity'] = '.ui.conversationlist.ConversationListActivity'
```

## 启动过程

appium 的启动实际上是在本机使用了4723端口开启了一个服务

1. 我们写的python 代码会访问本机的appium 服务器，并获取driver 对象
2. appium 会将我们 的driver 对象调用的方法转化成 post 请求，提交给appipum 服务器
3. appium 通过接收到的 post 请求发送给手机，再由手机进行执行.

## Appium 基础操作API

**1.1 在脚本内启动其它app**
**应用场景**
如果一个应用需要跳转到其它应用，就可以使用这个api进行应用的跳转，就像我们通过外卖应用下订单之后会跳转到支付页面一样.
**方法名和参数**

- appPackage:要打开的程序的包名
- appActivity:要打开的程序的界面名

```
driver.start_activity(appPackage,appActivity)
```

**先打开设置应用，等待三秒跳转到短信界面**

```
driver = webdriver.Remote("http://localhost:4723/wd/hub",desired_cap)
time.sleep(3)
driver.start_activity('com.android.messaging','.ui.conversationlist.ConversationListActivity')
time.sleep(3)
driver.quit()
```

**1.2 获取app 的包名和界面名**
**应用场景**
当我们从一个应用跳转到另外一个应用的时候，想输出其包名，界面名或者想在报告中展现对象信息，我们就可以调用这个属性来进行获取
**方法和属性**

- 获取包名

```
driver.current_package
```

- 获取界面名

```
driver.current_activity   
```

**示例**

```
driver = webdriver.Remote("http://localhost:4723/wd/hub",desired_cap)
time.sleep(3)
print(driver.current_package)
print(driver.current_activity)
driver.start_activity('com.android.messaging','.ui.conversationlist.ConversationListActivity')
time.sleep(3)
print(driver.current_package)
print(driver.current_activity)
driver.quit()
```

结果

```
com.android.settings
.Settings
com.android.messaging
.ui.conversationlist.ConversationListActivity
```

**1.3 关闭app和驱动对象**
**方法**

- 关闭当前app，但driver 对象还在

```
driver.close_app()
```

- 关闭当前app,同时关闭所有关联的app,关闭了driver对象

```
driver.quit()
```

**示例**

```
driver = webdriver.Remote("http://localhost:4723/wd/hub",desired_cap)
time.sleep(3)
driver.close_app()
print(driver.current_package)
```

结果

```
# 下面这个是正常得到的桌面的包名  
com.android.launcher3
```

如果调用`driver.quit()`后获取包名，则会报错，因为driver 对象已经被关闭了.
**1.4 安装、卸载及判断应用是否被安装**
**应用场景**
一些应用市场的软件可能会有一些按钮，如果某一个程序已经安装则显示卸载，如果没有安装则显示安装
**方法和参数**

- 安装app

```
"""
参数： 
    app_path:apk 路径
"""
driver.install_app(app_path)
```

- 卸载app

```
"""
参数： 
    app_id:应用程序包名
"""
driver.remove_app(app_id)
```

- 判断app是否已经安装

```
"""
参数：  
    app_id:应用程序包名  
返回值：布尔类型
    True:安装
    False:未安装
"""
driver.is_app_installed(app_id)
```

**1.5 将应用置于后台**
**应用场景**
银行类app 会在进入后台一定时间后，如果再回到前台页面也会再输入密码，如果需要自动化测试这种功能，可以使用这个api 进行测试
**方法**

```
"""
seconds:进入后台的时间   
"""
driver.background_app(seconds)
```

这个方法会自动回到前台
**打开设置应用进入后台5秒，再回到前台**

```
driver = webdriver.Remote("http://localhost:4723/wd/hub",desired_cap)
# 进入后台5秒再回到前台
print("进入后台")
driver.background_app(5)
print("回到前台")
time.sleep(3)
driver.quit()
```

结果：

```
进入后台
回到前台
```

**注意点**

- 热启动:表示进入后台回到前台
- 冷启动:关机再开这种切断电源的行为可以叫做"冷启动"

# UIAutomatorViewer 的使用

**应用场景**
定位元素必须根据元素的相关特征来进行定位，而UIAutomatorViewer就是用来获取元素特征的
**简介**
UIAutomatorViewer用来扫描和分析Android 应用程序的 UI 控件的工具
**使用步骤**

- SDK安装目录下的tools文件夹，打开UIAutomatorViewer.bat
- 电脑连接真机或打开android 模拟器
- 启动待测试app
- 点击UIAutomatorViewer 左上角的device Screenshot(从左数第二个按钮)
- 点击希望查看的控件
- 查看右下角Node Detail 的相关信息

# 元素定位操作

- 元素定位操作需要采用前面刚提到的UIAutomatorViewer
- 元素定位是基于当前屏幕范围内展示的可见元素

**一、通过id,class,xpath 定位某一个元素**
想要对按钮进行点击，想要对输入框进行输入，想要获取文本框的内容，定位元素是自动化操作必须要使用的方法，只有获取元素之后，才能对这个元素进行操作。
**使用id 进行定位**

```
"""
参数:
    id_value:元素的resource-id 属性值
返回值:
    定位到的单个元素   
"""
driver.find_element_by_id(id_value)
```

- 通过 class 定位一个元素

```
""" 
参数:
    class_value:元素的 class 属性值
返回值:
    定位到的单个元素   
"""
driver.find_element_by_class_name(class_value)
```

- 通过xpath 定位一个元素

```
""" 
参数:
    xpath_value:定位元素的 xpath 表达式
返回值:
    定位到的单个元素   
"""
driver.find_element_by_xpath(xpath_value)
```

**示例**

- 通过id 定位放大镜按钮并点击
- 通过class的形式，定位"输入框",输入"hello"
- 通过xpath 的形式，定位"返回"按钮，并点击

```
driver = webdriver.Remote("http://localhost:4723/wd/hub",desired_cap)
# 通过id 定位放大镜按钮，点击
driver.find_element_by_id("com.android.settings:id/search").click()
# 通过class定位输入框，输入hello
driver.find_element_by_class_name("android.widget.EditText").send_keys("hello")
# 通过xpath定位返回按钮，点击
driver.find_element_by_xpath("//*[@content-desc='收起']").click()
driver.quit
```

**注意**

- 如果有很多个元素的特征是相同的，使用定位元素得到方法会找到第一个
- 尽量去找有唯一性的特征去定位元素
- 如果传入一个没有的特征，会报NoSuchElementException异常

**二、通过id,class,xpath 定位某一组元素**
和定位一个元素相同，但如果想要批量的获取某个相同特征的元素，使用定位一组元素的方式更加方便
**方法**

- 通过id 定位一组元素

```
"""
参数:
    id_value:元素的resource-id 属性值
返回值:
    列表，定位到的所有符合条件的元素 
"""
driver.find_elements_by_id(id_value)
```

- 通过 class 定位一组元素

```
""" 
参数:
    class_value:元素的 class 属性值
返回值:
    列表，定位到的所有符合条件的元素
"""
driver.find_elements_by_class_name(class_value)
```

- 通过xpath 定位一组元素

```
""" 
参数:
    xpath_value:定位元素的 xpath 表达式
返回值:
    列表，定位到的所有符合条件的元素
"""
driver.find_elements_by_xpath(xpath_value)
```

**示例**

- 通过id的形式，获取所有resource-id 为com.android.settings:id/title的元素，并打印其文字内容
- 通过class的形式，获取所有class 为 android.widget.TextView 的元素，并打印其文字内容
- 通过xpath 的形式，获取所有包含'设'的元素，并打印其文字内容

```
driver = webdriver.Remote("http://localhost:4723/wd/hub",desired_cap)
# 通过id的形式，获取所有resource-id 为com.android.settings:id/title的元素，并打印其文字内容
titles = driver.find_elements_by_id("com.android.settings:id/title")
for var in titles:
    print(var.text,end=",")
print()
# 通过class的形式，获取所有class 为 android.widget.TextView 的元素，并打印其文字内容
textViews = driver.find_elements_by_class_name("android.widget.TextView")
for var in textViews:
    print(var.text,end=",")
print()
# 通过xpath 的形式，获取所有包含'设'的元素，并打印其文字内容
xpath = driver.find_elements_by_xpath("//*[contains(@text,'设')]")
for var in xpath:
    print(var.text,end=",")
```

结果:

```
WLAN,流量使用情况,更多,显示,提示音和通知,应用,
设置,,无线和网络,WLAN,流量使用情况,更多,设备,显示,提示音和通知,应用,
设置,设备,
```

**注意点**

- 如果传入一个没有的特征,会返回一个空的列表

# 元素操作

获取到元素之后就可以对元素进行操作了
**一、点击元素**

```
element.click()      
```

**二、输入内容**

```
element.send_keys(value)   
```

示例

```
# 输入
driver.find_element_by_class_name("android.widget.EditText").send_keys("hello")
```

注意：默认输入中文无效，但不会报错，需要在前置代码中加入两个参数

```
desired_cap['unicodeKeyboard'] = True
desired_cap['resetKeyboard'] = True
```

**三、清空内容**

```
element.clear()      
```

**四、获取元素的文本内容**

```
element.text        
```

示例

```
# 通过id的形式，获取所有resource-id 为com.android.settings:id/title的元素，并打印其文字内容
titles = driver.find_elements_by_id("com.android.settings:id/title")
for var in titles:
    print(var.text,end=",")
```

**五、获取元素的位置和大小**

```
"""
返回值:
    字典，x为元素的x坐标，y 为元素的 y 坐标
"""
element.location
"""
返回值:   
    字典，width 为宽度，height 为高度   
"""
element.size
```

**示例**

```
## 获取搜索按钮的位置和大小
search = driver.find_element_by_id("com.android.settings:id/search")
print(search.location)
print(search.size)
```

**六、获取元素的属性值**

```
"""
参数:   
    value，要获取的属性名   
返回值:   
    根据属性名得到的属性值   
"""
element.get_attribute(value) #value元素的属性
```

**示例**

1. 打开设置
2. 获取所有resource-id 为com.android.settings:id/title的元素
3. 使用get_attribute()获取这些元素的enabled,text,content-desc,resource-id,class 的属性值

```
driver = webdriver.Remote("http://localhost:4723/wd/hub",desired_cap)
'''   
1. 打开设置                                                               
2. 获取所有resource-id 为com.android.settings:id/title的元素                                       
3. 使用get_attribute()获取这些元素的enabled,text,content-desc,resource-id,class 的属性值                      
'''
titles = driver.find_elements_by_id("com.android.settings:id/title")
for title in titles:
    print(title.get_attribute("enabled"),end=",")
    print(title.get_attribute("text"), end=",")
    print(title.get_attribute("name"), end=",")
    print(title.get_attribute("resourceId"), end=",")
    print(title.get_attribute("className"))
```

结果

```
true,WLAN,WLAN,com.android.settings:id/title,android.widget.TextView
true,流量使用情况,流量使用情况,com.android.settings:id/title,android.widget.TextView
true,更多,更多,com.android.settings:id/title,android.widget.TextView
true,显示,显示,com.android.settings:id/title,android.widget.TextView
true,提示音和通知,提示音和通知,com.android.settings:id/title,android.widget.TextView
true,应用,应用,com.android.settings:id/title,android.widget.TextView
```

**注意点**

> value='text' 返回text的属性值
> value= 'name' 返回content-desc /text 属性值
> value = 'className' 返回class 属性值，只有API>=18才能支持
> value = 'resourceId' 返回resource-id属性值，只有API>=18才能支持
> 其它的参考uiAutomator viewer 中的属性名

# 元素等待

由于一些原因，我们想找的元素并没有立刻出来，此时如果直接定位可能会报错，如以下原因:

- 由于网络速度原因
- 服务器处理请求原因
- 电脑配置原因

WebDriver 定位页面元素时如果未找到，会在指定时间内一直等待的过程，分为两种方式

- 显式等待
- 隐式等待

找元素的时候，通过一个时间的设置，进行等待元素，等待元素出来之后，再来定位，防止报错
**一、显式等待**
**应用场景**

针对所有定位元素的超时时间设置为不同值的时候
等待元素加载指定的时长，超出时长还未加载出来时抛出TimeoutException

**步骤**

- 导包

```
from selenium.webdriver.support.wait import WebDriverWait
```

- 创建WebDriverWait 对象

```
wait = WebDriverWait(driver,5)
```

- 调用WebDriverWait 对象的 until 方法
  这个until方法的参数是一个函数，当

**示例**
在5秒钟内，每1秒在《设置》程序中的“返回” 按钮，如果找到则点击，如果找不到则观察对应错误信息

```
from selenium.webdriver.support.wait import WebDriverWait
driver = webdriver.Remote("http://localhost:4723/wd/hub",desired_cap)
wait = WebDriverWait(driver,5)
back_button = wait.until(lambda x:x.find_element_by_xpath("//*[@content-desc='收起']"))
print("准备找返回并点击~")
back_button.click()
print("点完了~")
driver.quit()
```

结果:

```
准备找返回并点击~
点完了~
```

**注意**

- 在设置了显式等待之后，可以等待一个超时时间，在这个超时时间之内进行查找，默认每0.5秒找一次
- 0.5 秒的频率是可以设置的
- 一旦找到这个元素，直接进行后续操作
- 如果没有找到，报错，TimeOutException

**WebDriverWait()**

```
WebDriverWait(driver,timeout,poll_frequency=0.5,ignored_exceptions=None)
```

- driver：浏览器驱动
- timeout：最长超时时间，默认以秒为单位
- poll_frequency：检测的间隔步长，默认为0.5s
- ignored_exceptions：超时后的抛出的异常信息，默认抛出NoSuchElementExeception异常

**与until()或者until_not()方法结合使用**

> WebDriverWait(driver,10).until(method，message="")
> 调用该方法提供的驱动程序作为参数，直到返回值为True

> WebDriverWait(driver,10).until_not(method，message="")
> 调用该方法提供的驱动程序作为参数，直到返回值为False

在设置时间（10s）内，等待后面的条件发生。如果超过设置时间未发生，则抛出异常。在等待期间，每隔一定时间（默认0.5秒)，调用until或until_not里的方法，直到它返回True或False。

**二、隐式等待**
**应用场景**
针对所有定位元素的超时时间设置为同一个值的时候
等待元素加载指定的时长，超出时长抛出NoSuchElementException异常
**步骤**
在获取driver 对象后，会用driver调用implicitly_wait方法即可

```
driver.implicitly_wait(10)
```

**示例**

```
driver = webdriver.Remote("http://localhost:4723/wd/hub",desired_cap)
time.sleep(3)
driver.close_app()
print(driver.current_package)
```

结果:

```
准备找返回并点击~
点完了~
```

**隐式等待和显式等待的选择**
**作用域**
显式等待为单个元素有效，隐式为全局元素

**方法**
显式等待方法封装在WebDriverWait类中，而隐式等待则直接通过driver 实例化对象调用

**关于sleep的形式**

- sleep 是固定死一个时间，不是不行是不推荐
- 元素等待可以让元素出来的第一时间进行操作，sleep 可能造成不必要的浪费

**选择**

1. 从使用的角度上

- 隐式等待更简单
- 显式等待相对复杂

1. 从灵活性的角度上

- 显式等待更加灵活，因为可以针对每一个元素进行单独的设置
- 隐式等待是针对全局的定位元素

# 滑动和拖拽事件

在做自动化测试的时候，有些按钮需要滑动几次屏幕后才会出现，此时，我们需要使用代码来模拟手指的滑动

**一、swipe滑动事件**
从一个坐标位置滑动到另一个坐标位置，只能是两个点之间的滑动
**方法**

```
'''
参数：
    start_x:起点x轴坐标
    start_y:起点y轴坐标   
    end_x:终点x轴坐标   
    end_y:终点y轴坐标   
    duration:滑动这个操作一共持续的时间长度，单位：ms
    （这个参数是有默认值的）  
'''
driver.swipe(start_x,start_y,end_x,end_y,duration)
```

**示例**
模拟手指从（1000,2000）到（1000,1000）的位置

```
driver = webdriver.Remote("http://localhost:4723/wd/hub",desired_cap)
driver.swipe(100,1000,100,500)
driver.quit()
```

遇到的问题
在滑动的时候本来y坐标是2000到1000，一直报错，显示手机屏幕的坐标范围为[0,0][0,1280], 所以在写代码前最好看屏幕大小

**注意点**

- 参数是坐标点
- 持续时间短，惯性大
- 持续时间长，惯性大
- 因为每个屏幕的分辨率不一样，所以在一个手机上适用不一定在所有手机上适用，可以动态的获取手机分辨率再进行滑动

**二、scroll 滑动事件**
**概念**
从一个元素滑动到另一个元素，直到页面自动停止
**方法**

```
'''
参数:
    origin:滑动开始的元素
    destination:滑动结束的元素
'''
driver.scroll(origin,destination)
```

**示例**

```
save = driver.find_element_by_xpath("//*[@text='存储']")
more = driver.find_element_by_xpath("//*[@text='更多']")
driver.scroll(save,more)
```

**特点**
不能设置持续时间，惯性大

**三、drag_and_drop 拖拽事件**
**概念**
从一个元素滑动到另一个元素，第二个元素替代第一个元素原本屏幕上的位置

**方法**

```
'''
参数:
    origin:滑动开始的元素
    destination:滑动结束的元素
'''
driver.drag_and_drop(origin,destination)
```

**示例**

```
save = driver.find_element_by_xpath("//*[@text='存储']")
more = driver.find_element_by_xpath("//*[@text='更多']")
driver.drag_and_drop(save,more)
```

**特点**
不能设置持续时间，没有惯性

**滑动和拖拽事件的选择**
滑动和拖拽主要考虑的是是否有惯性，以及传递的参数是否是坐标

- 有惯性，传入元素:scroll
- 无惯性，传入元素:drag_and_drop
- 有惯性，传入坐标:swipe,设置较短的duration 时间
- 无惯性，传入坐标:swipe,设置较长的duration 时间

# 手机操作

**一、获取手机分辨率**
因为每个手机的分辨率不一致，所以一般我们通过动态获取分辨率来获取一些点击或者滑动的坐标
**方法名**

```
get_window_size()
```

**示例**
获取手机屏幕分辨率并打印

```
print(driver.get_window_size())
```

结果

```
  {'width': 720, 'height': 1280}
```

**二、获取手机截图**
有些时候自动化操作可能也没有反应，但不报错，此时就可以将操作过后的关键情况，截图留存，后期也可以根据图片发现问题
**方法**

```
"""
参数:   
    filename:指定路径下，指定格式的图片
"""
get_screenshot_as_file(filename)
```

**示例**
截图并保存

```
driver.get_screenshot_as_file("screen.png")
driver.get_screenshot_as_file("C:\\Users\\Administrator\\Desktop\\screen.png")
```

**注意点**

- 如果直接写了文件名，则会默认保存在项目目录下

**三、获取和设置网络状态**
视频应用在使用流量看视频的时候，大部分都会提示用户是否继续播放，作为测试人员，我们可能需要用自动化的形式来判断是否有对应的提示，即，用流量的时候应该有提示，不用流量的时候应该没有提示
**方法**

```
# 获取网络状态
print(driver.network_connection)
# 设置  
driver.set_network_connection(6)
```

appium 各项连接的api

| Value              | Date | wifi | Airplane Mode |
| ------------------ | ---- | ---- | ------------- |
| 0 (None)           | 0    | 0    | 0             |
| 1 (Airplane Mode)  | 0    | 0    | 1             |
| 2 (Wifi only)      | 0    | 1    | 0             |
| 4 (Data only)      | 1    | 0    | 0             |
| 6 (All network on) | 1    | 1    | 0             |

appium 还有一个枚举类,一般不建议使用数字，而是使用这个类的变量，代码看起来比较清晰

```
class ConnectionType(object):
    NO_CONNECTION = 0
    AIRPLANE_MODE = 1
    WIFI_ONLY = 2
    DATA_ONLY = 4
    ALL_NETWORK_ON = 6
```

这个类在另一个包里，需要引入一下

```
from appium.webdriver.connectiontype import ConnectionType
```

**四、发送键到设备**
模拟按"返回键","home键等等操作",比如,很多应用有按两次返回键退出应用的功能，如果这个功能更需要我们做自动化，那么一定会用到这个方法
**方法名**

```
"""   
参数：   
    keycode:发送给设备的关键代码   
    metastate:关于被发送的关键代码的元信息,一般为默认值   
"""
driver.press_keycode(keycode,metastate)
```

**示例**
点击三次音量加，再点击返回，再点击两次音量减

```
driver.press_keycode(24)
time.sleep(2)
driver.press_keycode(24)
time.sleep(2)
driver.press_keycode(24)
time.sleep(2)
driver.press_keycode(4)
time.sleep(2)
driver.press_keycode(25)
time.sleep(2)
driver.press_keycode(25)
```

**注意点**
按键对应的编码，可以在百度搜索关键字"android keycode"

**五、打开和关闭手机通知栏**
测试即时通信类软件的时候，如果A给B发送一条消息，B的通知栏肯定会显示对应的消息，我们想通过通知栏来判断B是否收到消息，一定要先操作手机的通知栏
**方法**

```
driver.open_notifications()
```

**示例**
打开通知栏，两秒后，关闭通知栏

```
print("打开通知栏")
driver.open_notifications()
time.sleep(2)

print("关闭通知栏")
size = driver.get_window_size()
width = size["width"]
height = size["height"]
driver.swipe(0.5*width,0.75*height,0.5*width,0.25*height)
```

或者也可以使用`driver.press_keycode(4)`(返回键)来进行关闭.
**注意点**

- appium 官方并没有为我们提供关闭通知的api,那么现实中怎么关闭就怎么操作就行，比如手指滑动，或者按返回键

# 高级手势

TouchAction 可以实现一些针对手势的操作，比如滑动、长按、长拖等，我们可以将这些基本手势组合成一个相对复杂的手势。
比如，我们解锁手机或者一些应用软件都有手势解锁这种方式。
**使用步骤**

1. 创建TouchAction 对象
2. 通过对象调用想执行的手势
3. 通过perform()执行动作

**注意点**
所有手势都要通过执行perform()函数才会运行

**一、轻敲手势**
模拟手指对某个元素或坐标按下并快速抬起
**方法名**

```
'''
参数:
    element 元素  
    x:x坐标  
    y:y坐标
'''
TouchAction(driver).tap(element=None,x=None,y=None).perform()
```

**示例**

```
TouchAction(driver).tap(driver.find_element_by_xpath("//*[@text='WLAN']")).perform()
```

**注意**
多次点击使用count参数

**二、按下手势**
模拟手指一直按下，模拟手指抬起。可以用来组合成轻敲或者长按的操作
**方法名**

```
''' 
参数:   
    el：元素   
    x:x 坐标
    y:y 坐标   
'''
TouchAction(driver).press(el=None,x=None,y=None).perform()
```

**示例**

```
button = driver.find_element_by_xpath("//*[@text='WLAN']")
# 轻敲
TouchAction(driver).tap(button).perform()
```

**三、抬起手势**
模拟手指对元素或者坐标的抬起操作

```
TouchAction(driver).release().perform()
```

**示例**

```
button = driver.find_element_by_xpath("//*[@text='WLAN']")
# 摁下
TouchAction(driver).press(button).perform()
```

**四、等待操作**
模拟手指的等待，比如按下后等待5秒后再抬起
**方法名**

```
''' 
参数:   
    ms：暂停的毫秒数  
'''
TouchAction(driver).wait(ms=0).perform()
```

**示例**

```
button = driver.find_element_by_xpath("//*[@text='WLAN']")
TouchAction(driver).tap(button).perform()
TouchAction(driver).press(x=650,y=650).wait(2000).release().perform()
```

**五、长按手势**
模拟手指对元素或坐标的长按操作
**方法名**

```
"""
参数：
    el：元素   
    x:x 坐标
    y:y 坐标 
    duration:长按时间，毫秒
"""
TouchAction(driver).long_press(el=None,x=None,y=None,duration=1000).perform()
```

**示例**

```
button = driver.find_element_by_xpath("//*[@text='WLAN']")
# 轻敲
TouchAction(driver).tap(button).perform()
time.sleep(2)
TouchAction(driver).long_press(x=200,y=320,duration=2000).perform()
```

**六、手指移动操作**
模拟手指移动操作，比如手势解锁需要先按下屏幕，再移动
**方法名**

```python
"""
参数：
    el：元素   
    x:x 坐标
    y:y 坐标     
"""   
TouchAction(driver).move_to(el=None,x=None,y=None).perform()
```