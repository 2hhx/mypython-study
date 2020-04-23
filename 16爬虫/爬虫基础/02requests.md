# HTTP协议

HTTP，Hypertext Transfer Protocol 超文本传输协议

HTTP是一个基于"请求与响应"模式的，无状态的应用层协议

HTTP协议采用URL作为定位网络资源的标识。

URL格式：http://host[:post]\[path]

URL是通过HTTP协议存取资源的Internet路径，一个URL对应一个数据资源。

1. host:合法的Internet主机域名或者IP地址

2. port:端口号,缺省端口为80(默认为80)

3. path：请求资源的路径

4. HTTP协议对资源的操作

   ![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819204516462-1319632079.png)

5. ![1566219086050](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1566219086050.png)






## 主要掌握get，head

网络链接有风险，异常处理很重要：

```python
#检测代码是否可以爬取
import requests


def getHTMLTEXT(url):
    try:
        r = requests.get(url,timeout = 30)
        r.raise_for_status()   #如果状态码不是200，会出错
        r.encoding = r.apparent_encoding
        return r.text  # 获取头部
    except Exception as e:
        return e



if __name__ == '__main__':
    url = "http://baidu.com"
    print(getHTMLTEXT(url))
```





![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190820210617487-285127439.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190820210623452-501426088.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190820210629448-921647999.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190820210658550-187571985.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190820210704626-460154845.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190820210710277-686834483.png)



# 爬虫

网络爬虫的流程：

1. 获取网页：获取网页就是给一个网页发送请求，该网页会返回整个网页的数据，类似在浏览器中键入网站并且按回车键，然后就可以看到网站的整个内容。

2. 解析网页(提取所需要的信息)：就是从整个网页的数据中提取想要的数据，类似在浏览去中看到网站的整个网页，但是你需要提取你需要的信息

3. 存储信息：一般可以存储在csv或者数据库中。

   

网络爬虫的技术实现：

1. 获取网页：
   1. 获取网页的基础技术：requests、urllib和selenium(模拟浏览器也可以使用谷歌模拟)
   2. 获取网页的进阶技术：多进程多线程抓取，登陆抓取，突破IP封禁和服务器抓取。
2. 解析网页：
   1. 解析网页的基础技术：re正则表达式，Beautiful Soup和lxml
   2. 解析网页的进阶技术：解决中文乱码问题
3. 存储数据：
   1. 存储数据的基础技术：存入txt文件中和存入csv文件中
   2. 存储数据的进阶技术：存入Mysql数据库中和存入MongoDB数据库中





掌握定向网络数据爬取和网页解析的基本能力





将网站当成API

# requests



1. 自动爬取html页面，自动网络请求提交

2. 最优秀的爬虫库

3. | 方法               | 说明                                           |
   | ------------------ | ---------------------------------------------- |
   | requests.request() | 构造一个请求，支撑以下各方法的基础方法         |
   | requests.get()     | 获取HTML网页的主要方法，对应HTTP的GET          |
   | requests.head()    | 获取HTML网页头信息的方法，对应于HTTP的HEAD     |
   | request.post()     | 向HTML网页提交POST请求的方法，对应于HTTP的POST |
   | request.put()      | 向HTML网页提交PUT请求的方法，对应于HTTP的PUT   |
   | request.patch()    | 向HTML网页提交局部修改请求，对应HTTP的PATCH    |
   | requests.delete()  | 向THTML网页提交删除请求，对应HTTP的DELETE      |

    

   1. requests.get(url,params = None,**kwargs)
      1. url:网页的url链接
      2. params:url的额外参数，字典或者字节流格式，是可选择的
      3. **kwargs：12个控制访问的参数，是可选择的。

4. Requests库的2的重要对象

   1. r = requests.get(url)

   2. Response(包含了爬虫返回的所有内容)

   3. | Response对象的属性  | 说明                                                         |
      | ------------------- | ------------------------------------------------------------ |
      | r.status_code       | ＨＴＴＰ请求的返回状态，200表示链接成功，404表示失败（一般来说除了200，其余都是失败的） |
      | r.text              | HTTP响应内容的字符串形式，即，url对应的页面内容              |
      | r.encoding          | 从HTTPheader中猜测的响应内容编码方式                         |
      | r.apparent_encoding | 从内容中分析出响应内容的编码方式(备选编码方式)               |
      | r.content           | HTTP响应内容的二进制形式(主要是用于图片等)                   |

      

   4. Request

   5. r.status_code   状态码，如果为200，证明能爬虫成功，其余失败

   6. r.headers        返回get请求获得页面的头部信息

   7. 如果r.header中不存在charset，可以认为编码方式维ISO-8859-1(这个可以通过r.encoding获得)

   8. 如果出现乱码，可以用r.apparent_encoding获得备用编码，然后用r.encoding = '获得的编码',也可以直接尝试r.encoding = 'utf-8'

5. Requests异常

   ![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819202225243-252006079.png)

6. | 异常                 | 说明                                    |
   | -------------------- | --------------------------------------- |
   | r.raise_for_status() | 如果不是200，产生异常requests.HTTPError |

```python
#检测代码是否可以爬取
import requests


def getHTMLTEXT(url):
    try:
        r = requests.get(url,timeout = 30)
        r.raise_for_status()   #如果状态码不是200，会出错
        r.encoding = r.apparent_encoding
        return r.text  # 获取头部
    except Exception as e:
        return e



if __name__ == '__main__':
    url = "http://baidu.com"
    print(getHTMLTEXT(url))
```





![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211318216-1933901540.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211325012-533534287.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211330562-1118240644.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211430544-1638282944.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211501879-518730779.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211507297-1635553049.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211512975-1266521431.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211547820-599443451.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211553163-10786160.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211600707-461131669.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211642092-1208663332.png)



![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211655488-2072104933.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211701331-22490794.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211743951-2033416648.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211749939-1427703756.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211754890-1780613461.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211802093-223375818.png)

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190819211648653-715858014.png)





