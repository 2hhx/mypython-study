**/1 前言/**

  最近在家闲的快发霉了，想看看电视剧吧，发现这个要充会员，那个也要充会员？？？

![img](https://mmbiz.qpic.cn/mmbiz_png/icjSAZybsq57iarjlyYOAjogJkyJc0Aye42ck0Jf1a1IQ6plibV4Y8iaUDDhT8taIwaEia9vZcUcz2u1INOjpKMNdvA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1) 

  这种特殊时期我连饭都吃不起了哪还有钱充会员，于是我决定发挥技术宅男的优势，用python做个免费的vip视频播放软件，从此告别会员充值，“白嫖”看视频！

  下面本宅男就给大家介绍一下，不充会员，如何看VIP视频。

  主体思路是引用VIP视频解析接口，然后用python将其整合到可视化窗口，再添加VIP视频网址输入模块和启动浏览器播放按钮，最后，使用女神的照片为背景，就大功告成了，下面是具体的实现步骤。

![img](https://mmbiz.qpic.cn/mmbiz_png/icjSAZybsq57iarjlyYOAjogJkyJc0Aye4E6ibVZbCvB8dYE69lzUatngtLDjicDwOIib3C8j2iabqCLycMLlko4lQuQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



**/2 分析找到解析地址/**

  1、首先找到解析地址的网站，这种网站很多，随便找一个，如下图所示。

![img](https://mmbiz.qpic.cn/mmbiz_png/icjSAZybsq57iarjlyYOAjogJkyJc0Aye4GkD2ENYFyAmwdmCRiafc51y7VnVSg6dx0MGUUx5KHPIxrar6y8zK32Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  2、将vip视频网址输进去，然后打开流量分析工具。

![img](https://mmbiz.qpic.cn/mmbiz_png/icjSAZybsq57iarjlyYOAjogJkyJc0Aye4IP7m5MduMO0CicUCL48surPd9cxFyhYBDiaaktiblFUuMYAUJ4E4RSpcA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  3、点击“Go-点击开始解析”，如下图所示。

![img](https://mmbiz.qpic.cn/mmbiz_png/icjSAZybsq57iarjlyYOAjogJkyJc0Aye40FcSIu8o2uZjfDNjCibnrbAAlVAar5hmWcTqIWW9utIibWAnx9hw1DiaQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  仔细看一下划红线的url，发现它是一个拼接的url，后面的https://www.iqiyi.com/v_19rv62nbf8.html是视频网页，那么http://jx.598110.com/?url=就是我们要找的视频接口啦！



**/3 启用selenium模块/**

  Selenium是一个用于测试网站的自动化测试工具，支持各种浏览器包括Chrome、Firefox、Safari等主流界面浏览器，同时也支持phantomJS无界面浏览器，支持Windows、Linux、IOS、Android等多种操作系统。

  Selenium的安装比较简单，只需命令行输入pip install Selenium

Selenium调用浏览器必须有一个webdriver驱动文件

Chrome驱动文件下载：‘https://chromedriver.storage.googleapis.com/index.html?path=2.35/’

Firefox驱动文件下载：

‘https://github.com/mozilla/geckodriver/releases’

Selenium调用浏览器打开网页只需三行代码，如下图所示。



![img](https://mmbiz.qpic.cn/mmbiz_png/icjSAZybsq57iarjlyYOAjogJkyJc0Aye4RO4WBkLJZXaPoqibRjCglbbDpWnkbsT6O5MjOHHj1vnS4GiaDwndZ3tA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

上图中的url为接口地址+vip视频网页地址。



**/4 调用tkinter模块，做个可视化界面/**

  最近在家闲的快发霉了，想看看电视剧吧，发现这个要充会员，那个也要充会员。  

  Tkinter是python默认的GUI库，我们可以用它实现很多直观的功能，而且使用比较简单，通过各种控件可以增加可视化窗口的功能。

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

  本次主要使用Label、Entry、Button等控件。其中Canvas组件和 html5 中的画布一样，都是用来绘图的，可以将图形，文本，小部件或框架放置在画布上。

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

  主要解释一下，第一行width和heigth是画布的宽度和高度，第五行266和150是图片中心在画布中的位置，因为图片像素是533X300，所以这种布局恰好将画布铺满。

  Label 组件用于显示文本和图像，如下图所示。



![img](https://mmbiz.qpic.cn/mmbiz_png/icjSAZybsq57iarjlyYOAjogJkyJc0Aye4gcZmrYpo9GIa7yMDadt9o0yI9EdxjNspRickx5KmamgCWGHqLBa51Dw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  **Text**是要显示的文本，bg是背景颜色，font是字体样式及大小，fg是字体颜色，padx是文本和文本框的横向距离，pady是文本和文本框的纵向距离，单位是像素。

  **Entry****（输入框）**组件通常用于获取用户的输入文本，如下图所示。

![img](https://mmbiz.qpic.cn/mmbiz_png/icjSAZybsq57iarjlyYOAjogJkyJc0Aye41rBOOpZhoniaRlic08ibgrB72kK4CPGy7QjqDGYDCkpxuGiaQ0xp99YmpQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  在这一步走了很多坑，最终发现要这样表述，Search即是输入框获得的内容。**Button****（按钮）**组件用于实现各种各样的按钮。Button 组件可以包含文本或图像，你可以将一个 Python 的函数或方法与之相关联，当按钮被按下时，对应的函数或方法将被自动执行。



![img](https://mmbiz.qpic.cn/mmbiz_png/icjSAZybsq57iarjlyYOAjogJkyJc0Aye4ZVV0xHSbgjHtNk3JnGdD0khCBqlpsNiaxyOiaOr96Qic9lGJNGH8p7hCg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  Text为按钮名称OpenHtml为要执行的函数，search_text.get()Entry输入框的内容，作为OpenHtml的参数，到此的效果图如下。

![img](https://mmbiz.qpic.cn/mmbiz_png/icjSAZybsq57iarjlyYOAjogJkyJc0Aye4Zt94vVe6xk0ND0hwj9uA2Ria9YaIz9CKW4dicxJb4pcibnBVefYqRkNzg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  哇喔，女神好美啊！



**/5 将程序打包成可执行的.exe文件/**

  关于Python实现打包的方式，小编最近也有写，回头发给大家学习。利用python有现成的模块pyinstaller，在pycharm里可以直接安装，安装完成后打开Win+R，输入cmd打开命令窗口，直接输入下图命令。

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

  打码的是代码文件地址,运行成功后,会提示生成exe文件的位置。

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)



**/6 整体效果演示/**

  最终呈现的整体效果动图，小编以gif动图形式给大家展示，但是其大小超过了5M，在文章中加载不出来，小编将动图和代码文件都上传到了github地址上，后台回复“**视****频播放**”四个字即可获取代码和动图地址。



**/7 结语/**

  本文主体思路是引用VIP视频解析接口，然后用python将其整合到可视化窗口，再添加VIP视频网址输入模块和启动浏览器播放按钮，最后，使用女神的照片为背景，就大功告成了。

  欢迎大家尝试，消耗在家的无聊时间。本文涉及的代码都上传到了github地址上，后台回复“**视****频播放**”四个字即可获取代码。



**-------------------** End **-------------------**

往期精彩文章推荐：

* ## [干货|Python大佬手把手带你破解哔哩哔哩网滑动验证（下篇）](http://mp.weixin.qq.com/s?__biz=MzU3MzQxMjE2NA==&mid=2247486028&idx=2&sn=8281b1ff1e2ff6174abff6f60211dc1e&chksm=fcc34c67cbb4c5715ed847e3400adbed3db39f52b80391fe9a2cd6f28e7a41877a75116021ca&scene=21#wechat_redirect) 

* ## [40行代码教你利用Python网络爬虫批量抓取小视频](http://mp.weixin.qq.com/s?__biz=MzU3MzQxMjE2NA==&mid=2247486132&idx=1&sn=4ef0654e5b56fbb09a611a43ad69be02&chksm=fcc34c9fcbb4c5894e419eedc655cfd198a413fb8ff21d6006757530d1b583ec3b5b5dc8169b&scene=21#wechat_redirect) 

* ## [利用Python网络爬虫抓取微信好友的签名及其可视化展示](http://mp.weixin.qq.com/s?__biz=MzU3MzQxMjE2NA==&mid=2247484235&idx=2&sn=c88325878677fa2596ce0f03047f4ecb&chksm=fcc34560cbb4cc769da87af2bbd3d53f271b73e7c89daf48965167b19c5e214525c77565a69c&scene=21#wechat_redirect)