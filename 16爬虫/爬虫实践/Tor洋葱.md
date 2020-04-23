# Tor 与 爬虫



#### Tor 是什么

先来看看维基百科的介绍：

> Tor是实现匿名通信的自由软件。其名源于“The Onion Router”（洋葱路由器）的英语缩写。用户可透过Tor接达由全球志愿者免费提供，包含7000+个中继的覆盖网络，从而达至隐藏用户真实地址、避免网络监控及流量分析的目的。
> Tor用户的互联网活动（包括浏览在线网站、帖子以及即时消息等通信形式）相对较难追踪。
> Tor的设计原意在于保障用户的个人隐私，以及不受监控地进行秘密通信的自由和能力。

Tor是在1990年代中期由美国海军研究实验室的员工，数学家保罗·西维森（Paul Syverson）和计算机科学家迈克·里德（G. Mike Reed）和大卫·戈尔德施拉格（David Goldschlag），为保护美国情报通信而开发的软件。之后，洋葱路由于1997年交由美国国防高等研究计划署进行进一步开发。

Tor是如何做到匿名通信的呢？

#### Tor 的原理

其实上文中提到的7000+中继器就是关键。通过一系列的中继器，Tor可以在你的电脑和目的地之间建立一种跳跃的连接。默认的情况下,Tor通过3个中继器进行跳转连接。

•Entry/guard中继器-这是Tor网络的入口点。这些中继器在存在了一段时间后,如果被证明是稳定的，并具有高带宽，就会被选来作为guard中继器。

•Middle中继器-middle中继器事实上是中间节点，用于将流量从guard中继器传输到exit中继器。这可以避免guard中继器和exit中继器探查到彼此的位置。

•Exit中继器——这些中继器是位于Tor网络边缘的出口点。这些中继器会将流量发送到客户指定的最终目的地。





可以看到Tor访问网络不仅仅只用一层代理，而是采用了多层代理，在每一层代理都进行了加密，层层加密使得信息的来源得到保护。就像一个洋葱，只有把每一层都剥开，才能看到核心。因此Tor才被称为“洋葱网络”。



Tor网络上的数据包不是直接通过从源到目的地，而是通过多个节点的随机路径，因此任何单点的观察者都无法知道数据来自哪里或去哪里。

#### Tor 的安装与使用

Tor 有官方的浏览器，可以直接下载，据说连接上之后可以直接当梯子，不过这里我想不这样玩，以前写爬虫经常会遇到封ip的情况，而Tor服务默认每10分钟就会自动切换ip，这个时间是可以自由设置的，这样就不用去买ip池，直接用tor做代理层写爬虫。

Mac上安装Tor很简单，一行命令就搞定：



```undefined
brew install tor
```

耐心等待，这个时间有点长。强烈建议开启梯子之后再执行安装命令，原因你们懂的。

安装完成之后，命令行直接执行



```undefined
tor
```

就开启了tor服务







tor服务开启

有时候，执行了上面的命令，会卡在5%左右，一直不能到100%，这时候就需要修改一下Tor的配置文件了，得通过梯子才能连接到tor的中继器。Mac 下面是在 /usr/local/etc/tor 文件夹下面有个`torrc.sample`的文件，这个是模板，我们先复制它到torrc



```css
cp torrc.sample torrc
```

然后修改一下配置



```undefined
vi torrc
```

添加两句话



```css
Socks5Proxy 127.0.0.1:1086  # 通过梯子代理连接tor
MaxCircuitDirtiness 10      # 每隔n时间进行ip更换，时间单位为秒
```

好了，现在在命令行重新执行`tor`，应该就没问题了。

下面我们写个小demo测试下是不是每10秒就自动更换ip了，代码如下：



```python
import time
import requests

def get_ip():
    # 获取ip地址的网站
    url = 'https://api.ipify.org?format=json'
    # 9050端口是tor的端口，我们通过tor访问网站
    proxies = {'http': 'socks5://127.0.0.1:9050', 'https': 'socks5://127.0.0.1:9050'}

    r = requests.get(url, proxies=proxies)
    print('当前ip: %s' % r.text)


def main():
    # 一直循环
    while True:
        # 获取ip
        get_ip()
        # 等待10秒
        time.sleep(10)

if __name__ == "__main__":
    main()
```

输出如下，结果和我们预想的一样。







结果

有些网站为了防止爬虫，都开始使用js渲染页面，那我们来试试`selenium`



```python
import time
import json
import requests
import traceback

from selenium import webdriver
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys

class Spider(object):

    def __init__(self):
        self.url = 'https://api.ipify.org?format=json'
        self.proxy = 'socks5://127.0.0.1:9050'
        self.create_driver(proxy=self.proxy)

    def create_driver(self, proxy=None):
        options = webdriver.ChromeOptions()
        if proxy:
            options.add_argument('--proxy-server={0}'.format(proxy))
        options.add_argument('--disable-gpu')
        options.add_argument('blink-settings=imagesEnabled=false')
        options.add_argument('--headless')
        self.driver = webdriver.Chrome(executable_path='./chromedriver', chrome_options=options)
        self.driver.implicitly_wait(5)

    def start(self):

        print('......程序开始......')
        try:
            self.driver.get(self.url)
            # 解析数据
            ip = json.loads(self.driver.find_elements_by_xpath('//pre')[0].text)['ip']
            print('当前ip: %s' % ip)

        except Exception as e:
            traceback.print_exc()
        finally:
            self.driver.quit()
            print('......程序退出......')


if __name__ == "__main__":
    spider = Spider()
    spider.start()
```



恩，也是没问题的。

ok，这篇文章只是简单的介绍了下Tor，并结合python做了一个爬虫小测试，其实慢慢研究一下，应该还是有不少好玩的东西。





# [Win10环境下的Scrapy结合Tor进行匿名爬取](https://www.cnblogs.com/kylinlin/p/5242266.html)



本文内容来源：http://blog.privatenode.in/torifying-scrapy-project-on-ubuntu/

在使用Scrapy的时候，一旦进行高频率的爬取就容易被封IP，此时可以通过使用TOR来进行匿名爬取，同时要安装Polipo代理服务器

 

注意：要进行下面的操作的前提是，你能翻墙

####  

#### 安装TOR

下载地址：https://www.torproject.org/download/download.html.en

[![Image 003](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152919002-47905298.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152917830-1591907464.png)

下载Expert Bundle并解压到一个目录下，例如：D:\Tor，这个版本并没有一个图形化的操作界面，要修改配置十分麻烦，可以通过下载Vidalia来使用TOR，Vidalia的下载地址：https://people.torproject.org/~erinn/vidalia-standalone-bundles/ ，下载该页面的最下面那个即可：vidalia-standalone-0.2.21-win32-1_zh-CN.exe，安装完成之后，以管理员权限运行Start Vidalia.exe，进行下面的设定

[![Image 007](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152920643-1770667969.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152919830-454932150.png)

[![Image 006](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152922002-1668026837.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152921205-1721035618.png)

 

点击启动Tor

[![Image 009](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152923002-1754751671.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152922487-1170112161.png)

过一阵子后显示连接成功

[![Image 010](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152924174-305348809.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152923612-677331345.png)

####  

#### 下载安装Polipo

下载地址：http://www.pps.univ-paris-diderot.fr/~jch/software/files/polipo/

选择polipo-1.1.0-win32.zip，下载并解压，然后编辑解压后的文件config.sample，在文件的开头加上以下配置

```
socksParentProxy = "localhost:9050"

socksProxyType = socks5

diskCacheRoot = ""
```

使用cmd命令运行该目录下的程序：polipo.exe -c config.sample

[![Image 011](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152925143-335761818.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152924705-171558955.png)

打开edge浏览器，设置代理

[![Image 013](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152926534-1422251554.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152925799-1359799697.png)

 

然后在浏览器中访问：https://check.torproject.org/

看到以下的界面意味着配置成功

[![Image 014](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152927612-208257011.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152927034-894321295.png)

####  

#### 配置Scrapy

在settings.py文件中加入下面的内容



```
#More comprehensive list can be found at

#http://techpatterns.com/forums/about304.html

USER_AGENT_LIST = [

    'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.7 (KHTML, like Gecko) Chrome/16.0.912.36 Safari/535.7',

    'Mozilla/5.0 (Windows NT 6.2; Win64; x64; rv:16.0) Gecko/16.0 Firefox/16.0',

    'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_3) AppleWebKit/534.55.3 (KHTML, like Gecko) Version/5.1.3 Safari/534.53.10',

    ]

HTTP_PROXY = 'http://127.0.0.1:8123'

DOWNLOADER_MIDDLEWARES = {

    'myspider.middlewares.RandomUserAgentMiddleware': 400, # 修改这里的myspider为项目名称

    'myspider.middlewares.ProxyMiddleware': 410, # 同上

    'scrapy.contrib.downloadermiddleware.useragent.UserAgentMiddleware': None,

}
```



在scrapy项目的根目录新建一个middlewares.py文件，并输入以下内容



```
import random

from scrapy.conf import settings

from scrapy import log

class RandomUserAgentMiddleware(object):

    def process_request(self, request, spider):

        ua = random.choice(settings.get('USER_AGENT_LIST'))

        if ua:

            request.headers.setdefault('User-Agent', ua)

            #this is just to check which user agent is being used for request

            spider.log(

                u'User-Agent: {} {}'.format(request.headers.get('User-Agent'), request),

                level=log.DEBUG

            )

class ProxyMiddleware(object):

    def process_request(self, request, spider):

        request.meta['proxy'] = settings.get('HTTP_PROXY')
```



至此，scrapy与tro的整合完成了，本文不对任何人使用这个方法所造成的后果负责

####  

#### 配置Tor浏览器

下面的内容与上面无关，只是记录一下如何使用Tor浏览器，在我们下载tor的页面上，还有一个下载选项（第一个就是一个浏览器，通过该浏览器可以匿名访问网页，Tor Browser会自动通过Tor网络启动Tor的后台进程连接网络。一旦关闭程序的便会自动删除隐私敏感数据，如HTTP cookie和浏览历史记录，以避免窃听并保留在互联网上的隐私）

[![Image 015](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152928924-1376858261.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152928284-2081872296.png)

下载了第一个Tor Browser并安装后，进行下面的配置

[![Image 016](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152930284-1281235375.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152929659-829649178.png)

由于Tor的连接被墙掉了，所以要配置网桥

[![Image 017](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152932065-336551511.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152931221-1241001481.png)

 

获取网桥：https://bridges.torproject.org/options

[![Image 018](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152933143-830669787.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152932518-1655117931.png)

 

[![Image 019](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152934440-480534138.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152933846-85626850.png)

将网桥复制下来，粘贴到tor浏览器上

[![Image 020](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152935440-1477409046.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152934940-1916065357.png)

[![Image 022](https://images2015.cnblogs.com/blog/771870/201603/771870-20160304152936315-846665473.png)](http://images2015.cnblogs.com/blog/771870/201603/771870-20160304152935862-788976666.png)

有时候连接不成功，就要再申请新的网桥来尝试