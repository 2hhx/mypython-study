# 老男孩上海py11代码发布直播课

## 昨日内容回顾

* ### django channels模块

  ```python
  """
  django默认只支持http协议
  """
  ###############前期基础配置#################
  # 1.配置文件中注册channels应用
  INSTALLED_APPS = [
  	'channels'
  ]
  
  # 2.配置文件再配置一个参数
  ASGI_APPLICATION = '项目名同名的文件夹名.routing.application'
  
  # 3.去项目名同名的文件夹下创建routing.py文件，文件内定义application变量名
  """参考昨日笔记拷贝"""
  
  # 上述三个步骤配置完成后 启动django会由原来的wsgiref切换成asgi启动(达芙妮)
  """
  原来基于http协议的请求响应还是再urls.py和views.py中
  新的wesocket协议请求响应则再routing.py和consumers.py中
  """
  
  # consumer.py中 常见方法
  class XXXConsumer(WebScoketConsumer):
  	def websocket_connect(self,message):
      """客户端请求链接自动触发"""
     	self.accept()
    
    def websocket_receive(self,message):
      """客户端发送消息自动触发"""
  		self.send('我给你回消息')
    
    def websocket_disconnect(self,message):
      """客户端断开链接之后自动触发"""
      raise StopConsumer()  # 内部自动捕获
  ```

  前端需要掌握的代码

  ```html
  <script>
  	var ws = new WebSocket('ws://127.0.0.1:8080/');
    
    // 请求链接成功自动触发
    ws.onopen = function(){}
    
    // 发送数据
    ws.send('数据')
  
    // 服务端回消息自动接收触发
    ws.onmessage = function(){}
    
    // 主动断开链接
    ws.close()
  </script>
  ```

* ### 基于channels模块实现简易版本的群聊功能

  ```python
  self就是来的客户端链接
  简单粗暴的处理方式 自己维护一个对象列表 之后循环列表给每个对象发送数据
  ```

* ### gojs前端插件

  ```python
  """
  三个基本文件
  	gojs.js  基本都用它
  	go-debug.js  了解即可 上线一般不用
  	Figures.js  额外的扩展图形
  
  使用的规律
  	先用div占位，之后所有的操作全在div中进行，div类似于画板
  """
  ```

  **重要概念**

  * TextBlock
  * Shape
  * Node
  * Link

  每一个基本使用，查看案例，无需掌握，知道主要参数的含义即可

  需要掌握的是如何跟后端数据进行交互，实现图标的动态变化

  **数据绑定**

  需要你拷贝到自己的笔记或者博客中，之后直接使用即可

  提前将节点和箭头生成模版，之后只需要将对应的数据传入模版中即可

  数据我们为了方便与后端交互，构造成了列表套字典的形式

  **去除水印**

  查找固定的一个字符串，注释调该字符串所在的一行代码

  添加一行新的代码

  ```js
  a.kr = function(){return true};
  ```

* ### paramiko模块

  既可以操作服务器执行命令也可以上传下载文件

  即支持用户名密码的方式也支持公钥私钥的方式

  对执行命令和上传下载文件进行功能封装

  **面向对象知识点回顾**

  ```python
  class MyClass(object):
    def __enter__(self,*args,**kwargs):
      return self  # 步骤1
    
    def __exit__(self,*args,**kwargs):
      pass  # 步骤3
    
  with MyClass() as f:
    print(f)  # 步骤2
  ```

# 今日内容

* gitpython操作
* 服务器管理
* 项目管理
* 发布任务管理
* channel-layers实现发布/部署流程

任何一个项目，增删改查的业务逻辑占比非常大80%+

通用代码发布流程图参考qq群

```python
"""
ps:当服务器特别多的时候，如何快速实现上传下载？？？

目前最新最好的技术:比特流技术  p2p

将所有的下载者同时也变成上传者
联想迅雷种子下载，百度网盘下载
"""
```

# 今日内容详细

### python操作git

**安装模块**

```python
pip3 install gitpython
```

**基本使用**

```python
import os
from git.repo import Repo

# 创建本地路径用来存放远程仓库下载的代码
download_path = os.path.join('jason','NB')
# 拉取代码
Repo.clone_from('https://github.com/DominicJi/TeachTest.git',to_path=download_path,branch='master')

```

**其他常见操作**

```python
# ############## 2. pull最新代码 ##############
import os
from git.repo import Repo
 
local_path = os.path.join('jason', 'NB')
repo = Repo(local_path)
repo.git.pull()

# ############## 3. 获取所有分支 ##############
import os
from git.repo import Repo
 
local_path = os.path.join('jason', 'NB')
repo = Repo(local_path)
 
branches = repo.remote().refs
for item in branches:
    print(item.remote_head)
    
# ############## 4. 获取所有版本 ##############
import os
from git.repo import Repo
 
local_path = os.path.join('jason', 'NB')
repo = Repo(local_path)
 
for tag in repo.tags:
    print(tag.name)

# ############## 5. 获取所有commit ##############
import os
from git.repo import Repo
 
local_path = os.path.join('jason', 'NB')
repo = Repo(local_path)
 
# 将所有提交记录结果格式成json格式字符串 方便后续反序列化操作
commit_log = repo.git.log('--pretty={"commit":"%h","author":"%an","summary":"%s","date":"%cd"}', max_count=50,
                          date='format:%Y-%m-%d %H:%M')
log_list = commit_log.split("\n")
real_log_list = [eval(item) for item in log_list]
print(real_log_list)
 
# ############## 6. 切换分支 ##############
import os
from git.repo import Repo
 
local_path = os.path.join('jason', 'NB')
repo = Repo(local_path)
 
before = repo.git.branch()
print(before)
repo.git.checkout('master')
after = repo.git.branch()
print(after)
repo.git.reset('--hard', '854ead2e82dc73b634cbd5afcf1414f5b30e94a8')
 
# ############## 7. 打包代码 ##############
with open(os.path.join('jason', 'NB.tar'), 'wb') as fp:
    repo.archive(fp)

```

将上述所有的方法封装到类中以便后续的调用(后续如果你想要操作git直接拷贝使用即可)

```python
import os
from git.repo import Repo
from git.repo.fun import is_git_dir


class GitRepository(object):
    """
    git仓库管理
    """
    def __init__(self, local_path, repo_url, branch='master'):
        self.local_path = local_path
        self.repo_url = repo_url
        self.repo = None
        self.initial(repo_url, branch)

    def initial(self, repo_url, branch):
        """
        初始化git仓库
        :param repo_url:
        :param branch:
        :return:
        """
        if not os.path.exists(self.local_path):
            os.makedirs(self.local_path)

        git_local_path = os.path.join(self.local_path, '.git')
        if not is_git_dir(git_local_path):
            self.repo = Repo.clone_from(repo_url, to_path=self.local_path, branch=branch)
        else:
            self.repo = Repo(self.local_path)

    def pull(self):
        """
        从线上拉最新代码
        :return:
        """
        self.repo.git.pull()

    def branches(self):
        """
        获取所有分支
        :return:
        """
        branches = self.repo.remote().refs
        return [item.remote_head for item in branches if item.remote_head not in ['HEAD', ]]

    def commits(self):
        """
        获取所有提交记录
        :return:
        """
        commit_log = self.repo.git.log('--pretty={"commit":"%h","author":"%an","summary":"%s","date":"%cd"}',
                                       max_count=50,
                                       date='format:%Y-%m-%d %H:%M')
        log_list = commit_log.split("\n")
        return [eval(item) for item in log_list]

    def tags(self):
        """
        获取所有tag
        :return:
        """
        return [tag.name for tag in self.repo.tags]

    def change_to_branch(self, branch):
        """
        切换分值
        :param branch:
        :return:
        """
        self.repo.git.checkout(branch)

    def change_to_commit(self, branch, commit):
        """
        切换commit
        :param branch:
        :param commit:
        :return:
        """
        self.change_to_branch(branch=branch)
        self.repo.git.reset('--hard', commit)

    def change_to_tag(self, tag):
        """
        切换tag
        :param tag:
        :return:
        """
        self.repo.git.checkout(tag)


if __name__ == '__main__':
    local_path = os.path.join('codes', 'luffycity')
    repo = GitRepository(local_path,remote_path)
    branch_list = repo.branches()
    print(branch_list)
    repo.change_to_branch('dev')
    repo.pull()

```

### 服务器管理

增删改查业务逻辑

```python
"""
当默认的views.py中代码逻辑比较多的时候，也可以做拆分
拆分成文件夹的形式，内部根据业务逻辑的不同建专门的py文件与之对应

django默认的语言环境是英文的，但是它的内部是支持多国语言的，你可以修改语言配置
配置文件中

# 修改语言环境为中文
LANGUAGE_CODE = 'zh-hans'

如何查看django内部支持的语言环境
from django.conf import global_settings
"""

# modelform是forms组件的加强版 比forms组件更加的好用
# forms组件跟model没有直接的关系  而modelfrom直接跟model相关
# 用modelfrom不需要你自己一个个写字段了 只需要指定模型表和字段数量即可
# 配置完成后 其他操作跟forms组件一模一样
class ServerModelForm(ModelForm):
    # 字段是否需要加form-control配置项
    exclude_bootstrap = []

    class Meta:
        model = models.Server
        fields = '__all__'

    def __init__(self,*args,**kwargs):
        super().__init__(*args,**kwargs)
        # self.fields = {'hostname':字段对象}
        for k,field in self.fields.items():
            if k in self.exclude_bootstrap:
                continue
            field.widget.attrs['class'] = 'form-control'

```

**编辑功能**

```python
def server_edit(request,edit_id):
    edit_obj = models.Server.objects.filter(pk=edit_id).first()
    form_obj = ServerModelForm(instance=edit_obj)  # 要编辑的对象
    """
    instance参数只要你加了 那么在渲染页面的时候会自动帮你填充数据信息
    """
    if request.method == 'POST':
        form_obj = ServerModelForm(data=request.POST,instance=edit_obj)
        form_obj.save()  # 不加instance就会是新增 加了就是修改
        return redirect('server_list')

    return render(request,'server_edit.html',locals())

```

**删除功能**

删除需要做二次确认操作，之前我们写过ajax结合sweetalert实现的删除

BOM操作 弹出框 alert、prompt、confirm

```html
<a onclick="removeData(this,{{ server_obj.pk }})">删除</a>
<script>
        function removeData(ths,delete_id) {
            var res = confirm('你确定要删吗?');
            if (res){
                // 发送ajax请求删除数据
                $.ajax({
                    url:'/server/delete/'+delete_id+'/',
                    type:'get',
                    dataType:'JSON',
                    success:function (args) {
                        if(args.status){
                            // 为了保持删除之后还在当前页 我们不采用render刷新的方式而是采用DOM操作
                            // 将用户点击的删除按钮所在的行DOM操作移除
                            $(ths).parent().parent().remove();
                        }
                    }
                })
            }
        }
</script>

```

### 项目管理

```python
# models.py
class Project(models.Model):
    """项目表"""
    title = models.CharField(verbose_name='项目名',max_length=32)

    repo = models.CharField(verbose_name='仓库地址',max_length=128)

    # 针对可以列举完全的选项 基本都用choices参数来实现
    env_choices = (
        ('prod','正式'),
        ('test','测试'),
    )
    # 给项目设置测试的默认 因为项目流程必须是先测试再上线
    env = models.CharField(verbose_name='环境',max_length=16,choices=env_choices,default='test')

```

直接拷贝服务器url和视图函数的代码，修改即可

**优化**

* 公用添加页和编辑页面

* 单独开设文件夹存储项目所使用到的modelform(根据项目的 不同开设不同的py文件)

* 将modelform中相同的部分抽取出来 父类

  ```python
  from django.forms import ModelForm
  class BaseModelForm(ModelForm):
      # 字段是否需要加form-control配置项
      exclude_bootstrap = []
      def __init__(self,*args,**kwargs):
          super().__init__(*args,**kwargs)
          # self.fields = {'hostname':字段对象}
          for k,field in self.fields.items():
              if k in self.exclude_bootstrap:
                  continue
              field.widget.attrs['class'] = 'form-control'
  
  # 子类
  from app01 import models
  from app01.myforms.baseform import BaseModelForm
  class ProjectModelForm(BaseModelForm):
      class Meta:
          model = models.Project
          fields = '__all__'
  
  ```

* 给项目表添加两个必备的字段

  ```python
  # 项目上线之后 还应该有远程服务器上面的地址
      path = models.CharField(verbose_name='线上项目地址',max_length=128)
  
      # 项目与服务器应该是有关系的  项目跑在哪些服务器上  多对多  混用 多个项目跑在同一台服务器上
      servers = models.ManyToManyField(to='Server',verbose_name='关联服务器')
  
  ```

* 项目表展示增加新增的字段

  

### 发布任务

```python
class DeployTask(models.Model):
    """
    发布任务单
    项目主键            项目版本
    1                      v1
    1                      v2
    1                      v3
    2                      v1
    2                      v2
    """
    project = models.ForeignKey(to='Project',verbose_name='项目')
    tag = models.CharField(max_length=16,verbose_name='版本')
    # 任务单 可能出现的状态
    status_choices = (
        (1,'待发布'),
        (2,'发布中'),
        (3,'成功'),
        (4,'失败'),
    )
    status = models.IntegerField(verbose_name='状态',choices=status_choices,default=1)

    # 我在发布的流程中 设置了几个钩子节点 方便用户在不同阶段可以进行额外的操作  定义多少及哪些阶段 具体看业务需求
    # 步骤3:预留钩子功能字段  项目有多种python、java、php、vue大体一致但小细节有区别
    before_download_script = models.TextField(verbose_name='下载前脚本', null=True, blank=True)
    after_download_script = models.TextField(verbose_name='下载后脚本', null=True, blank=True)
    before_deploy_script = models.TextField(verbose_name='发布前脚本', null=True, blank=True)
    after_deploy_script = models.TextField(verbose_name='发布后脚本', null=True, blank=True)

```