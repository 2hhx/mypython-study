# 1 什么是互联网

互联网是由于网络设备(网线，路由器，交换机，防火墙等等)和计算机连接而成，像一张网一样。

## 1.1 互联网建立的目的？

互联网的核心价值在于数据的共享和传递，数据是放在一台计算机上 的，而将计算机互联到一起的目的就是为了能够方便彼此之间数据的共享和传递，否则你只能拿优盘取别人计算机上拷贝数据。

# 2 什么是上网？

我们所谓的上网便是由用户端计算机发送请求给目标计算机，将目标计算机上的书籍下载到本地的过程。

# 什么是爬虫？

 爬虫的定义：向网站发起请求，获取资源后分析并提取有用数据的程序

用户获取网络数据的方式是：浏览器提交请求->下载网页代码->解析/渲染成页面。
爬虫学术概念：爬取数据，爬虫就是通过编写程序模拟浏览器上网，让其去互联网上抓取数据的过程。
爬虫程序要做的就是：
模拟浏览器发送请求->下载网页的代码->只提取有用的数据->存放与数据库或者文件中。
爬虫程序只提取网页代码中对我们有用的数据。
模拟浏览器发送请求

## 为什么要使用爬虫技术

 requests 模块底层帮助我们封装好了socket套接字，我们只需要关注http协议的通信流程；

## 普通用户获取数据：

​    打开浏览器，输入网址
​    访问目标网站
​    目标网站将数据返回给浏览器
​    浏览器将数据进行渲染
​    然后可以手动获取（ctrl+c ctrl + V）

## "爬虫程序"获取数据:

1. 模拟浏览器往目标网站发送请求;

2. 请求库：

   * requests模块

   * selenium模块

3. 获取目标网站返回的响应数据
4. 服务端会自动将数据返回，无需通过代码实现
5. 解析并提取有价值的数据
   * 解析模块
     * re正则模块
     * BeautifulSoup4解析库
     * xpath解析语法：通过文档树，查找规则
     * selector属性选择解析库：Css
6. 保存到数据库或者本地
   * 存储库：
     * MySQL
     * redis
     * mongodb
     * file



# 爬虫的全过程

1. 发送请求（请求体可以携带无限的数据）
2. 获取响应数据
3. 解析并且提取数据
4. 保存数据

# 爬虫三部曲

1. 发送请求
   * 先分析目标网站的http协议请求流程
   * 编写代码
2. 获取数据
3. 保存数据

# 爬虫的价值：

  互联网中最有价值的便是数据，比如天猫商城的商品信息，链家网的租房信息，雪球网的证券投资信息等等，这些数据都代表了各个行业的真金白银，可以说，谁掌握了行业内的第一手数据，谁就成了整个行业的主宰，如果把整个互联网的数据比喻为一座宝藏，那我们的爬虫课程就是来教大家如何来高效地挖掘这些宝藏，掌握了爬虫技能，你就成了所有互联网信息公司幕后的老板，换言之，它们都在免费为你提供有价值的数据。

# 分析http协议的请求流程

[http协议参考链接](http://www.cnblogs.com/linhaifeng/articles/8243379.html)

1. 谷歌浏览器自带的开发者工具（推荐使用）
2. fildder(一款软件，需要下载)

* GET:
  * 请求url:Request URL: https://www.baidu.com/
  * 请求方式：Request Method: GET
  * 响应头注意点（请求后，服务端返回的数据）：
    * set-cookies： 服务端告诉浏览器要保存cookies
    * Location: 服务端告诉浏览器需要立马跳转的url地址
  * 请求头注意点（携带访问目标服务端的数据）：
    * Cookie
    * user-agent: 浏览器凭证，服务端有可能通过它做反扒
    * Referer: 当前url，上一次访问的url地址;
  * 请求参数：params
    * 可携带1KB左右的数据
    * https://www.baidu.com/s?wd=%E7%BE%8E%E5%A5%B3    - wd=%E7%BE%8E%E5%A5%B3
* POST:
  - 请求url:Request URL: https://passport.baidu.com/v2/api/?login
  - 请求方式：Request Method: POST
  - 响应头注意点（请求后，服务端返回的数据）：
    - set-cookies： 服务端告诉浏览器要保存cookies
    - Location: 服务端告诉浏览器需要立马跳转的url地址
  - 请求头注意点（携带访问目标服务端的数据）：
    - Cookie
    - user-agent: 浏览器凭证，服务端有可能通过它做反扒
    - Referer: 当前url，上一次访问的url地址;https://www.baidu.com/s?wd=%E7%BE%8E%E5%A5%B3
  - 请求体data：
    - 明文用户名:
      - 正确用户名
    - 正确密文pwd:
      - OQUoK471BPXLSQbQ3rKKpbYJZ/x8zgZ91LVs4Bw6sKoNeuJH9KoOkac0mhDiZ/H3n1Adxq1g/tgSD3EEhreqVoxJMO3VxqYH5LDlHR0U0gxeGpeuTaHGKWE4aun6vxVWjYIJcfAmQ3/b2JUqGawYcM9xtJZPrhPU86u/1i7e3DM=



# Requests

[requests方法总结使用](https://www.jianshu.com/p/50bdcb7cd5f6)

1、请求方式：
    常用的请求方式：GET，POST
    其他请求方式：HEAD，PUT，DELETE，OPTHONS

```
ps：用浏览器演示get与post的区别，（用登录演示post）

post与get请求最终都会拼接成这种形式：k1=xxx&k2=yyy&k3=zzz
post请求的参数放在请求体内：
    可用浏览器查看，存放于form data内
get请求的参数直接放在url后
```

2、请求url
    url全称统一资源定位符，如一个网页文档，一张图片
    一个视频等都可以用url唯一来确定

```
url编码
https://www.baidu.com/s?wd=图片
图片会被编码（看示例代码）
```

```
网页的加载过程是：
加载一个网页，通常都是先加载document文档，
在解析document文档的时候，遇到链接，则针对超链接发起下载图片的请求
```

3、请求头
    User-agent：请求头中如果没有user-agent客户端配置，
    服务端可能将你当做一个非法用户
    host
    cookies：cookie用来保存登录信息

```
一般做爬虫都会加上请求头
```

4、请求体
    如果是get方式，请求体没有内容
    如果是post方式，请求体是format data

```
ps：
1、登录窗口，文件上传等，信息都会被附加到请求体内
2、登录，输入错误的用户名密码，然后提交，就可以看到post，正确登录后页面通常会跳转，无法捕捉到post 
```

# 爬取百度图片

```
from urllib.parse import urlencode
import requests

headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36',
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9'
}

# https://www.baidu.com/s?wd=美女
response=requests.get('https://www.baidu.com/s?'+urlencode({'wd':'美女'}),headers=headers)
response=requests.get('https://www.baidu.com/s',params={'wd':'美女'},headers=headers) #params内部就是调用urlencode
reponse.encoding = 'utf8'
print(response.text)
```



# 代理IP



https://www.xicidaili.com/wt/
https://www.kuaidaili.com/free/
http://www.xiladaili.com/