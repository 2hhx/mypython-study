## render方法

我们先理一下关系`requests`和的作者是同一个人，`pyppeteer`是`nodejs`中`puppeteer`的非官方实现

`requests-html`调用的`pyppeteer`与浏览器进行交互,

`puppeteer`的中文文档 [点这里传送](https://segmentfault.com/a/1190000015913821)

`pyppeteer`的文档 [博文参考](https://blog.zhangkunzhi.com/2019/05/13/pyppeteer常用方法手册/index.html)

调用render 方法启动`pyppeteer`

使用之前要先下载`chromium` [下载地址](https://www.chromium.org/getting-involved/download-chromium)

你懂的，天朝网络环境很复杂，如果要用`pyppeteer`自己绑定的`chromium`，半天都下载不下来，所以我们要手动安装，然后在程序里面指定`executablePath`

对于`requests-html`源代码在714行中加入

```
executablePath=’path/to/the/chromium‘ 
from requests_html import HTMLSession

url  = 'https://httpbin.org/get'

session = HTMLSession()
res = session.get(url = url)
res.html.render()
print(res.html.html)
```

![img](https://i.loli.net/2019/08/07/48uaPQ1B9IlXrsy.png)

可以看到如上图中我用红色的圈出来的地方，标示的是无头浏览器`HeadlessChrome`，这个是明显不是正常的人类用户，会被反扒网站所识别

```
url  = 'https://httpbin.org/get'

session = HTMLSession(
            browser.args = [
                '--no-sand', 
                '--user-agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36"'
            ])
res = session.get(url = url)
res.html.render()
print(res.html.html)
```

在正常使用的浏览器的控制台中键入`navigator.userAgent`就可以看到浏览器的请求头把他复制到`--user-agent`之后，注意千万不能有空格，`--nosand` 是以最高权限运行

### 启动参数

```
kwargs = {
        'headless': False,
         'devtools': False, // 打开开发者工具
         'ignoreDefaultArgs':  // 忽略默认配置
         'userDataDir' :'./userdata', //设置用户目录，保存cookie
            'args': [
                '--disable-extensions',
                '--window-size={width},{height}',
                '--hide-scrollbars',
                '--disable-bundled-ppapi-flash',
                '--mute-audio', //页面静音
                '--no-sandbox',
                '--disable-setuid-sandbox',
                '--disable-gpu',
                '--enable-automation',
               
            ],
        'dumpio': True,
    }
```

请求网站可以看到我们进行设置的UA头生效了

### render 方法的参数

- retries 重试次数，默认为8，
- script，JS 脚本，可选参数，默认为None,`str`类型，如果有值，返回JS执行脚本的返回值
- wait 加载页面前等待的秒数，防止超时，默认0.2秒，可选参数，浮点型
- scrolldown，页面滚动次数，整数，默认为0,
- sleep, 首次渲染之后暂停的秒数，接收整数，可选类型，默认为0
- reload 默认为`True`,如果为False,如果为`False`,就会从内存中加载内容
- keep_page,默认为`False`,如果为`True`，就可以通过`r.html.page`和页面进行交互

```
"""Reloads the response in Chromium, and replaces HTML content
        with an updated version, with JavaScript executed.

        :param retries: The number of times to retry loading the page in Chromium.
        :param script: JavaScript to execute upon page load (optional).
        :param wait: The number of seconds to wait before loading the page, preventing timeouts (optional).
        :param scrolldown: Integer, if provided, of how many times to page down.
        :param sleep: Integer, if provided, of how many long to sleep after initial render.
        :param reload: If ``False``, content will not be loaded from the browser, but will be provided from memory.
        :param keep_page: If ``True`` will allow you to interact with the browser page through ``r.html.page``.
```

如果`sleep`和`scrolldown`一起用，表示翻一夜，停几秒

#### JS注入实例1

```
script = """
                () => {
                    return {
                        width: document.documentElement.clientWidth,
                        height: document.documentElement.clientHeight,
                        deviceScaleFactor: window.devicePixelRatio,
                    }
                }
                """
from requests_html import HTMLSession

url  = 'https://httpbin.org/get'

session = HTMLSession(
    browser_args=[
                '--no-sand',
                '--user-agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36"'
            ]
)
res = session.get(url = url)

r = res.html.render(script=script)
print(r)
```

输出结果为

```
{'width': 800, 'height': 600, 'deviceScaleFactor': 1}
```

#### JS 注入实例2 更改navigator.webdriver

```
'''() =>{
    
           Object.defineProperties(navigator,{
             webdriver:{
               get: () => undefined
             }
           })
        }'''
```

#### scrolldown

这个我们先改一下源码

![img](https://i.loli.net/2019/08/07/rkQEhYAHMPVCxoc.png)

![img](https://i.loli.net/2019/08/07/GUXVBery4Ydqjlv.png)

### 与浏览器进行交互

### page.screenshot([options])

```
- options `<object>` 可选配置 
    - path `<string>` 截图保存路径。截图图片类型将从文件扩展名推断出来。如果是相对路径，则从当前路径解析。如果没有指定路径，图片将不会保存到硬盘。
    - type `<string>` 指定截图类型, 可以是 jpeg 或者 png。默认 'png'.
    - quality `<number>` 图片质量, 可选值 0-100. png 类型不适用。
    - fullPage <boolean> 如果设置为true，则对完整的页面（需要滚动的部分也包含在内）。默认是false
    - clip `<object>` 指定裁剪区域。需要配置：
        - x `<number>` 裁剪区域相对于左上角（0， 0）的x坐标
        - y `<number>` 裁剪区域相对于左上角（0， 0）的y坐标
        - width `<number>` 裁剪的宽度
        - height `<number>` 裁剪的高度
    - omitBackground <boolean> 隐藏默认的白色背景，背景透明。默认不透明
    - encoding `<string>` 图像的编码可以是 base64 或 binary。 默认为“二进制”。
```

### 截图实例

```
import asyncio

from requests_html import HTMLSession

url  = 'https://httpbin.org/get'

session = HTMLSession(
    browser_args=[
                '--no-sand',
                '--user-agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36"'
            ]
)
res = session.get(url = url)
script = """
                () => {
                    return {
                        width: document.documentElement.clientWidth,
                        height: document.documentElement.clientHeight,
                        deviceScaleFactor: window.devicePixelRatio,
                    }
                }
               """
try:
    res.html.render(script=script,sleep = 1,keep_page = True)
    async def main():
        await res.html.page.screenshot({'path':'1.png'}) # 传入参数用字典path 代表路径 值为你要存放的路径

    asyncio.get_event_loop().run_until_complete(main())
finally:
    session.close()
#  指定截图位置，截图从哪个坐标开始
screenshot({'path':'1.png'，'clip':'{'x':200,'y':'300','weith':400,'height':'600'}'})
```

#### page.evaluate(pageFunction[, ...args])

- pageFunction <function|string> 要在页面实例上下文中执行的方法

```
js1 = '''() =>{
    
           Object.defineProperties(navigator,{
             webdriver:{
               get: () => undefined
             }
           })
        }'''
        

js4 = '''() =>{Object.defineProperty(navigator, 'languages', {get: () => ['en-US', 'en']});
        }'''
await page.evaluate(js1) ## 更改webdriver
await page.evaluate(js4) ##更改语言
```

#### page.setViewport()

设置页面大小

```
page.setViewport({'width': 1366, 'height': 768})
```

#### page.cookies（）

如果不指定任何 url，此方法返回当前页面域名的 cookie。 如果指定了 url，只返回指定的 url 下的 cookie。

#### page.type(selector, text[, options])

```
- selector `<string>` 要输入内容的元素选择器。如果有多个匹配的元素，输入到第一个匹配的元素。
- text `<string>` 要输入的内容
- options `<object>`
    - delay `<number>` 每个字符输入的延迟，单位是毫秒。默认是 0。
```

#### page.click(selector[, options])

```
- selector `<string>` 要点击的元素的选择器。 如果有多个匹配的元素, 点击第一个。
- options `<object>`
    - button `<string>` left, right, 或者 middle, 默认是 left。
    - clickCount `<number>` 默认是 1. 查看 UIEvent.detail。
    - delay `<number>` mousedown 和 mouseup 之间停留的时间，单位是毫秒。默认是0
```

#### page.focus(selector)

- selector `<string>` 要给焦点的元素的选择器selector。如果有多个匹配的元素，焦点给第一个元素。

#### page.hover(selector)

- selector `<string>` 要hover的元素的选择器。如果有多个匹配的元素，hover第一个

#### page.waitFor(selectorOrFunctionOrTimeout[, options[, ...args]])

```
- selectorOrFunctionOrTimeout <string|number|function> 选择器, 方法 或者 超时时间
- options `<object>` 可选的等待参数
    ...args <...Serializable|JSHandle> 传给 pageFunction 的参数
```

- 如果 `selectorOrFunctionOrTimeout` 是 `string`, 那么认为是 css 选择器或者一个xpath, 根据是不是'//'开头, 这时候此方法是 [page.waitForSelector](https://zhaoqize.github.io/puppeteer-api-zh_CN/#?product=Puppeteer&version=v1.19.0&show=api-pagewaitforselectorselector-options) 或 [page.waitForXPath](https://zhaoqize.github.io/puppeteer-api-zh_CN/#?product=Puppeteer&version=v1.19.0&show=api-pagewaitforxpathxpath-options)的简写
- 如果 `selectorOrFunctionOrTimeout` 是 `function`, 那么认为是一个predicate，这时候此方法是[page.waitForFunction()](https://zhaoqize.github.io/puppeteer-api-zh_CN/#?product=Puppeteer&version=v1.19.0&show=api-pagewaitforfunctionpagefunction-options-args)的简写
- 如果 `selectorOrFunctionOrTimeout` 是 `number`, 那么认为是超时时间，单位是毫秒，返回的是Promise对象,在指定时间后resolve
- 否则会报错

### page.emulate

模拟手机

```
await page.emulate(iPhone);
```

### 键盘事件

[详细的键盘键名语法](https://github.com/GoogleChrome/puppeteer/blob/master/lib/USKeyboardLayout.js)

语法：

```
res.html.page.keyboard.XXX
```

#### keyboard.down(key[, options])

- key `<string>` 按下的键名, 比如 ArrowLeft. 一个包含所有键名的列表见 USKeyboardLayout.-
- options `<object>` - text `<string>` 如果指定，则使用此文本生成输入事件.

#### keyboard.up(key)

- key `<string>` 要释放的键的键名, 例如 ArrowLeft

#### keyboard.press(key[, options])

- key `<string>` 按下的键名, 比如 ArrowLeft.
- options `<object>` - text `<string>` 如果指定，则使用此文本生成输入事件。 - delay `<number>` 在 keydown 和 keyup 间隔的时间, 以毫秒为单位. 默认为 0。

#### keyboard.type(text, options)

- text `<string>` 要输入到焦点元素中的文本。

- options `<object>` - delay `<number>` 按键间隔的时间, 以毫秒为单位. 默认为 0。

  ```
        page.keyboardtype('喜欢你啊'，{‘delay’:100})
  ```

  ### 鼠标事件

```
r.html.page.mouse.XXX
```

#### mouse.click(x, y, [options])

- x `<number>`
- y `<number>`
- options `<object>`
- button `<string>` left ，right 或 middle，默认是 left。
- clickCount `<number>` 默认是 1。见 UIEvent.detail。
- delay `<number>` 在毫秒内且在 mousedown 和 mouseup 之间等待的时间。 默认为0。

#### mouse.down([options])

- options `<object>`
- button `<string>` left，right 或 middle，默认是 left。
- clickCount `<number>` 默认是 1。

### mouse.up([options])

- options `<object>`
- button `<string>` left，right，或 middle，默认是 left。
- clickCount `<number>` 默认是 1。

## 项目代码

`puppeteer`项目参考 [传送门](https://juejin.im/post/5b58a1a051882519790c9295)

### 模拟登陆Gmail

```
import asyncio
import time
from pyppeteer import launch


async def gmailLogin(username, password, url):
    #'headless': False如果想要浏览器隐藏更改False为True
    # 127.0.0.1:1080为代理ip和端口，这个根据自己的本地代理进行更改，如果是vps里或者全局模式可以删除掉'--proxy-server=127.0.0.1:1080'
    browser = await launch({'headless': False, 'args': ['--no-sandbox', '--proxy-server=127.0.0.1:1080']})
    page = await browser.newPage()
    await page.setUserAgent(
        'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.67 Safari/537.36')

    await page.goto(url)

    # 输入Gmail
    await page.type('#identifierId', username)
    # 点击下一步
    await page.click('#identifierNext > content')
    page.mouse  # 模拟真实点击
    time.sleep(10)
    # 输入password
    await page.type('#password input', password)
    # 点击下一步
    await page.click('#passwordNext > content > span')
    page.mouse  # 模拟真实点击
    time.sleep(10)
    # 点击安全检测页面的DONE
    # await page.click('div > content > span')#如果本机之前登录过，并且page.setUserAgent设置为之前登录成功的浏览器user-agent了，
    # 就不会出现安全检测页面，这里如果有需要的自己根据需求进行更改，但是还是推荐先用常用浏览器登录成功后再用python程序进行登录。

    # 登录成功截图
    await page.screenshot({'path': './gmail-login.png', 'quality': 100, 'fullPage': True})
    #打开谷歌全家桶跳转，以Youtube为例
    await page.goto('https://www.youtube.com')
    time.sleep(10)


if __name__ == '__main__':
    username = '你的gmail包含@gmail.com'
    password = r'你的gmail密码'
    url = 'https://gmail.com'
    loop = asyncio.get_event_loop()
    loop.run_until_complete(gmailLogin(username, password, url))
# 代码由三分醉编写，网址www.sanfenzui.com，参考如下文章：
# https://blog.csdn.net/Chen_chong__/article/details/82950968
```

