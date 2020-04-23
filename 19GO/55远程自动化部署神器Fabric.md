## 一 Fabric介绍

fabric是一个Python的库，同时它也是一个命令行工具。使用fabric提供的命令行工具，可以很方便地执行应用部署和系统管理等操作。

fabric依赖于paramiko进行ssh交互，fabric的设计思路是通过几个API接口来完成所有的部署，因此fabric对系统管理操作进行了简单的封装，比如执行命令，上传文件，并行操作和异常处理等。

    
    
    平时我们的开发流程是这样，经过几个月奋战，项目终于开发完了，测试也没问题了，我们就把代码提交到 GitHubm准备部署到正式环境。
    你ssh登录到正式服务器，进入到项目目录中，把代码从远程仓库拉下来，然后启动程序。
    以后每次发布版本，都要执行重复的操作，登录服务器，切换到指定目录，拉取代码，重启服务。
    
    其实这种操作非常繁琐，也没什么技术含量，还容易出问题，于是 Fabric 出场了。Fabric 是一个远程部署神器，它可以在本地执行远程服务器的命令

## 二 版本问题

    
    
    注意，如果你安装的是旧版的 Fabric，那么新版的 Fabric 是不兼容旧版的，
    目前 Fabric 有三个版本，
    Fabric1 就是以前的 Fabric，只支持 Python2，已不推荐使用
    而 Fabric2 就是现在的 Fabric，同时支持 Python2 和 Python3， 也是官方强烈推荐的版本， 
    还有一个 Fabric3，这是网友从旧版的 Fabric1 克隆过来的非官方版本，但是兼容 Fabric1，也支持 Python2 和 Python3。
    
    最新的 Fabric 不需要 fabfile.py 文件， 也不需要 fab 命令，而现在网络上几乎所有的教程、资料都还是基于 fabric1 写的，当你在看那些教程的时候，注意甄别。 而新版 Fabric 提供的 API 非常简单
    #我们使用fabric2
    

## 三 管理单台服务器

安装：pip3 install fabric2

    
    
    from fabric2 import Connection
    
    def deploy():
        # 如果你的电脑配了ssh免密码登录，就不需要 connect_kwargs 来指定密码了。
        conn = Connection("root@172.16.209.100", connect_kwargs={"password": "123"})
        conn.run('ls')  # 执行ls命令
    
        with conn.cd('/home'):  # 切换到/home目录下
            conn.run("mkdir testdir")  # 在home目录下创建testdir文件夹
        with conn.cd('/home/testdir'):  # 切换到/home/testdir目录
            # conn.sudo('mkdir aaa') #
            conn.run('mkdir aaa')  # 在该目录下创建aaa文件夹
            conn.put('db.sqlite3', '/home/testdir')  # 将当前目录下db.sqlite3文件上传到/home/testdir目录下
    
    
    if __name__ == '__main__':
        deploy()
    

### 3.1 Connection参数

    
    
      def __init__(
    　　　　self,
    　　　　host,                       #主机ip
    　　　　user=None,                  #用户名
    　　　　port=None,                  #ssh端口，默认是22
    　　　　config=None,                #登录配置文件
    　　　　gateway=None,               #连接网关
    　　　　forward_agent=None,         #是否开启agent forwarding
    　　　　connect_timeout=None,       #设置超时
    　　　　connect_kwargs=None,        #设置 密码登录connect_kwargs={"password": "123456"}) 还是 密钥登录connect_kwargs={"key_filename": "/home/myuser/.ssh/private.key"}
    　　　　inline_ssh_env=None,      
    　　　　)

## 3.2 conn对象属性

    
    
    run：                #执行远程命令，如：run('uname -a')
    cd：                 #切换远程目录，如：cd('/root');   with conn.cd('/root'):继承这个状态
    put：                #上传本地文件到远程主机，如：put('/root/test.py','/root/test.py')
    get:                 #获取服务器上文件，如：get('/root/project/test.log')
    sudo：               #sudo方式执行远程命令，如：sudo('service docker start')

## 四 管理多台服务器

    
    
    #很多情况下，我们需要同时操作多台服务器。最直接的办法自然是通过迭代的方法遍历整个主机列表。
    
    from fabric2 import Connection
    for host in ('root@172.16.209.100', 'root@172.16.209.101', 'root@172.16.209.102'):
         result = Connection(host,connect_kwargs={'password':'123'}).run('uname -s')
         print("{}: {}".format(host, result.stdout.strip()))
    
    
    from fabric2 import SerialGroup
    
    pool = SerialGroup('root@172.16.209.100', 'root@172.16.209.101', 'root@172.16.209.102',connect_kwargs={'password':'123'})
    print(pool)
    pool.run('uname -s')
    for conn in pool:
        conn.run('mkdir laqzzz')

## 五 执行本地命令

    
    
    #通过invoke模块
    import invoke
    invoke.run('ls')
    
    #通过local命令
    from fabric2 import Connection
    cn = Connection('root@172.16.209.100',connect_kwargs={'password':'123'})
    cn.run('ls -al')     
    cn.sudo('whoami')    
    cn.local('ls')

