[cookies池的搭建：https://github.com/Python3WebSpider/cookiesPool](https://github.com/Python3WebSpider/cookiesPool)

python爬虫-搭建cookies池
前段时间跟着静觅大神学习了自建ip代理池，
但是很多情况下，页面的某些信息需要登录才能查看。
所以，今天又和大神学习了cookies池的搭建。

整体思路
Cookies池的架构和代理池类似，同样是4个核心模块（存储模块、生成模块、检测模块和接口模块）：

存储模块，负责存储每个账号的用户名、密码以及每个账号对应的Cookies信息，同时还需要提供一些方法来实现方便的存取操作。
生成模块，负责生成新的Cookies。此模块会从存储模块逐个拿取账号的用户名和密码，然后模拟登录目标页面，判断登录成功，就将Cookies返回并交给存储模块存储。
检测模块，定时检测数据库中的Cookies。在这里我们需要设置一个检测链接，不同的站点检测链接不同，检测模块会逐个拿取账号对应的Cookies去请求链接，如果返回的状态是有效的，那么此Cookies没有失效，否则Cookies失效并移除。接下来等待生成模块重新生成即可。
接口模块，需要用API来提供对外服务的接口。由于可用的Cookies可能有多个，我们可以随机返回Cookies的接口，这样保证每个Cookies都有可能被取到。Cookies越多，每个Cookies被取到的概率就会越小，从而减少被封号的风险。
以上就是Cookies池的框架，下面我们一一来实现各个模块吧。

# 存储模块

需要存储的内容包括账号信息与Cookies信息。

账号由用户名和密码两部分组成，我们可以存成用户名和密码的映射。Cookies可以存成JSON字符串，但是我们后面得需要根据账号来生成Cookies。生成的时候我们需要知道哪些账号已经生成了Cookies，哪些没有生成，所以需要同时保存该Cookies对应的用户名信息，其实也是用户名和Cookies的映射。

这里就是两组映射，Redis的Hash有天然的优势，所以这里我们就建立两个Hash。Hash的Key就是账号，Value对应着密码或者Cookies。

由于Cookies池需要可扩展，存储的账号和Cookies可能不止一个网站，所以这里Hash的名称可以做二级分类。以马蜂窝为例，存放账号的Hash名称可以为accounts:mafeng，存放Cookies的Hash名称为cookies:mafeng。这样方便扩展。

接下来我们创建一个存储模块类（saver.py）：



```python
import random
import redis

REDIS_HOST = 'localhost'
REDIS_PORT = 6379
REDIS_PASSWORD = None

class RedisClient(object):
    def __init__(self,type,website,host=REDIS_HOST,port=REDIS_PORT,password=REDIS_PASSWORD):
        self.db = redis.StrictRedis(host=host,port=port,password=password,decode_responses=True)
        self.type = type
        self.website = website

    def name(self):
        return "{type}:{website}".format(type=self.type,website=self.website)

    def set(self,username,value):
        '''
        设置键值对
        :param username: 用户名
        :param value: 密码或cookies
        :return:
        '''
        return self.db.hset(self.name(),username,value)

    def get(self,username):
        '''
        根据键名获取键值
        :param username: 用户名
        :return:
        '''
        return self.db.hget(self.name(),username)

    def delete(self,username):
        '''
        根据键名删除键值对
        :param username: 用户名
        :return: 删除结果
        '''
        return self.db.hdel(self.name(),username)

    def count(self):
        '''
        获取数目
        :return:数目
        '''
        return self.db.hlen(self.name())

    def random(self):
        '''
        随机获得键值，用户随机cookies获取
        :return: 随机Cookies
        '''
        return random.choice(self.db.hvals(self.name()))

    def usernames(self):
        '''
        获取所有账户信息
        :return: 所有用户名
        '''
        return self.db.hkeys(self.name())

    def all(self):
        '''
        获取所有键值对
        :return: 用户名和密码或cookies的映射表
        '''
        return self.db.hgetall(self.name())



```



这里要注意的是，name()方法拼接了type和website,组成Hash的名称。

另外，我们还需要一个导入数据的方法，便于批量导入用户名与密码。
（听说淘宝上可以买账号，我没买，所以只导入了一个仅有的账号）
具体代码如下（importer.py）：

```python
from saver import RedisClient

conn = RedisClient('accounts', 'mafeng')

def set(account, sep='----'):
    username, password = account.split(sep)
    result = conn.set(username, password)
    print('账号', username, '密码', password)
    print('录入成功' if result else '录入失败')


def scan():
    print('请输入账号密码组, 输入exit退出读入')
    while True:
        account = input()
        if account == 'exit':
            break
        set(account)


if __name__ == '__main__':
    scan()

```



# 生成模块

生成模块负责获取各个账号信息并模拟登录，随后生成Cookies并保存。我们首先获取两个Hash的信息，看看账户的Hash比Cookies的多了哪些还没有生成Cookies的账号，然后将剩余的账号遍历，再去生成Cookies即可。

这里我们对接的是马蜂窝网站，下面是生成模块的主要代码（getter.py）





```python
import json
from selenium import webdriver
from saver import RedisClient
from cookies.mafengcookies import MafengCookies


class CookiesGenerator(object):
    def __init__(self,website='default'):
        self.website = website
        self.cookies_db = RedisClient('cookies',self.website)
        self.account_db = RedisClient('accounts',self.website)
        self.init_browser()

    def __del__(self):
        self.close()

    def init_browser(self):
        self.option = webdriver.ChromeOptions()
        self.option.add_argument('headless')
        self.browser = webdriver.Chrome(executable_path='F:\BaiduNetdiskDownload\cookie_pool\chromedriver',chrome_options=self.option)

    def new_cookies(self,username,password):
        '''
        新生成Cookies,子类需要重写
        :param username: 用户名
        :param password: 密码
        :return:
        '''
        raise NotImplementedError

    def process_cookies(self,cookies):
        '''
        处理Cookies
        :param cookies:
        :return:
        '''
        dict = {}
        for cookie in cookies:
            dict[cookie['name']] = cookie['value']
        return dict

    def run(self):
        accounts_usernames = self.account_db.usernames()
        cookies_usenames = self.cookies_db.usernames()

        #找出没有Cookies的账号
        for username in accounts_usernames:
            if not username in cookies_usenames:
                password = self.account_db.get(username)
                print('正在生成Cookies','账号',username,'密码','---')
                result = self.new_cookies(username,password)

                if result.get('status') == 1:
                    cookies = self.process_cookies(result.get('content'))
                    print('成功获取到Cookies',cookies)
                    if self.cookies_db.set(username,json.dumps(cookies)):
                        print('成功保存Cookies')
                #密码错误，移除账号
                elif result.get('status') == 2:
                    print(result.get('content'))
                    if self.account_db.delete(username):
                        print('成功删除帐号')
                else:
                    print(result.get('content'))
        else:
            print('所有账号都已经成功获取Cookies')

    def close(self):
        try:
            print('Closing Browser')
            self.browser.close()
            del self.browser
        except TypeError:
            print('Brower not opened')


class MafengCookiesGenerator(CookiesGenerator):
    def __init__(self,website='mafeng'):
        CookiesGenerator.__init__(self,website)
        self.website = website
    def new_cookies(self,username,password):
        return MafengCookies(username,password,self.browser).main()


```



不过现在还需要加一个获取马蜂窝网站Cookies的类，并针对不同的情况返回不同的结果。
具体代码如下所示（cookies.mafengcookies.py）:

```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
import selenium.common.exceptions as ex
import time


class MafengCookies():
    def __init__(self,username,password,browser):
        self.url = 'https://passport.mafengwo.cn/'
        self.browser = browser
        self.wait = WebDriverWait(self.browser,10)
        self.username = username
        self.password = password

    def open(self):
        '''
        打开网页并输入用户名与密码
        :return:
        '''
        self.browser.get(self.url)
        username = self.wait.until(EC.presence_of_element_located((By.XPATH,'//form[@id="_j_login_form"]//input[@name="passport"]')))
        password = self.wait.until(EC.presence_of_element_located((By.XPATH,'//form[@id="_j_login_form"]//input[@name="password"]')))
        submit = self.wait.until(EC.presence_of_element_located((By.XPATH,'//form[@id="_j_login_form"]//div[@class="submit-btn"]/button')))
        username.send_keys(self.username)
        password.send_keys(self.password)
        time.sleep(2)
        submit.click()

    def password_error(self):
        '''
        判断密码是否错误
        :return:
        '''
        try:
            return bool(
                WebDriverWait(self.browser,10).until(EC.presence_of_element_located((By.XPATH,'//div[@class="alert alert-danger"]')))
            )
        except ex.TimeoutException:
            return False

    def get_cookies(self):
        '''
        获取cookies
        :return:
        '''
        return self.browser.get_cookies()

    def login_successfully(self):
        '''
        判断是否登录成功
        :return:
        '''
        try:
            return bool(
                WebDriverWait(self.browser,10).until(EC.presence_of_element_located((By.XPATH,'//div[@class="user-image"]')))
            )
        except ex.TimeoutException:
            return False

    def main(self):
        self.open()
        time.sleep(5)
        if self.password_error():
            return{
                'status':2,
                'content':'用户名或密码错误'
            }
        elif self.login_successfully():
            cookies = self.get_cookies()
            return {
                'status':1,
                'content':cookies
            }
        else:
            return{
                'status':3,
                'content':'登录失败'
            }


```



# 检测模块

我们现在可以用生成模块来生成Cookies，但是还是免不了Cookies失效的问题，例如时间太长或者Cookies使用太频繁导致无法正常请求网页。

如果遇到这样的Cookies，我们肯定不能让它继续保存在数据库里了。

所以，我们还需要增加一个定时检测模块，负责遍历池中的所有Cookies，同事设置好对应的检测链接，我们用一个个Cookies去请求这个链接。如果检测失效，就将该Cookies从数据库中移除。

为了实现通用可扩展性，我们还定义了一个检测器的父类，声明了一些通用组件实现代码如下所示（tester.py）

```python
from saver import RedisClient
import json
import requests

TEST_URL_MAP = {
    'mafeng':'http://www.mafengwo.cn/friend/index/follow'
}

class ValidTester(object):
    def __init__(self,website='default'):
        self.website = website
        self.cookies_db = RedisClient('cookies',self.website)
        self.accounts_db = RedisClient('accounts',self.website)
        self.header = {
            'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8',
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 '
                          '(KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36'
        }

    def test(self,username,cookies):
        raise NotImplementedError

    def run(self):
        cookies_groups = self.cookies_db.all()
        for username,cookies in cookies_groups.items():
            self.test(username,cookies)

class MafengValidTester(ValidTester):
    def __init__(self, website='mafeng'):
        ValidTester.__init__(self,website)

    def test(self,username,cookies):
        print('正在测试Cookies','用户名',username)
        try:
            cookies = json.loads(cookies)
        except TypeError:
            print('Cookies不合法',username)
            self.cookies_db.delete(username)
            print('删除Cookies',username)
            return
        try:
            test_url = TEST_URL_MAP[self.website]
            response = requests.get(test_url,headers=self.header,cookies=cookies,timeout=5, allow_redirects=False)
            if response.status_code == 200:
                print('Cookies有效',username)
            else:
                print(response.status_code,response.headers)
                print('Cookies失效',username)
                self.cookies_db.delete(username)
                print('删除Cookies',username)
        except ConnectionError as e:
            print('发生异常',e.args)

```



# 接口模块

生成模块和检测模块如果定时运行，就可以完成Cookies实时检测和更新。

但是Cookies需要给爬虫来用，同时一个Cookies池可供多个爬虫使用，所以我们还需要定义一个Web接口。

我们采用Flask来实现接口的搭建，代码如下所示（api.py）：

```python
import json
from flask import Flask,g
from saver import *

GENERATOR_MAP = {
    'mafeng':'http://www.mafengwo.cn/friend/index/follow'
}

__all__ = ['app']

app = Flask(__name__)

@app.route('/')
def index():
    return '<h2>Welcome to Cookie Pool System</h2>'

def get_conn():
    for website in GENERATOR_MAP:
        print(website)
        #g是个全局对象
        if not hasattr(g,website):
            setattr(g,website+'_cookies',eval('RedisClient'+'("cookies","'+website+'")'))
            setattr(g, website + '_accounts', eval('RedisClient' + '("accounts","' + website + '")'))
        return g

@app.route('/<website>/random')
def random(website):
    '''
    获取随机的cookie,访问地址如/zhihu/random
    :param website:站点
    :return:随机cookie
    '''
    g = get_conn()
    cookies = getattr(g,website+'_cookies').random()
    return cookies

def add(website,username,password):
    '''
    添加用户，访问地址如/mafeng/add/user/password
    :param website: 站点
    :param username: 用户名
    :param password: 密码
    :return:
    '''
    g = get_conn()
    print(username,password)
    getattr(g,website+'_accounts').set(username,password)
    return json.dumps({'status':'1'})

@app.route('/<website>/count')
def count(website):
    '''
    获取cookies总数
    :param website:
    :return:
    '''
    g = get_conn()
    count = getattr(g,website+'_cookies').count()
    return json.dumps({'status': '1', 'count': count})

```



# 调度模块

最后，我们再加上一个调度模块让这几个模块配合运行起来，主要的工作就是驱动几个模块定时运行，同时各个模块需要在不同进程上运行，实现如下所示（scheduler.py）：



```python
import time
from tester import *
from saver import *
from getter import *
from api import *
from multiprocessing import Process

API_HOST = '127.0.0.1'
API_PORT = 5000
# 产生器和验证器循环周期
CYCLE = 120
TESTER_MAP = {
    'mafeng':'MafengValidTester'
}
GENERATE_MAP = {
    'mafeng':'MafengCookiesGenerator'
}
# 产生器开关，模拟登录添加Cookies
GENERATOR_PROCESS = True
# 验证器开关，循环检测数据库中Cookies是否可用，不可用删除
VALID_PROCESS = True
# API接口服务
API_PROCESS = True


class Scheduler(object):
    @staticmethod
    def valid_cookie(cycle=CYCLE):
        while True:
            print('Cookies检测进程开始运行')
            try:
                for website,cls in TESTER_MAP.items():
                    tester = eval(cls + '(website="'+ website +'")')
                    tester.run()
                    print('Cookies检测完成')
                    del tester
                    time.sleep(cycle)
            except Exception as e:
                print(e.args)

    @staticmethod
    def generate_cookie(cycle=CYCLE):
        while True:
            print('Cookies生成进程开始运行')
            try:
                for website, cls in GENERATE_MAP.items():
                    generator = eval(cls + '(website="' + website + '")')
                    generator.run()
                    print('Cookies生成完成')
                    generator.close()
                    time.sleep(cycle)
            except Exception as e:
                print(e.args)

    @staticmethod
    def api():
        print('API接口开始运行')
        app.run(host=API_HOST,port=API_PORT)

    def run(self):
        if API_PROCESS:
            api_process = Process(target=Scheduler.api)
            api_process.start()
        if GENERATOR_PROCESS:
            generate_process = Process(target=Scheduler.generate_cookie)
            generate_process.start()
        if VALID_PROCESS:
            valid_process = Process(target=Scheduler.valid_cookie)
            valid_process.start()

```



运行效果
至此，我们的Cookies就全部完成了。接下来我们将数据（用户名、密码）导入，并将模块同时开启，启动调度器，控制台运行效果如下：


通过接口访问：


通过接口随机获取Cookies：
