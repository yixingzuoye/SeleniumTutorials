# 自动化测试-[零壹码博客](https://lingyima.com)

## 软件开发流程

1. 需求分析
2. 架构模块设计
3. 编码
4. 测试: 单元测试 => 集成测试 => 系统测试 => 验收

## 测试分类

1. 功能测试：检查实际的功能是否符合用户的需求
2. 性能测试：通过自动化的测试工具模拟多种正常、峰值以及异常负载条件来对系统的各项性能指标进行测试
3. 手工测试：指定case, 测试工程师一步一步去测试
4. 自动化测试：把以人为的驱动测试行为转换为机器执行的过程

## 自动化测试有点

1. 程序的回归测试更方便。程序修改比较频繁时，效果是非常明显的。
2. 运行更多繁琐的测试
3. 执行手工测试困难或不可能进行的测试
4. 更好的利用资源
5. 一致性和可重复性及测试用例的复用
6. 增加被测试软件的可靠性

## 适合自动化测试场景

1. 任务测试明确，不会频繁变动
2. 软件需求变更少
3. 项目周期长，测试脚本可以复用

## 常用测试工具

QTP：回归测试和测试同一软件的新版

Robot Framework: python编写的功能自动化测试框架，良好的可扩展性

selenium: 用于Web应用程序测试的工具，支持多平台、多浏览、多语言的去实现自动化测试

## selenium 简介

1. 开源软件
2. 支持主流浏览器：FF,Chrome,IE
3. 跨平台：windows,Linux,Mac
4. 多语言：Java, Python, Ruby，PHP, JS
5. 对 Web 支持良好，丰富简单的 API

## Web 调试工具介绍与开发环境搭建

1. 官网下载 `https://www.python.org/`

2. 安装 pip: `https://pypi.python.org/pypi/pip`;
  解压pip-version目录之后 执行 `python setup.py install`

3. 安装 selenium: `pip install -U selenium`

4. 使用selenium 打开 Firefox 浏览器

``` Python
from selenium from webdriver
borwser = webdriver.Firefox()
borwser.quit()
```

安装了python3，使用pip安装了selenium，但是在使用时，报了“selenium.common.exceptions.WebDriverException: Message: 'geckodriver' executable needs to be in PATH.”

方法二：下载geckodriver.exe

下载地址：`https://github.com/mozilla/geckodriver/releases`，根据自己的电脑，下载的 win64 位的

在firefox的安装目录下，解压 `geckodriver`，然后将该路径添加到path环境变量下，不报这个错了

- Firefox 前段工具
  - firebug 先改为 **Firefox Development Edition**

``` Python
from selenium import webdriver
b = webdriver.Chrome()

```

- 安装 chrome 浏览器 webdriver
1. setup Chrome
2. [chromedriver.exe](http://npm.taobao.org/mirrors/chromedriver/)
3. chromedriver.exe 解压到 `C:\Program Files\Chrome\Application` 目录下
4. 配置环境变量

``` python
# 打开网页
b.get('http://www.baidu.com)
# 输入title
b.title
# 是否包含 百度字符串
'百度' in b.title
# 在浏览器打开地址 http://www.f.cc
b.current_url

```

## 元素定位

- 元素名称      | webdriver API
- id           | find_element_by_id()
- name         | find_element_by_name()
- class name   | find_element_by_class_name()
- tag name     | find_element_by_tag_name() 紧返回第一个标签
- link text    | find_element_by_link_text() 精确定位
- partial link text | find_element_by_partial_link_text() 模糊文本
- xpath        | find_element_by_xpath()
- css selector | find_element_by_css_selector()

- find_element("id|name", "value")

- 元素操作
- clear() 清除元素内容
- send_keys('追加内容') 模拟按键输入
- click() 点击
- submit() 提交表单
- b.back()

## xpath

/xxx 根节点

``` python
ele = b.find_element_by_xpath('/html')
ele.text

ele = b.find_element_by_xpath('/html/body/form/input')
ele.get_attribute('type')
'text'
ele.get_attribute('name')
'firstname'
ele.send_keys("张")

# 第二个输入框(编号1开始)
ele1 = b.find_element_by_xpath('/html/body/form/input[2]')
ele2.get_attribute('name')
'firstname'
```

/xx/yy 根据绝对路径选择元素

//xxx 整个文档扫描，找到所有xx元素

//xx/yy 所有父元素为xx的yy元素

. 选取当前节点的父元素节点

.. 选取父元素地址

//xx[@id] 选取所有元素中有 id 属性的元素

//input[not(@id)]

//xx[@id=yy] 选取所有 xx 元素id 属性为 yy 的元素

//* 所有元素

``` Python
ele3 = b.find_element_by_xpath('//form//input/..')
ele3.tag_name

ele3 = b.find_element_by_xpath('//*')
ele3.tag_name
'html'


ele3 = b.find_element_by_xpath('//*[count(input)=2]')
ele3.tag_name
'form'

```

表达式              结果

//*[count(tag)=2] 统计tag元素个数=2节点

//*[local-name()='xx'] 找到tag伪xxx的元素

//*[starts-with(local-name(),'x')] 找到所有 tag 以 x 开头的元素

//*[contains(local-name(),'x')] 找到所有 tag 包含 x 的元素

//*[string-length(local-name()) = 3] 找到所有 tag 长度为 3 的元素

//xx | /yy 多个路径查找

``` Python
ele = b.find_element_by_xpath('//*[local-name()="input"]')
ele.tag_name

ele.get_attribute('name')
'age'

ele = b.find_element_by_xpath('//input')
ele.get_attribute('name')
'age'

ele = b.find_element_by_xpath('//*[starts-with(local-name(), "i")]')
ele.tag_name
'input'

ele = b.fidn_element_by_xpath('//*[contains(local-name)(, "i")]')
ele.get_attribute('name')
ele.tag_name
'title'

ele = b.find_element_by_xpath('//*[contains(local-name(), "i")][last()]')
ele.tag_name

ele b = b.find_element_by_xpath('//form//*[contains(local-name(), "i")]')
ele.tag_name
'input'
ele.get_attribute('name')
'firstname'

ele b = b.find_element_by_xpath('//form//*[contains(local-name(), "i")][last()]')
ele.tag_name
'input'
ele.get_attribute('name')
'lastname'


ele b = b.find_element_by_xpath('//form//*[contains(local-name(), "i")][last()-1]')

ele = b.find_element_by_xpath('//title | //input[last()]')
ele.tag_name
'title'


```





## webdriver 模块对浏览器进行操作

``` Python
# 窗口全屏
b.maximize_window()
```

## 鼠标事件

from selenium.webdriver.common.action_chains import ActionChains

ActionChains(driver): 用于生成模拟用户行为

perform()： 执行存储行为

expression | introduction

context_click 右击事件

double_click 双击事件

drag_and_drop 拖动

move_to_element() 鼠标停在一个元素上

click_and_hold 按下鼠标左键在一个元素上

``` Python
from selenium.webdriver.common.action_chains import ActionChains
ele = b.find_element_by_link_text('学习列表')
ActionChains(b).move_to_element(ele).perform()
sub_ele = b.find_element_by_link_text('selenium 学习')
sub_ele.click()
```

## 键盘事件

from selenium.webdriver.common.keys import Keys

send_keys(Keys.BACK_SPACE) 退格键

send_keys(Keys.CONTRL, 'a') 全选

send_keys(Keys.CONTRL, 'v') 粘贴

send_keys(Keys.CONTRL, 'c') 复制

send_keys(Keys.CONTRL, 'x') 剪切

send_keys(Keys.ENTER) 回车

``` Python
s.send_keys('python')
from selenium.webdriver.common.keys import Keys
s.clear()
s.send_keys('python1')
s.send_keys(Keys.BACKSPACE) # 去掉1
s.send_kyes(Keys.CONTRL, 'a') # 选择
s.send_kyes(Keys.CONTRL, 'x') # 复制
s.send_kyes(Keys.CONTRL, 'v') # 粘贴

ele = b.find_element_by_link_text('Python Web开发')
ele.text
'Python Web开发'

# 列出所有的句柄
b.window_handles

# 当前句柄
b.current_window_handle

# 切换窗口
b.switch_to_window(d.window.handlers[1])

# 关闭句柄
b.close()

# 退出
b.quit()

```

## 测试脚本中的等待方法

1. 等待是为了使脚本执行更加稳定
2. 常用的休眠方式：time 模块的 sleep 方法

### selenium 模块中的等待方法

- implicitly_wait() 设置 webdriver 等待时间；多个操作
- WebDriverWait 等待条件满足后者超时后退出 `from selenium.webdriver.support.ui import WebDriverWait` 一个操作

``` Python
d.get('http://www.baidu.com')
d.find_element_by_id('kw').send_keys('lingyima')
d.implicitly_wait(5) # 查找5秒钟之后没有找到，则退出
d.find_element_by_id('kw1') # 不存在的元素
```

``` Python
from selenium.webdriver.support.ui import WebDriverWait
help(WebDriverWait)


def get_ele_times(driver, times, func):
  return WebDriverWait(driver, times).until(func)

def main():
  ele_login = get_ele_times(b, 10, 
    lambda b: b.find_element_by_class_name("login"))

  ele_login.click()

# WebDriverWait: poll_frequency->check->until->
```

## Alert()对象

switch_to_alert() 切到 alter，返回一个 alert对象

accept 确认

dismiss 取消

send_keys() 有输入框才能使用，否则报错

``` Python
f.find_element_by_id('alert').click()
alter = b.switch_to_alert()
type(alter)
<class 'selenium.webdriver.common.alert.Alert'>

alter.text

alter.driver

alter.accpet() # 确认弹出框
alter.dismiss() # 取消弹出框

```

## 脚本功能分析与模块化

1. OpenBrowser
2. OpenUrl
3. FindElement
4. SendKeys
5. CheckResult

测试脚本模块化与数据隔离

## 数据设计

### 字典形式

key => value

url 打开地址
text_id 登录元素
userid/pw_id/login_id 登录账号元素
uname/pwd 输入账号信息
error_id 检查错误条件

### web

### 代码测试

### 移动(Android/IOS)

### 数据库

## 自动化工具 selenium

## 自动化测试报告整理

### log

### excel

### mail

## 测试用例
