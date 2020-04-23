1. 安装库

   ```
   #Windows平台
       1、pip3 install wheel #安装后，便支持通过wheel文件安装软件，wheel文件官网：https://www.lfd.uci.edu/~gohlke/pythonlibs
       3、pip3 install lxml
       4、pip3 install pyopenssl
       5、下载并安装pywin32：https://sourceforge.net/projects/pywin32/files/pywin32/
       6、下载twisted的wheel文件：http://www.lfd.uci.edu/~gohlke/pythonlibs/#twisted
       7、执行pip3 install 下载目录\Twisted-17.9.0-cp36-cp36m-win_amd64.whl
       8、pip3 install scrapy
   
   #Linux平台
       1、pip3 install scrapy
   ```

2. 创建爬虫程序

   ```
   scrapy genspider 程序名 域名（爬取网站的域名）
   scrapy genspider tmall www.tmall.com
   
   ```

3. 修改settings

   ```
   # root协议--君子协议
   # ROBOTSTXT_OBEY = True
   # 不遵循
   ROBOTSTXT_OBEY = False
   ```

4. 创建run.py

   ```
   # @Author : SkyOceanchen # @File : run.py
   #在项目目录下新建：run.py
   from scrapy.cmdline import execute
   # --nolog 控制日志的打印
   # execute(['scrapy', 'crawl', 'tmall','-a','pro=男装','--nolog']) #没有日志
   # execute(['scrapy', 'crawl', 'tmall','-a','pro=男装'])
   execute(['scrapy', 'crawl', 'tmall','--nolog'])
   ```
   
5. 爬虫代码

   ```
   # -*- coding: utf-8 -*-
   import scrapy
   from urllib.parse import urlencode
   from firstscrapy import items
   # class TmallSpider(scrapy.Spider):
   #     name = 'tmall'
   #     # 域名
   #     allowed_domains = ['www.tmall.com']
   #     start_urls = ['https://www.tmall.com/']
   #     # 局部配置
   #     custom_settings = {
   #
   #         'DEFAULT_REQUEST_HEADERS': {
   #             'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
   #             'Accept-Language': 'en',
   #             'User-Agent': "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.70 Safari/537.36"
   #         }
   #
   #     }
   #
   #     # 重写请求
   #
   #     def start_requests(self):
   #         yield scrapy.Request(url=self.start_urls[0], callback=self.parse, errback=self.dealerr, dont_filter=True)
   #     #     callback=self.parse 回调函数
   #     # errback=self.dealerr, 报错信息
   #     # dont_filter=True 不去重
   #
   #
   #
   #     def parse(self, response):
   #         print('进入解析')
   #         pass
   #
   #     def dealerr(self, msg):
   #         print(msg)
   class TmallSpider(scrapy.Spider):
       name = 'tmall'
       # 域名
       allowed_domains = ['www.tmall.com']
       start_urls = ['https://www.tmall.com/']
   
   
   
       # 重写请求
       def __init__(self,*args,**kwargs):#在实例化的时候会传入很多参数
           super(TmallSpider, self).__init__(*args,**kwargs)
           self.api = "http://list.tmall.com/search_product.htm?"
   
       def start_requests(self):
           for q in self.qs:
               self.param = {
                   "q": q,
                   "totalPage": 1,
                   'jumpto': 1,
               }
               url = self.api + urlencode(self.param)
               yield scrapy.Request(url=url,callback=self.gettotalpage,dont_filter=True)
   
   
       def gettotalpage(self, response):
           totalpage = response.css('[name="totalPage"]::attr(value)').extract_first()
           self.param['totalPage'] = int(totalpage)
           for i in range(1,self.param['totalPage']+1):
           # for i in range(1,3):
               self.param['jumpto'] = i
               url  = self.api + urlencode(self.param)
               yield scrapy.Request(url=url,callback=self.get_info,dont_filter=True)
       def get_info(self,response):
           product_list = response.css('.product')
           for product in product_list:
               title = product.css('.productTitle a::attr(title)').extract_first()
               price = product.css('.productPrice em::attr(title)').extract_first()
               status = product.css('.productStatus em::text').extract_first()
               # print(title,price,status)
               item = items.FirstscrapyItem()
               item['title'] = title
               item['price'] = price
               item['status'] = status
               yield item
       def dealerr(self, msg):
           print(msg)
   
   ```

6. 在items.py函数

   ```
   接收传递过来的值
   import scrapy
   
   class FirstscrapyItem(scrapy.Item):
       # define the fields for your item here like:
       # name = scrapy.Field()
       title = scrapy.Field()
       price = scrapy.Field()
       status = scrapy.Field()
   ```

7. 修改settings.py

   ```
   # 链接mongo
   HOST = '127.0.0.1'
   PORT = 27017
   DB = 'tmall'
   TABLE = 'products'
   
   
   # 打开管道中间件，这里用于存储
   ITEM_PIPELINES = {
      'firstscrapy.pipelines.FirstscrapyPipeline': 300,# 数值越小，优先级越高
      'firstscrapy.pipelines.FirstscrapyPipeline1': 500,# 数值越小，优先级越高
   }
   ```

8. 继续传递到pipelines.py

   ```
   # 存储
   import pymongo
   import json
   
   
   class FirstscrapyPipeline(object):
       def __init__(self, host, post, db, table):
           self.host = host
           self.port = post
           self.db = db
           self.table = table
   
       @classmethod
       # 先执行from_crawler函数，然后执行__init__初始化
       def from_crawler(cls, crawl):
           port = crawl.settings.get("PORT")
           host = crawl.settings.get("HOST")
           db = crawl.settings.get("DB")
           table = crawl.settings.get("TABLE")
           return cls(host, port, db, table)
       # 打开数据库
       def open_spider(self,crawl):
           self.client=pymongo.MongoClient(port=self.port,host=self.host)
           db_obj = self.client[self.db]
           self.table_obj = db_obj[self.table]
       # 关闭数据库
       def close_spider(self,crawl):
           self.client.close()
   
       def process_item(self, item, spider):
           self.table_obj.insert(dict(item))
           print(item['title'],'存储成功')
           return item
   class FirstscrapyPipeline1(object):
       def open_spider(self, crawl):
           self.f = open("xxx.txt","at",encoding="utf-8")
   
       def close_spider(self, crawl):
           self.f.close()
   
       def process_item(self, item, spider):
   
           self.f.write(json.dumps(dict(item)))
   
           return item
   ```

9. 修改settings.py

   ```
   SPIDER_MIDDLEWARES = {
      'firstscrapy.middlewares.FirstscrapySpiderMiddleware': 543,
   }
   
   # 下载中间件打开
   DOWNLOADER_MIDDLEWARES = {
      'firstscrapy.middlewares.FirstscrapyDownloaderMiddleware': 300,# 数值越小，优先级越高
      'firstscrapy.middlewares.FirstscrapyDownloaderMiddleware1': 543,# 数值越小，优先级越高
   }
   
   #异常中间件可以选择打开
   EXTENSIONS = {
      'scrapy.extensions.telnet.TelnetConsole': None,
   }
   
   ```

10. middle

    ```
    class FirstscrapySpiderMiddleware(object):
        # Not all methods need to be defined. If a method is not defined,
        # scrapy acts as if the spider middleware does not modify the
        # passed objects.
    
        @classmethod
        def from_crawler(cls, crawler):
            # This method is used by Scrapy to create your spiders.
            s = cls()
            crawler.signals.connect(s.spider_opened, signal=signals.spider_opened)
            return s
    
        def process_spider_input(self, response, spider):
            # Called for each response that goes through the spider
            # middleware and into the spider.
    
            # Should return None or raise an exception.
            return None
    
        def process_spider_output(self, response, result, spider):
            # Called with the results returned from the Spider, after
            # it has processed the response.
    
            # Must return an iterable of Request, dict or Item objects.
            for i in result:
                yield i
    
        def process_spider_exception(self, response, exception, spider):
            # Called when a spider or process_spider_input() method
            # (from other spider middleware) raises an exception.
    
            # Should return either None or an iterable of Request, dict
            # or Item objects.
            pass
    
        def process_start_requests(self, start_requests, spider):
            # Called with the start requests of the spider, and works
            # similarly to the process_spider_output() method, except
            # that it doesn’t have a response associated.
    
            # Must return only requests (not items).
            for r in start_requests:
                yield r
    
        def spider_opened(self, spider):
            spider.logger.info('Spider opened: %s' % spider.name)
    ```

    # 重点

    ```
    # -*- coding: utf-8 -*-
    
    # Define here the models for your spider middleware
    #
    # See documentation in:
    # https://docs.scrapy.org/en/latest/topics/spider-middleware.html
    
    from scrapy import signals
    from scrapy import Request
    from scrapy.http import Response
    import requests
    
    def get_proxy():
        return requests.get("http://127.0.0.1:5010/get/").text
    
    def delete_proxy(proxy):
        requests.get("http://127.0.0.1:5010/delete/?proxy={}".format(proxy))  # ip：prot
    
    class FirstscrapyDownloaderMiddleware(object):
        # Not all methods need to be defined. If a method is not defined,
        # scrapy acts as if the downloader middleware does not modify the
        # passed objects.
        @classmethod
        def from_crawler(cls, crawler):
            # This method is used by Scrapy to create your spiders.
            s = cls()
            crawler.signals.connect(s.spider_opened, signal=signals.spider_opened)
            return s
    
        def process_request(self, request, spider):
            # 添加代理ip  添加响应
            request.meta['Download_timeout'] = 10
            request.meta['proxy'] = "http://" + get_proxy()
            # Called for each request that goes through the downloader
            # middleware.
    
            # Must either:
            # - return None: continue processing this request
            # - or return a Response object
            # - or return a Request object
            # - or raise IgnoreRequest: process_exception() methods of
            #   installed downloader middleware will be called
            return None
    
        def process_response(self, request, response, spider):
            # Called with the response returned from the downloader.
    
            # Must either;
            # - return a Response object
            # - return a Request object
            # - or raise IgnoreRequest
            return response
    
        def process_exception(self, request, exception, spider):
            # Called when a download handler or a process_request()
            # (from other downloader middleware) raises an exception.
    
            # Must either:
            # - return None: continue processing this exception
            # - return a Response object: stops process_exception() chain
            # - return a Request object: stops process_exception() chain
            # 当代理ip被封号后，执行删除代理IP 进行替换
            old_ip = request.meta['proxy'].split("//")[-1]  # http://ip:port
            delete_proxy(old_ip)
            request.meta['proxy'] = "http://" + get_proxy()
    
            return Request(url=request.url, dont_filter=True)
    
        def spider_opened(self, spider):
            spider.logger.info('Spider opened: %s' % spider.name)
    
    ```

11. 添加代理ip

    ```
    
    ```

    
