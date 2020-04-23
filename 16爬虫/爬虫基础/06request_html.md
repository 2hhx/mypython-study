# 牛逼的requests-html

​	**安装：** pip install requests-html

我们可以在安装的时候看到他安装了lxml,reuqests,bs4......我们常用的解析和爬取的库都分装在他里面

Python上有一个非常著名的HTTP库——[requests](http://cn.python-requests.org/zh_CN/latest/)，相信大家都听说过，用过的人都说非常爽！现在requests库的作者又发布了一个新库，叫做[requests-html](https://html.python-requests.org/)，看名字也能猜出来，这是一个解析HTML的库，具备requests的功能以外，还新增了一些更加强大的功能，用起来比requests更爽！需要注意一点就是，requests-html只支持Python 3.6或以上的版本，所以使用老版本的Python的同学需要更新一下Python版本了。

优点：

- 支持JavaScript
- 支持CSS选择器（又名jQuery风格, 感谢PyQuery）
- 支持Xpath选择器
- 可自定义模拟User-Agent（模拟得更像真正的web浏览器）
- 自动追踪重定向
- 连接池与cookie持久化
- 支持异步请求

## **使用：**

### **请求：**

```
from requests_html import HTMLSession
session = HTMLSession()
**参数：**
browser.args = [
    '--no-sand',
    '--user-agent=XXXXX'
]
响应对象 = session.request（......）
响应对象 = session.get（......）
响应对象 = session.post（......）
```

​			**参数和requests模块一毛一样**

```
from requests_html import HTMLSession
session = HTMLSession()

#用法和requests.session实例化的对象用法一模一样,也会自动保存返回信息
#相比reuqests,他多了对于response.html这个属性

```

注意点`:发默认发送的的是`无头浏览器`,且他如果用render`调用浏览器内核

### 解决无头浏览器(针对反爬,如果没有做反爬无所谓)

修改源码

- ctrl左键进入`HTMLSession`

- 我们可以看到他是继承`BaseSession`

- ctrl左键进入`BaseSession`

  `原来的源码`

  ```
  class BaseSession(requests.Session):
      def __init__(self, mock_browser : bool = True, verify : bool = True,
                   browser_args : list = ['--no-sandbox']):
          super().__init__()
          if mock_browser:
          self.headers['User-Agent'] = user_agent()
  
          self.hooks['response'].append(self.response_hook)
          self.verify = verify
  
          self.__browser_args = browser_args
          self.__headless = headless
  
        #中间没用的省略掉不是删掉
      @property
      async def browser(self):
          if not hasattr(self, "_browser"):
              self._browser = await pyppeteer.launch(ignoreHTTPSErrors=not(self.verify), headless=True, args=self.__browser_args)
  
          return self._browser
  ```

  `修改后的源码`

  ```
  class BaseSession(requests.Session):
      """ A consumable session, for cookie persistence and connection pooling,
      amongst other things.
      """
  
      def __init__(self, mock_browser : bool = True, verify : bool = True,
                   browser_args : list = ['--no-sandbox'],headless=False):       #如果你设置成True他就是无头,且你再运行render时候不会弹出浏览器
          super().__init__()
  
          # Mock a web browser's user agent.
          if mock_browser:
              self.headers['User-Agent'] = user_agent()
  
          self.hooks['response'].append(self.response_hook)
          self.verify = verify
  
          self.__browser_args = browser_args
          self.__headless = headless
            #中间没用的省略掉不是删掉
      @property
      async def browser(self):
          if not hasattr(self, "_browser"):
              self._browser = await pyppeteer.launch(ignoreHTTPSErrors=not(self.verify), headless=self.__headless, args=self.__browser_args)
  
          return self._browser
  ```

  `其实我就做了个处理方便传一个headless进去`

对于`session`重新设置

```
from requests_html import HTMLSession
session = HTMLSession(
browser_args=['--no-sand',
              '--user-agent='xxxxx'
             ],
             #headless=False  无头
              #headless=True  有头
             
)
#这样你就可以直接定义他是什么浏览器发送请求啦
```

### **响应：**

```
r.url
**属性和requests模块一毛一样
```

# **解析：**

## **html对象属性：**

```
r.html.absolute_links          /xx/yy   -->   把网站上的路径全部转换成绝对路径进行返回 http://www....../xx/yy
r.html.links                 路径原样（路径原样返回）
r.html.base_url          网站基础路径,.base标签里的路径,如果没有base标签,就是当前url
r.html.html              解码过的响应内容   #返回字符串字符串内包含有标签,相当于reqyests里面的r.html 
r.html.text 返回页面所有标签内的文本返回字符串字符串内不包含有标签爬取什么小说新闻之类的超级好用!
r.html.encoding = 'gbk'     控制的是r.html.html的解码格式  
r.html.raw_html            相当于r.content  返回二进制
r.html.pq					拿到pyquQuery对象
r.html.pq
```

```

# from requests_html import HTMLSession
# # 获取sessions的对象
# session = HTMLSession()
# # 往新浪新闻主页发送get请求
# sina = session.get('https://news.sina.com.cn/')
# print(sina.status_code)#打印状态码
# print(sina.raise_for_status())
# sina.encoding='utf-8'#修改编码
# print(sina.encoding)#打印编码格式
# print(sina.text)#打印文本
```



### r.html.absolute_links 和r.html.links 与r.html.base_url 

```
from requests_html import HTMLSession
# 获取请求对象
session = HTMLSession()

# 往京东主页发送get请求
jd = session.get('https://jd.com/')

# 得到京东主页所有的链接，返回的是一个set集合
print(jd.html.links)
print('*' * 1000)

# 若获取的链接中有相对路径，我们还可以通过absolute_links获取所有绝对链接
print(jd.html.absolute_links)
print(len(jd.html.links),len(jd.html.absolute_links))
'''
'//channel.jd.com/1315-1342.html'
'https://channel.jd.com/1315-1342.html'
'''
print(jd.html.base_url)#https://www.jd.com/
```



### r.html.pq

```
from requests_html import HTML,HTMLSession
# session = HTMLSession()
# # 返回一个响应
# r = session.get(url='http://www.baidu.com')
# #<class 'requests_html.HTML'>
# print(type(r.html))
# # <class 'pyquery.pyquery.PyQuery'>
# print(type(r.html.pq))
# # print(r.html.pq)
```

## **html对象方法：**

[xpath解析学习https://www.jianshu.com/p/85a3004b5c06](https://www.jianshu.com/p/85a3004b5c06)

```
r.html.find('css选择器')              [element对象,element对象...]

r.html.find('css选择器'，first = True)     对一个element对象
r.html.xpath(‘xpath选择器’)

r.html.xpath('‘xpath选择器'，first = True)
					
对应re正则模块
r.html.search(‘模板’)                  result对象(匹配第一次)
    （‘xxx{}yyy{}’）[0]
    （‘xxx{name}yyy{pwd}’）[‘name’]

r.html.search_all('模板')             匹配所有,[result对象,result对象,....]
r.html.render(.....)               渲染后的结果去替换 r.html.html
	**参数：**

script：“”“ ( ) => {
    js代码
    js代码
    }
 ”“”

            scrolldown：n  控制页码

            sleep:n    控制休息

            lkeep_page:True/False


绕过网站对webdriver的检测:
							'''
							() =>{
                                Object.defineProperties(navigator,{
                                webdriver:{
                                    get: () => undefined
                                    }
                                })
                            }
                            '''
```

```
# 1.通过css选择器选取一个Element对象
# 获取id为content-left的div标签，并且返回一个对象
content = obj.html.find('div#content-left', first=True)

# 2.获取一个Element对象内的文本内容
# 获取content内所有文本
print(content.text)

# 3.获取一个Element对象的所有attributes
# 获取content内所有属性
print(content.attrs)

# 4.渲染出一个Element对象的完整的HTML内容
html = content.html
print(html)

# 5.获取Element对象内的指定的所有子Element对象，返回列表
a_s = content.find('a')
print(a_s)
print(len(a_s))  # 79

# 循环所有的a标签
for a in a_s:
    # 获取a标签内所有属性的href属性 并拼接
    href = a.attrs['href']
    if href.startswith('/'):
        url = 'https://www.qiushibaike.com' + href
        print(url)

# 6.在获取的页面中通过search查找文本
# {}大括号相当于正则的从头到后开始匹配，获取当中想要获取的数据
text = obj.html.search('把{}夹')[0]  # 获取从 "把" 到 "夹" 字的所有内容
text = obj.html.search('把糗事{}夹')[0]  # 获取从把子到夹字的所有内容
print(text)

print('*' * 1000)

# 7.支持XPath
a_s = obj.html.xpath('//a')  # 获取html内所有的a标签
for a in a_s:
    href = a.attrs['href']

    # 若是//开头的url都扔掉
    if href.startswith('//'):
        continue

    # 若是/开头的都是相对路径
    elif href.startswith('/'):
        print('https://www.qiushibaike.com' + href)


# 8.获取到只包含某些文本的Element对象（containing）
# 获取所有文本内容为幽默笑话大全_爆笑笑话_笑破你的肚子的搞笑段子 - 糗事百科 title标签
# 注意: 文本内有空格也必须把空格带上
title = obj.html.find('title', containing='幽默笑话大全_爆笑笑话_笑破你的肚子的搞笑段子 - 糗事百科')
print(title)
```



### r.html.search()

```
from requests_html import HTML,HTMLSession
#
session = HTMLSession()
r= session.get(url='https://baike.baidu.com/item/%E6%9D%80%E9%A9%AC%E7%89%B9/4557227?fr=aladdin')
# <div class="para" label-module="para">杀马特，一词源于英文单词smart,意思为时尚的；聪明的。</div>
# 返回一个对象 利用魔法方法进行封装
# search_obj = r.html.search('杀马特，一词源于{},意思为{}；聪明的')
# #<Result ('英文单词smart', '时尚的') {}>
# print(search_obj)
# # 相当于无名分组
# print(search_obj[0])#英文单词smart
# print(search_obj[1])#时尚的

search_obj = r.html.search('杀马特，一词源于{name},意思为{mast}；聪明的')
# <class 'parse.Result'> <Result () {'name': '英文单词smart', 'mast': '时尚的'}>
print(type(search_obj),search_obj)
# 相当于有名分组
print(search_obj['name'])
print(search_obj['mast'])



# %E6%9D%80%E9%A9%AC%E7%89%B9 == 杀马特
# 他的编码格式为utf8
print('杀马特'.encode('utf8'))
# b'\xe6\x9d\x80\xe9\xa9\xac\xe7\x89\xb9'
# %E6%9D%80%E9%A9%AC%E7%89%B9
# 差别 就是 \x变成%   并且大写
```

### (js注入)解决浏览器内核(针对反爬,如果没有做反爬无所谓)

```
解决浏览器内核(针对反爬,如果没有做反爬无所谓)
from requests_html import HTMLSession
session = HTMLSession()
response = session.get(url='https://www.baidu.com')
# 绕过网站对webdriver(网页内驱动软件;)的检测:
script='''
()=>{
Object.defineProperties(navigator,{
        webdriver:{
        get: () => undefined
        }
    })}'''
#利用模块进行js注入
print(response.html.render(script=script))

```



### r.html.render(.....) 渲染后的结果去替换 r.html.html

调用浏览器内核进行渲染

方式1 ：在次通过浏览器发送请求

方式2：r.html.render(reload=False)不会向浏览器发送请求，而是根据自己本地的请求进行渲染  

```
内部集成了pypettr,相当于selenium
```

pypettr,相当于selenium

```
1 pypettr 属于使用不广泛的，利用少
selenium是使用最多，使用方便，因此在大公司对他的限制很多，导致不能使用
```

　**注意：第一次运行render()方法时，它会将Chromium下载到您的主目录中(例如~/.pyppeteer/)。这种情况只发生一次。**

#### 下载Chromeium问题

　　因为是从国外的站点下载几分钟才3%，实在是太慢了。所以我们需要通过国内的镜像去下载！需要做以下几步:

- - 手动下载Chrome

    先去国内源下载自己需要的版本，地址：

    https://npm.taobao.org/mirrors/chromium-browser-snapshots/

  - 修改chromeium_downloader.py文件

    下载后之后解压后，进入python安装目录下的\Lib\site-packages\pyppeteer目录, 并打开chromium_downloader.py文件。

    ```
    # 找到自己的操作系统相应的配置位置
    '''
    chromiumExecutable = {
    'linux': DOWNLOADS_FOLDER / REVISION / 'chrome-linux' / 'chrome',
    'mac': (DOWNLOADS_FOLDER / REVISION / 'chrome-mac' / 'Chromium.app' /
    'Contents' / 'MacOS' / 'Chromium'),
    'win32': DOWNLOADS_FOLDER / REVISION / 'chrome-win32' / 'chrome.exe',
    'win64': DOWNLOADS_FOLDER / REVISION / 'chrome-win32' / 'chrome.exe',
    }
    '''
    from pyppeteer import __chromium_revision__, __pyppeteer_home__
    
    DOWNLOADS_FOLDER = Path(__pyppeteer_home__) / 'local-chromium'
    REVISION = os.environ.get('PYPPETEER_CHROMIUM_REVISION', __chromium_revision__)
    
    # 打印这两个变量可以知道执行的驱动具体位置
    print(DOWNLOADS_FOLDER)
    print(REVISION)
    
    '''
    由上面可以知道：chromium路径是：C:\Users\Ray\AppData\Local\pyppeteer\pyppeteer\local-chromium\575458\chrome-win32\chrome.exe
    
    所以自己建文件夹，然后一直到chrome-win32文件夹，把上面下载的chromium文件，拷贝到此目录下
    '''reder的使用
    ```

#### render的使用

```
from requests_html  import HTMLSession

session  =HTMLSession()
response = session.get('https://www.cnblogs.com/pythonywy/')

print(response.html.render())
进行js注入
模拟人操作浏览器
```

#### render的参数

  ```

二.render的参数
1.script(str)
执行的js代码

语法:response.html.render(script='js代码字符串格式')

2.scrolldown(int)
滑动滑块

和sleep联用为多久滑动一次

语法:response.html.render(scrolldown=页面向下滚动的次数)

3.retries(int)
加载页面失败的次数
语法:response.html.render(retries=加载页面失败的次数)
4.wait(float)
加载页面的等待时间（秒），防止超时（可选）
语法:response.html.render(wait=加载页面的等待时间（秒），防止超时（可选)
5.sleep(int)
在页面初次渲染之后的等待时间
语法:response.html.render(sleep=在页面初次渲染之后的等待时间)
6.timeout(int or float)
页面加载时间上线
语法:response.html.render(timeout=页面加载时间上线)
7.keep_page(bool)
如果为真，允许你用r.html.page访问页面
语法:response.html.render(keep_page=True/Flase)
8.reload(bool)
如果为假，那么页面不会从浏览器中加载，而是从内存中加载
语法:response.html.render(reload=Flase/True)
  ```



## **Element对象方法及属性**

```
		element对象 .absolute_links:绝对url
                    .links:相对url
                    .text:只显示文本
                    .html:标签也会显示
                    .attrs:属性
                    .find('css选择器')
                    .xpath('xapth路径')
                    .search('模板')
                    .search_all('模板')
```

## **与浏览器交互    r.html.page.XXX**

```

async def xxx():

await r.html.page.XXX

session.loop.run....(xxx())


	r.html.page.screenshot({'path':路径,'clip':{'x':1,'y':1,'width':100,'height':100}})

	r.html.page.evaluate('''() =>{js代码}’‘’})

	r.html.page.cookies()

	r.html.page.type('css选择器'，’内容‘，{’delay‘：100})

	r.html.page.click('css选择器',{'button':'left','clickCount':1,'delay':0})

	r.html.page.focus('css选择器')

	r.html.page.hover('css选择器')

	r.html.page.waitForSelector('css选择器')

	r.html.page.waitFor(1000)
```

```
from requests_html  import HTMLSession

session  =HTMLSession()
response = session.get('https://www.cnblogs.com/pythonywy/')

print(response.html.render(keep_page=true))
async def run():
    #交互语句
    await r.html.page.XXX

 

try:
    session.loop.run_until_complete(run())
finally:
    session.close()
```





## 键盘事件	r.html.page.keyboard.XXX

```
r.html.page.keyboard.down('Shift')
r.html.page.keyboard.up('Shift')

r.html.page.keyboard.press('ArrowLeft')

r.html.page.keyboard.type('喜欢你啊'，{‘delay’:100})
```



```
keyboard.down('键盘名称'):按下键盘不弹起(与键盘有点不太down('h')只会出现一个h而不是hhhhhhh....)
keyboard.up('键盘名称'):抬起按键
keyboard.press('键盘名称'):按下+弹起
keyboard.type('输入的字符串内容'，{‘delay’:100}) delay为每个子输入后延迟时间单位为ms
```



## **鼠标事件	r.html.page.mouse.XXX**



```
r.html.page.mouse.click(x,y,{
                'button'：'left',
                'click':1
                'delay':0
			})
r.html.page.mouse.down({'button'：'left'})
r.html.page.mouse.up({'button'：'left'})
r.html.page.mouse.move(x,y,{'steps'：1})
```

```
点击

click('css选择器',{ 'button'：'left', 'clickCount':1,'delay':0})
button为鼠标的按键left, right, or middle,
clickCount:点击次数默认次数为1
delay:点击延迟时间,单位是毫秒
mouse.click(x, y,{ 'button'：'left', 'clickCount':1,'delay':0})
x,y:muber数据类型,代表点击对象的坐标
点下去不抬起

mouse.down({'button':xxx,clickCount:xxx})
抬起鼠标

mouse.up({'button':xxx,clickCount:xxx})
```

# 其他

1. 等待

- waitFor('选择器, 方法 或者 超时时间')

  - 选择器: css 选择器或者一个xpath 根据是不是`//`开头

  - 方法:时候此方法是[page.waitForFunction()](https://zhaoqize.github.io/puppeteer-api-zh_CN/#?product=Puppeteer&version=v1.19.0&show=api-pagewaitforfunctionpagefunction-options-args)的简写

  - 超时时间:单位毫秒

2. 等待元素加载

   waitForSelector('css选择器')

3. 获取x,y坐标 

   ```
   
       mydic =await r.html.page.evaluate('''() =>{ 
           var a = document.querySelector('#kw')   #对象的css选择器
           var b = a.getBoundingClientRect()
           return {'x':b.x,'y':b.y , 'width':b.width , 'height':b.height }
           }''')
   
   ```

4. 执行js代码

   evaluate('js代码字符串格式')

5. 输入内容

   type('css选择器'，’内容‘，{’delay‘：100})

6. 聚焦

   focus('css选择器')

7. 移动动到

   hover('css选择器')

8. 获取cookies

   cookies()

9. 设置页面大小

   setViewport({'width': 1366, 'height': 768})

10. 截图

    screenshot({'path':保存本地路径,'clip':{'x':1,'y':1,'width':100,'height':100}})

    - x:图片的x坐标
    - y:图片的y坐标
    - width: 图片宽
    - height:图片高