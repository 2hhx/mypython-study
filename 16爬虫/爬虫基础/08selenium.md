[TOC]



# 一 介绍

```
selenium最初是一个自动化测试工具,而爬虫中使用它主要是为了解决requests无法直接执行JavaScript代码的问题

selenium本质是通过驱动浏览器，完全模拟浏览器的操作，比如跳转、输入、点击、下拉等，来拿到网页渲染之后的结果，可支持多种浏览器

from selenium import webdriver
browser=webdriver.Chrome()
browser=webdriver.Firefox()
browser=webdriver.PhantomJS()
browser=webdriver.Safari()
browser=webdriver.Edge() 
```

[官网：http://selenium-python.readthedocs.io](http://selenium-python.readthedocs.io/)

# 二 安装

## **1、有界面浏览器**

### selenium+chromedriver

```
#安装：selenium+chromedriver
pip3 install selenium
下载chromdriver.exe放到python安装路径的scripts目录中即可，注意最新版本是2.38，并非2.9
国内镜像网站地址：http://npm.taobao.org/mirrors/chromedriver/2.38/
最新的版本去官网找:https://sites.google.com/a/chromium.org/chromedriver/downloads

#验证安装
C:\Users\Administrator>python3
Python 3.6.1 (v3.6.1:69c0db5, Mar 21 2017, 18:41:36) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> from selenium import webdriver
>>> driver=webdriver.Chrome() #弹出浏览器
>>> driver.get('https://www.baidu.com')
>>> driver.page_source

#注意：
selenium3默认支持的webdriver是Firfox，而Firefox需要安装geckodriver
下载链接：https://github.com/mozilla/geckodriver/releases
```



## **2、无界面浏览器**

PhantomJS不再更新

### selenium+phantomjs

```
#安装：selenium+phantomjs
pip3 install selenium
下载phantomjs，解压后把phantomjs.exe所在的bin目录放到环境变量
下载链接：http://phantomjs.org/download.html

#验证安装
C:\Users\Administrator>phantomjs
phantomjs> console.log('egon gaga')
egon gaga
undefined
phantomjs> ^C
C:\Users\Administrator>python3
Python 3.6.1 (v3.6.1:69c0db5, Mar 21 2017, 18:41:36) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> from selenium import webdriver
>>> driver=webdriver.PhantomJS() #无界面浏览器
>>> driver.get('https://www.baidu.com')
>>> driver.page_source
```

## 3 、使用

在 PhantomJS 年久失修, 后继无人的节骨眼 
Chrome 出来救场, 再次成为了反爬虫 Team 的噩梦

自Google 发布 chrome 59 / 60 正式版 开始便支持`Headless mode` 

这意味着在无 GUI 环境下, PhantomJS 不再是唯一选择 

### selenium+谷歌浏览器headless模式

```
#selenium:3.12.0
#webdriver:2.38
#chrome.exe: 65.0.3325.181（正式版本） （32 位）

from selenium import webdriver
from selenium.webdriver.chrome.options import Options
chrome_options = Options()
chrome_options.add_argument('window-size=1920x3000') #指定浏览器分辨率
chrome_options.add_argument('--disable-gpu') #谷歌文档提到需要加上这个属性来规避bug
chrome_options.add_argument('--hide-scrollbars') #隐藏滚动条, 应对一些特殊页面
chrome_options.add_argument('blink-settings=imagesEnabled=false') #不加载图片, 提升速度
chrome_options.add_argument('--headless') #浏览器不提供可视化页面. linux下如果系统不支持可视化不加这条会启动失败
chrome_options.binary_location = r"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" #手动指定使用的浏览器位置


driver=webdriver.Chrome(chrome_options=chrome_options)
driver.get('https://www.baidu.com')

print('hao123' in driver.page_source)

driver.close() #切记关闭浏览器，回收资源
```

1. selenium的安装非常简单，和其他的Python 库一样，我们可以用pip 安装。

2. ```python
   pip install selenium
   ```

3. selenium的脚本可以控制浏览器进行操作，可以实现多个浏览器的调用，包括IE（7, 8, 9, 10, 11），Firefox，Safari，Google Chrome，Opera等。最常用的是 Chrome，因此下面的讲解也以chrome为例，在执行之前你需要安装chrome浏览器。

4. ```python
   from selenium import webdriver
   driver = webdriver.Chrome("F:\python_老男孩学习\实战项目\爬虫\chromedriver.exe")#这里是写chrome的地址
   driver.get("http://www.santostang.com/2018/07/04/hello-world/")
   ```

### selenium选择元素的方法

* find_element_by_id：通过元素的id选择，例如:driver.find_element_by_id(‘loginForm’)
* find_element_by_name：通过元素的name选择，driver.find_element_by_name(‘password’)
* find_element_by_xpath：通过xpath选择，driver.find_element_by_xpath(“//form[1]”)
* find_element_by_link_text：通过链接地址选择
* find_element_by_partial_link_text：通过链接的部分地址选择
* find_element_by_tag_name：通过元素的名称选择
* find_element_by_class_name：通过元素的id选择
* find_element_by_css_selector：通过css选择器选择

有时候，我们需要查找多个元素。在上述例子中，我们就查找了所有的评论。因此，也有对应的元素选择方法，就是在上述的element后加上s，变成elements。

– find_elements_by_name
– find_elements_by_xpath
– find_elements_by_link_text
– find_elements_by_partial_link_text
– find_elements_by_tag_name
– find_elements_by_class_name
– find_elements_by_css_selector

其中xpath和css_selector是比较好的方法，一方面比较清晰，另一方面相对其他方法定位元素比较准确。
除此之外，我们还可以使用selenium操作元素方法实现自动操作网页。常见的操作元素方法如下：
– clear 清除元素的内容
– send_keys 模拟按键输入
– click 点击元素
– submit 提交表单

# 三 基本使用

```
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By #按照什么方式查找，By.ID,By.CSS_SELECTOR
from selenium.webdriver.common.keys import Keys #键盘按键操作
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait #等待页面加载某些元素

browser=webdriver.Chrome()
try:
    browser.get('https://www.baidu.com')

​```
input_tag=browser.find_element_by_id('kw')
input_tag.send_keys('美女') #python2中输入中文错误，字符串前加个u
input_tag.send_keys(Keys.ENTER) #输入回车
​```


    wait=WebDriverWait(browser,10)
    wait.until(EC.presence_of_element_located((By.ID,'content_left'))) #等到id为content_left的元素加载完毕,最多等10秒

​```
print(browser.page_source)
print(browser.current_url)
print(browser.get_cookies())
​```

finally:
    browser.close()
```

# 四 选择器

## **一 基本用法**

```
#官网链接：http://selenium-python.readthedocs.io/locating-elements.html
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By #按照什么方式查找，By.ID,By.CSS_SELECTOR
from selenium.webdriver.common.keys import Keys #键盘按键操作
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait #等待页面加载某些元素
import time

driver=webdriver.Chrome()
driver.get('https://www.baidu.com')
wait=WebDriverWait(driver,10)

try:
    #===============所有方法===================
    # 1、find_element_by_id
    # 2、find_element_by_link_text
    # 3、find_element_by_partial_link_text
    # 4、find_element_by_tag_name
    # 5、find_element_by_class_name
    # 6、find_element_by_name
    # 7、find_element_by_css_selector
    # 8、find_element_by_xpath
    # 强调：
    # 1、上述均可以改写成find_element(By.ID,'kw')的形式
    # 2、find_elements_by_xxx的形式是查找到多个元素，结果为列表

​```
#===============示范用法===================
# 1、find_element_by_id
print(driver.find_element_by_id('kw'))

# 2、find_element_by_link_text
# login=driver.find_element_by_link_text('登录')
# login.click()

# 3、find_element_by_partial_link_text
login=driver.find_elements_by_partial_link_text('录')[0]
login.click()

# 4、find_element_by_tag_name
print(driver.find_element_by_tag_name('a'))

# 5、find_element_by_class_name
button=wait.until(EC.element_to_be_clickable((By.CLASS_NAME,'tang-pass-footerBarULogin')))
button.click()

# 6、find_element_by_name
input_user=wait.until(EC.presence_of_element_located((By.NAME,'userName')))
input_pwd=wait.until(EC.presence_of_element_located((By.NAME,'password')))
commit=wait.until(EC.element_to_be_clickable((By.ID,'TANGRAM__PSP_10__submit')))

input_user.send_keys('18611453110')
input_pwd.send_keys('xxxxxx')
commit.click()

# 7、find_element_by_css_selector
driver.find_element_by_css_selector('#kw')

# 8、find_element_by_xpath
​```


    time.sleep(5)

finally:
    driver.close()
```



## **二 xpath**

```
#官网链接：http://selenium-python.readthedocs.io/locating-elements.html
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By #按照什么方式查找，By.ID,By.CSS_SELECTOR
from selenium.webdriver.common.keys import Keys #键盘按键操作
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait #等待页面加载某些元素
import time

driver=webdriver.PhantomJS()
driver.get('https://doc.scrapy.org/en/latest/_static/selectors-sample1.html')
# wait=WebDriverWait(driver,3)
driver.implicitly_wait(3) #使用隐式等待

try:
    # find_element_by_xpath
    #//与/
    # driver.find_element_by_xpath('//body/a')  # 开头的//代表从整篇文档中寻找,body之后的/代表body的儿子，这一行找不到就会报错了

    driver.find_element_by_xpath('//body//a')  # 开头的//代表从整篇文档中寻找,body之后的//代表body的子子孙孙
    driver.find_element_by_css_selector('body a')

​```
#取第n个
res1=driver.find_elements_by_xpath('//body//a[1]') #取第一个a标签
print(res1[0].text)

#按照属性查找,下述三者查找效果一样
res1=driver.find_element_by_xpath('//a[5]')
res2=driver.find_element_by_xpath('//a[@href="image5.html"]')
res3=driver.find_element_by_xpath('//a[contains(@href,"image5")]') #模糊查找
print('==>', res1.text)
print('==>',res2.text)
print('==>',res3.text)
​```

​```
#其他
res1=driver.find_element_by_xpath('/html/body/div/a')
print(res1.text)

res2=driver.find_element_by_xpath('//a[img/@src="image3_thumb.jpg"]') #找到子标签img的src属性为image3_thumb.jpg的a标签
print(res2.tag_name,res2.text)

res3 = driver.find_element_by_xpath("//input[@name='continue'][@type='button']") #查看属性name为continue且属性type为button的input标签
res4 = driver.find_element_by_xpath("//*[@name='continue'][@type='button']") #查看属性name为continue且属性type为button的所有标签
​```




​```
time.sleep(5)
​```

finally:
    driver.close()
```

### 详解

 

```
doc='''
<html>
 <head>
  <base href='http://example.com/' />
  <title>Example website</title>
 </head>
 <body>
  <div id='images'>
   <a href='image1.html'>Name: My image 1 <br /><img src='image1_thumb.jpg' /></a>
   <a href='image2.html'>Name: My image 2 <br /><img src='image2_thumb.jpg' /></a>
   <a href='image3.html'>Name: My image 3 <br /><img src='image3_thumb.jpg' /></a>
   <a href='image4.html'>Name: My image 4 <br /><img src='image4_thumb.jpg' /></a>
   <a href='image5.html' class='li li-item' name='items'>Name: My image 5 <br /><img src='image5_thumb.jpg' /></a>
   <a href='image6.html' name='items'><span><h5>test</h5></span>Name: My image 6 <br /><img src='image6_thumb.jpg' /></a>
  </div>
 </body>
</html>
'''
from lxml import etree

html=etree.HTML(doc)
# html=etree.parse('search.html',etree.HTMLParser())
# 1 所有节点
# a=html.xpath('//*')
# 2 指定节点（结果为列表）
# a=html.xpath('//head')
# 3 子节点，子孙节点
# a=html.xpath('//div/a')
# a=html.xpath('//body/a') #无数据
# a=html.xpath('//body//a')
# 4 父节点
# a=html.xpath('//body//a[@href="image1.html"]/..')
# a=html.xpath('//body//a[1]/..')
# 也可以这样
# a=html.xpath('//body//a[1]/parent::*')
# 5 属性匹配
# a=html.xpath('//body//a[@href="image1.html"]')

# 6 文本获取
# a=html.xpath('//body//a[@href="image1.html"]/text()')

# 7 属性获取
# a=html.xpath('//body//a/@href')
# # 注意从1 开始取（不是从0）
# a=html.xpath('//body//a[1]/@href')
# 8 属性多值匹配
#  a 标签有多个class类，直接匹配就不可以了，需要用contains
# a=html.xpath('//body//a[@class="li"]')
# a=html.xpath('//body//a[contains(@class,"li")]')
# a=html.xpath('//body//a[contains(@class,"li")]/text()')
# 9 多属性匹配
# a=html.xpath('//body//a[contains(@class,"li") or @name="items"]')
# a=html.xpath('//body//a[contains(@class,"li") and @name="items"]/text()')
# # a=html.xpath('//body//a[contains(@class,"li")]/text()')
# 10 按序选择
# a=html.xpath('//a[2]/text()')
# a=html.xpath('//a[2]/@href')
# 取最后一个
# a=html.xpath('//a[last()]/@href')
# 位置小于3的
# a=html.xpath('//a[position()<3]/@href')
# 倒数第二个
# a=html.xpath('//a[last()-2]/@href')
# 11 节点轴选择
# ancestor：祖先节点
# 使用了* 获取所有祖先节点
# a=html.xpath('//a/ancestor::*')
# # 获取祖先节点中的div
# a=html.xpath('//a/ancestor::div')
# attribute：属性值
# a=html.xpath('//a[1]/attribute::*')
# child：直接子节点
# a=html.xpath('//a[1]/child::*')
# descendant：所有子孙节点
# a=html.xpath('//a[6]/descendant::*')
# following:当前节点之后所有节点
# a=html.xpath('//a[1]/following::*')
# a=html.xpath('//a[1]/following::*[1]/@href')
# following-sibling:当前节点之后同级节点
# a=html.xpath('//a[1]/following-sibling::*')
# a=html.xpath('//a[1]/following-sibling::a')
# a=html.xpath('//a[1]/following-sibling::*[2]')
# a=html.xpath('//a[1]/following-sibling::*[2]/@href')

# print(a)
```



## **三 获取标签属性**

```
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By #按照什么方式查找，By.ID,By.CSS_SELECTOR
from selenium.webdriver.common.keys import Keys #键盘按键操作
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait #等待页面加载某些元素

browser=webdriver.Chrome()

browser.get('https://www.amazon.cn/')

wait=WebDriverWait(browser,10)
wait.until(EC.presence_of_element_located((By.ID,'cc-lm-tcgShowImgContainer')))

tag=browser.find_element(By.CSS_SELECTOR,'#cc-lm-tcgShowImgContainer img')

#获取标签属性，
print(tag.get_attribute('src'))

#获取标签ID，位置，名称，大小（了解）
print(tag.id)
print(tag.location)
print(tag.tag_name)
print(tag.size)

browser.close()
```



# 五 等待元素被加载

## 隐式等待

```
#1、selenium只是模拟浏览器的行为，而浏览器解析页面是需要时间的（执行css，js），一些元素可能需要过一段时间才能加载出来，为了保证能查找到元素，必须等待

#2、等待的方式分两种：
隐式等待：在browser.get（'xxx'）前就设置，针对所有元素有效
显式等待：在browser.get（'xxx'）之后设置，只针对某个元素有效
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By #按照什么方式查找，By.ID,By.CSS_SELECTOR
from selenium.webdriver.common.keys import Keys #键盘按键操作
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait #等待页面加载某些元素

browser=webdriver.Chrome()

#隐式等待:在查找所有元素时，如果尚未被加载，则等10秒
browser.implicitly_wait(10)

browser.get('https://www.baidu.com')

input_tag=browser.find_element_by_id('kw')
input_tag.send_keys('美女')
input_tag.send_keys(Keys.ENTER)

contents=browser.find_element_by_id('content_left') #没有等待环节而直接查找，找不到则会报错
print(contents)

browser.close()
```

## 显式等待

```
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By #按照什么方式查找，By.ID,By.CSS_SELECTOR
from selenium.webdriver.common.keys import Keys #键盘按键操作
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait #等待页面加载某些元素

browser=webdriver.Chrome()
browser.get('https://www.baidu.com')



input_tag=browser.find_element_by_id('kw')
input_tag.send_keys('美女')
input_tag.send_keys(Keys.ENTER)

#显式等待：显式地等待某个元素被加载
wait=WebDriverWait(browser,10)
wait.until(EC.presence_of_element_located((By.ID,'content_left')))

contents=browser.find_element(By.CSS_SELECTOR,'#content_left')
print(contents)

browser.close()
```



# 六 元素交互操作

## 点击，清空

```
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By #按照什么方式查找，By.ID,By.CSS_SELECTOR
from selenium.webdriver.common.keys import Keys #键盘按键操作
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait #等待页面加载某些元素

browser=webdriver.Chrome()
browser.get('https://www.amazon.cn/')
wait=WebDriverWait(browser,10)



input_tag=wait.until(EC.presence_of_element_located((By.ID,'twotabsearchtextbox')))
input_tag.send_keys('iphone 8')
button=browser.find_element_by_css_selector('#nav-search > form > div.nav-right > div > input')
button.click()

import time
time.sleep(3)

input_tag=browser.find_element_by_id('twotabsearchtextbox')
input_tag.clear() #清空输入框
input_tag.send_keys('iphone7plus')
button=browser.find_element_by_css_selector('#nav-search > form > div.nav-right > div > input')
button.click()



# browser.close()
```

## Action Chains

```
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By  # 按照什么方式查找，By.ID,By.CSS_SELECTOR
from selenium.webdriver.common.keys import Keys  # 键盘按键操作
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait  # 等待页面加载某些元素
import time

driver = webdriver.Chrome()
driver.get('http://www.runoob.com/try/try.php?filename=jqueryui-api-droppable')
wait=WebDriverWait(driver,3)
# driver.implicitly_wait(3)  # 使用隐式等待

try:
    driver.switch_to.frame('iframeResult') ##切换到iframeResult
    sourse=driver.find_element_by_id('draggable')
    target=driver.find_element_by_id('droppable')

​```
#方式一：基于同一个动作链串行执行
# actions=ActionChains(driver) #拿到动作链对象
# actions.drag_and_drop(sourse,target) #把动作放到动作链中，准备串行执行
# actions.perform()

#方式二：不同的动作链，每次移动的位移都不同
​```

    ActionChains(driver).click_and_hold(sourse).perform()
    distance=target.location['x']-sourse.location['x']

​```
track=0
while track < distance:
    ActionChains(driver).move_by_offset(xoffset=2,yoffset=0).perform()
    track+=2

ActionChains(driver).release().perform()

time.sleep(10)
​```

finally:
    driver.close()
```

## 在交互动作比较难实现的时候可以自己写JS（万能方法）

```
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By #按照什么方式查找，By.ID,By.CSS_SELECTOR
from selenium.webdriver.common.keys import Keys #键盘按键操作
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait #等待页面加载某些元素



try:
    browser=webdriver.Chrome()
    browser.get('https://www.baidu.com')
    browser.execute_script('alert("hello world")') #打印警告
finally:
    browser.close()
```

## 补充:frame的切换

```
#frame相当于一个单独的网页，在父frame里是无法直接查看到子frame的元素的，必须switch_to_frame切到该frame下，才能进一步查找

from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By #按照什么方式查找，By.ID,By.CSS_SELECTOR
from selenium.webdriver.common.keys import Keys #键盘按键操作
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait #等待页面加载某些元素

try:
    browser=webdriver.Chrome()
    browser.get('http://www.runoob.com/try/try.php?filename=jqueryui-api-droppable')

​```
browser.switch_to.frame('iframeResult') #切换到id为iframeResult的frame

​```


    tag1=browser.find_element_by_id('droppable')
    print(tag1)

​```
# tag2=browser.find_element_by_id('textareaCode') #报错，在子frame里无法查看到父frame的元素
browser.switch_to.parent_frame() #切回父frame,就可以查找到了
tag2=browser.find_element_by_id('textareaCode')
print(tag2)

​```

finally:
    browser.close()
```



# 七 其他

## 模拟浏览器的前进后退

```
#模拟浏览器的前进后退
import time
from selenium import webdriver

browser=webdriver.Chrome()
browser.get('https://www.baidu.com')
browser.get('https://www.taobao.com')
browser.get('http://www.sina.com.cn/')

browser.back()
time.sleep(10)
browser.forward()
browser.close()
```

## cookies

```
#cookies
from selenium import webdriver

browser=webdriver.Chrome()
browser.get('https://www.zhihu.com/explore')
print(browser.get_cookies())
browser.add_cookie({'k1':'xxx','k2':'yyy'})
print(browser.get_cookies())

# browser.delete_all_cookies()
```

## 选项卡管理

```
#选项卡管理：切换选项卡，有js的方式windows.open,有windows快捷键：ctrl+t等，最通用的就是js的方式
import time
from selenium import webdriver

browser=webdriver.Chrome()
browser.get('https://www.baidu.com')
browser.execute_script('window.open()')

print(browser.window_handles) #获取所有的选项卡
browser.switch_to_window(browser.window_handles[1])
browser.get('https://www.taobao.com')
time.sleep(10)
browser.switch_to_window(browser.window_handles[0])
browser.get('https://www.sina.com.cn')
browser.close()
```

## 异常处理

```
from selenium import webdriver
from selenium.common.exceptions import TimeoutException,NoSuchElementException,NoSuchFrameException

try:
    browser=webdriver.Chrome()
    browser.get('http://www.runoob.com/try/try.php?filename=jqueryui-api-droppable')
    browser.switch_to.frame('iframssseResult')

except TimeoutException as e:
    print(e)
except NoSuchFrameException as e:
    print(e)
finally:
    browser.close()
```



# 八 项目练习

## 自动登录163邮箱并发送邮件

```
#注意：网站都策略都是在不断变化的，精髓在于学习流程。下述代码生效与2017-11-7，不能保证永久有效
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait

browser=webdriver.Chrome()

try:
    browser.get('http://mail.163.com/')

​```
wait=WebDriverWait(browser,5)

frame=wait.until(EC.presence_of_element_located((By.ID,'x-URS-iframe')))
browser.switch_to.frame(frame)

wait.until(EC.presence_of_element_located((By.CSS_SELECTOR,'.m-container')))

inp_user=browser.find_element_by_name('email')
inp_pwd=browser.find_element_by_name('password')
button=browser.find_element_by_id('dologin')
inp_user.send_keys('18611453110')
inp_pwd.send_keys('xxxx')
button.click()

#如果遇到验证码，可以把下面一小段打开注释
# import time
# time.sleep(10)
# button = browser.find_element_by_id('dologin')
# button.click()
​```


    wait.until(EC.presence_of_element_located((By.ID,'dvNavTop')))
    write_msg=browser.find_elements_by_css_selector('#dvNavTop li')[1] #获取第二个li标签就是“写信”了
    write_msg.click()

​```
wait.until(EC.presence_of_element_located((By.CLASS_NAME,'tH0')))
recv_man=browser.find_element_by_class_name('nui-editableAddr-ipt')
title=browser.find_element_by_css_selector('.dG0 .nui-ipt-input')
recv_man.send_keys('378533872@qq.com')
title.send_keys('圣旨')
print(title.tag_name)

​```

​```
frame=wait.until(EC.presence_of_element_located((By.CLASS_NAME,'APP-editor-iframe')))
browser.switch_to.frame(frame)
body=browser.find_element(By.CSS_SELECTOR,'body')
body.send_keys('egon很帅，可以加工资了')

browser.switch_to.parent_frame() #切回他爹
send_button=browser.find_element_by_class_name('nui-toolbar-item')
send_button.click()

#可以睡时间久一点别让浏览器关掉，看看发送成功没有
import time
time.sleep(10000)

​```

except Exception as e:
    print(e)
finally:
    browser.close()
```

## 爬取京东商城商品信息

```
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By #按照什么方式查找，By.ID,By.CSS_SELECTOR
from selenium.webdriver.common.keys import Keys #键盘按键操作
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait #等待页面加载某些元素
import time



def get_goods(driver):
    try:
        goods=driver.find_elements_by_class_name('gl-item')

​```
    for good in goods:
        detail_url=good.find_element_by_tag_name('a').get_attribute('href')

        p_name=good.find_element_by_css_selector('.p-name em').text.replace('\n','')
        price=good.find_element_by_css_selector('.p-price i').text
        p_commit=good.find_element_by_css_selector('.p-commit a').text

        msg = '''
        商品 : %s
        链接 : %s
        价钱 ：%s
        评论 ：%s
        ''' % (p_name,detail_url,price,p_commit)

        print(msg,end='\n\n')

​```

​```
    button=driver.find_element_by_partial_link_text('下一页')
    button.click()
    time.sleep(1)
    get_goods(driver)
except Exception:
    pass

​```

def spider(url,keyword):
    driver = webdriver.Chrome()
    driver.get(url)
    driver.implicitly_wait(3)  # 使用隐式等待
    try:
        input_tag=driver.find_element_by_id('key')
        input_tag.send_keys(keyword)
        input_tag.send_keys(Keys.ENTER)
        get_goods(driver)
    finally:
        driver.close()

if __name__ == '__main__':
    spider('https://www.jd.com/',keyword='iPhone8手机')
```

 

[TOC]

# 实现动态爬虫爬取数据

谷歌浏览器驱动下载地址：

```
http://chromedriver.storage.googleapis.com/index.html
```

火狐浏览器驱动下载地址：

```
https://github.com/mozilla/geckodriver/releases/
```



## 对登陆的处理

```python
user = driver.find_element_by_name("username")  #找到用户名输入框
user.clear  #清除用户名输入框内容
user.send_keys("1234567")  #在框中输入用户名
pwd = driver.find_element_by_name("password")  #找到密码输入框
pwd.clear  #清除密码输入框内容
pwd.send_keys("******")    #在框中输入密码
driver.find_element_by_id("loginBtn").click()  #点击登录
```

上述代码中，是一个自动登录程序中截取的一部分。从代码中可以看到，我们可以用selenium操作元素的方法，对浏览器中的网页进行各种操作，包括登录。
除此之外，selenium除了鼠标简单的操作，还可以实现复杂的双击，拖拽等操作。此外，它还可以获得网页中各个元素的大小，甚至还可以进行模拟键盘的操作。由于篇幅有限，有兴趣的读者，可以到selenium的官方文档查看：http://selenium-python.readthedocs.io/index.html





# 举例

## 2.1获取一个界面的信息

```python
"""
在上述的例子中，使用Chrome“检查”功能找到源地址还十分容易。
但是有一些网站非常复杂，例如前面的天猫产品评论，使用“检查”功能很难找到调用的网页地址。
除此之外，有一些数据真实地址的URL也十分冗长和复杂，有些网站为了规避这些抓取会对地址进行加密，造成其中的一些变量让人摸不着头脑。
因此，这里介绍第二种方法，使用浏览器渲染引擎。
直接用浏览器在显示网页时解析HTML，应用CSS样式并执行JavaScript的语句。
这方法在爬虫过程中会打开一个浏览器，加载该网页，自动操作浏览器浏览各个网页，顺便把数据抓下来。
用一句简单而通俗的话说，使用浏览器渲染方法，爬取动态网页变成了爬取静态网页。
我们可以用Python的selenium库模拟浏览器完成抓取。Selenium是一个用于Web应用程序测试的工具。
Selenium测试直接运行在浏览器中，浏览器自动按照脚本代码做出点击，输入，打开，验证等操作，就像真正的用户在操作一样。
"""
from selenium import webdriver
driver = webdriver.Chrome("F:\python_老男孩学习\实战项目\爬虫\chromedriver.exe")
driver.get("http://www.santostang.com/2018/07/04/hello-world/")
"""
下面代码的意思：
driver.find_element_by_css_selector是用CSS选择器查找元素，
找到class为’reply-content’的div元素；
find_element_by_tag_name则是通过元素的tag去寻找，
意思是找到comment中的p元素。最后，再输出p元素中的text文本
"""
# comment = driver.find_element_by_css_selector('div.reply-content')
# comment = comment.find_element_by_tag_name('p')
# print(comment.text)
"""
原来的js,没有解析出来，需要对js进行解析
"""

print(driver.page_source)
print("###########################")
driver.switch_to.frame(driver.find_element_by_css_selector("iframe[title='livere']"))
comment = driver.find_element_by_css_selector('div.reply-content')
comment = comment.find_element_by_tag_name('p')
print(comment.text)

```

## 2.2改进版（获取多个界面的信息）

```python
from selenium import webdriver
import time
driver = webdriver.Chrome("F:\python_老男孩学习\实战项目\爬虫\chromedriver.exe")
driver.implicitly_wait(20) # 隐性等待，最长等20秒
#把上述地址改成你电脑中geckodriver.exe程序的地址
driver.get("http://www.santostang.com/2018/07/04/hello-world/")
time.sleep(5)
for i in range(0,3):
    # 下滑到页面底部
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
    # 转换iframe，再找到查看更多，点击
    driver.switch_to.frame(driver.find_element_by_css_selector("iframe[title='livere']"))
    load_more = driver.find_element_by_css_selector('button.more-btn')
    load_more.click()
    # 把iframe又转回去
    driver.switch_to.default_content()
    time.sleep(2)
    driver.switch_to.frame(driver.find_element_by_css_selector("iframe[title='livere']"))
    comments = driver.find_elements_by_css_selector('div.reply-content')
    for eachcomment in comments:
        content = eachcomment.find_element_by_tag_name('p')
        print (content.text)
```

# 三，selenium的高级操作

## Selenium的高级操作

使用Selenium和使用浏览器“检查”方法爬取动态网页相比，Selenium因为要把整个网页加载出来，再开始爬取内容，速度往往较慢。
因此在实际使用当中，如果使用浏览器“检查”功能进行网页的逆向工程不复杂的话，最好使用浏览器“检查”功能方法。不过，也有一些方法可以用Selenium控制浏览器的加载的内容，从而加快Selenium的爬取速度。常用的方法有：

1. 控制CSS的加载

2. 控制图片文件的显示

3. 控制JavaScript的执行

   

4. 控制CSS。因为抓取过程中仅仅抓取页面的内容，CSS样式文件是用来控制页面的外观和元素放置位置的，对内容并没有影响，所以我们可以限制网页加载CSS，从而减少抓取时间。其代码如下：

```python
# 控制 css
from selenium import webdriver
fp = webdriver.FirefoxProfile()
fp.set_preference("permissions.default.stylesheet",2)
driver = webdriver.Firefox(firefox_profile=fp, executable_path = r'C:\Users\santostang\Desktop\geckodriver.exe')
#把上述地址改成你电脑中geckodriver.exe程序的地址
driver.get("http://www.santostang.com/2018/07/04/hello-world/")
```

在上述代码中，控制css的加载主要用fp = webdriver.FirefoxProfile()这个功能。设定不加载css，使用fp.set_preference(“permissions.default.stylesheet”,2)。之后使用webdriver.Firefox(firefox_profile=fp)就可以控制不加载css了。

2. 限制图片的加载。如果不需要抓取网页上的图片，最好可以禁止图片加载，限制图片的加载可以帮助我们极大地提高网络爬虫的效率。因为网页上的图片往往较多，而且图片文件相对于文字、CSS、JavaScript等其他文件都比较大，所以加载需要较长时间。

```python
# 限制图片的加载
from selenium import webdriver
fp = webdriver.FirefoxProfile()
fp.set_preference("permissions.default.image",2)
driver = webdriver.Firefox(firefox_profile=fp, executable_path = r'C:\Users\santostang\Desktop\geckodriver.exe')
#把上述地址改成你电脑中geckodriver.exe程序的地址
driver.get("http://www.santostang.com/2018/07/04/hello-world/")
```

与限制css类似，限制图片的加载可以用fp.set_preference(“permissions. default.image”,2)

3. 控制JavaScript的运行。如果需要抓取的内容不是通过JavaScript动态加载得到的，我们可以通过禁止JavaScript的执行来提高抓取的效率。因为大多数网页都会利用JavaScript异步加载很多内容，这些内容不仅是我们不需要的，它们的加载还浪费了时间。

```python
# 限制 JavaScript 的执行
from selenium import webdriver
fp = webdriver.FirefoxProfile()
fp.set_preference("javascript.enabled", False)
driver = webdriver.Firefox(firefox_profile=fp, executable_path = r'C:\Users\santostang\Desktop\geckodriver.exe')
#把上述地址改成你电脑中geckodriver.exe程序的地址
driver.get("http://www.santostang.com/2018/07/04/hello-world/")
```



![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190905170352354-364495707.jpg)