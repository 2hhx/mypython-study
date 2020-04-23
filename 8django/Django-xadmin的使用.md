# Django-xadmin的介绍

## 方式1


<div id='title'>Django-xadmin的介绍</div>
> `Django`是`python`的重量级web框架,写得少,做得多,非常适合后端开发,它很大的一个亮点是,自带后台管理模块,但它自带的后台管理有点丑,而`Xadmin`是基于`bootstrap`开发的一套后台管理框架,界面非常美观,只需几步就可以替换自带的`Django_admin`

<div id='title'>具体的安装步骤</div>
1. xadmin在python2.x时代的安装方法

在python2.x时代，安装xadmin是通过如下命令

> pip install xadmin 

2.`xadmin`在`python3.6.x`时代的安装方法

**需要安装如下的包**

```python
pip3 install django-import-export
pip3 install django-reversion
pip3 install django-formtools==2.1
pip3 install future
pip3 install httplib2
pip3 install six
pip3 install django-crispy-forms
```

2.1 下载`xadmin`
> https://github.com/sshwsfc/xadmin

2.2、解压缩，得到`xadmin`文件夹，复制到项目的`extra_apps`,解压缩，得到`xadmin`文件夹, 如下图所示：

![xadmin.jpg](https://i.loli.net/2019/06/20/5d0b41759655b82871.jpg)

2.3、在django中的根目录下创建`Python Package`，命名为`extra_apps`（如果不存在此文件夹则创建, 然后 鼠标右键`extra_app` 随后 `mark as sources root`）
（`Python Package`是带`init`文件的，跟普通`Package`不同）

创建完`extra_apps`，需要在`settings`中配置一下`extra_apps`。设置为可搜索的路径

```python
import os
import sys

# Build paths inside the project like this: os.path.join(BASE_DIR, ...)
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
sys.path.insert(0, os.path.join(BASE_DIR, 'extra_apps')) # 把extra_apps文件夹添加到搜索目录中
```

2.4、把`xadmin`文件夹复制到`extra_apps`


2.5、`xadmin`的配置

**配置到 INSTALLED_APPS**

```python

## 显示中文
# Application definition
# LANGUAGE_CODE = 'en-us'
LANGUAGE_CODE = 'zh-hans'
 
# TIME_ZONE = 'UTC'
TIME_ZONE = 'Asia/Shanghai'

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'xadmin',
    'crispy_forms', # 注意crispy_forms之间是下划线隔开，不是横线
]
```

2.6、修改`urls.py`的`admin`

```python
import xadmin
from django.conf.urls import url
from django.contrib import admin

urlpatterns = [
    # url('admin/', admin.site.urls),
    url(r'^xadmin/', xadmin.site.urls),
]
```

2.7、迁移文件

```python
python3 manage.py makemigrations
python3 manage.py migrate
```

**迁移完成后，我们看到数据库多了几张表**

![xadmintable.jpg](https://i.loli.net/2019/06/20/5d0b43084942d52028.jpg)

2.8、`pycharm`创建`superuser` 用户

```python
python3 manage.py makemigrations
python3 manage.py migrate
```

至此完成。

如果报错，请先把原先旧的`app`里面`admin.py`里面的注册代码去掉，再试试

运行一下项目，访问

> http://127.0.0.1:8000/xadmin/



#### <div id='title'>xadmin的使用</div>

1.需要在`app`中创建`adminx.py`文件

```python
import xadmin
from repository import models
from xadmin import views

class UserProfileAdmin(object):
	### 显示的字段名称
    list_display = ['id','name' ,'email','phone','mobile']
	
	# 搜索时可输入的字段内容
    search_fields = ['id', 'name', 'email', 'phone']
    
    # 点击id可进入详细界面进行编辑（默认的）
    list_display_links = ('id',)  
    
    ## 可编辑的列名
    list_editable = ['name' ,'email','phone','mobile']
    # list_filter = ['name' ,'email','phone','mobile']
	
	# 每页显示多少条
	list_per_page = 20 
	
	#根据id排序 
    ordering = ('id',)　
    　
    #设置只读字段　
    readonly_fields = ('user_email',) 
    
    #显示本条数据的所有信息
    show_detail_fields = ['asset_name'] 

xadmin.site.register(models.UserProfile,UserProfileAdmin)
```


3.数据导出
如果想要导出`Excel`数据，需要安装`xlwt`。

默认情况下，`xadmin`会提供`Excel`，`CSV`,`XML`，`json`四种格式的数据导出，可以通过设置`OptionClass`的`list_export`属性来指定使用哪些导出格式（四种格式分别用`xls`，`csv`，`xml`，`json`表示）或是将`list_export`设置为`None`来禁用数据导出功能

```python
list_export = ('xls', 'xml', 'json')
list_export_fields = ('id', 'name', 'title')
```


4.设置全局的配置

```python
# 全局修改，固定写法
class GlobalSettings(object):
    # 修改title
    site_title = 'xxx后台管理界面'
    # 修改footer
    site_footer = 'xxx的公司'
    # 收起菜单
    menu_style = 'accordion'
	
	# 设置 models图标
    # https://v3.bootcss.com/components/
    # http://www.yeahzan.com/fa/facss.html
	global_search_models = [models.Disk, models.Server]
    global_models_icon = {
        # Server: "glyphicon glyphicon-tree-conifer", Pool: "fa fa-cloud"
        models.Server: "fa fa-linux", models.Disk: "fa fa-cloud"
    }

	
# 将title和footer信息进行注册
xadmin.site.register(views.CommAdminView,GlobalSettings)

```

5. 图表显示

```python
data_charts = {
        "host_service_type_counts": {
            'title': '部门机器使用情况',
            'x-field': "business_unit",
            'y-field': ("business_unit"),
            'option': {
                "series": {"bars": {"align": "center", "barWidth": 0.8, "show": True}},
                "xaxis": {"aggregate": "count", "mode": "categories"}
            },
        },
        "host_idc_counts": {
            'title': '机房统计',
            'x-field': "idc",
            'y-field': ("idc",),
            'option': {
                "series": {"bars": {"align": "center", "barWidth": 0.3, "show": True}},
                "xaxis": {"aggregate": "count", "mode": "categories"}
            }
        }
    }
```

6. 注册模型与对应的管理类

```python
xadmin.site.register(models.Disk, DiskAdmin)
xadmin.site.register(models.Server, ServerAdmin)
xadmin.site.register(models.IDC, IDCAdmin)
xadmin.site.register(models.UserProfile, UserProfileAdmin)
xadmin.site.register(models.UserGroup, UserGroupAdmin)
```

## 方式2

### xadmin后台管理

##### 安装

```python
# >: pip install https://codeload.github.com/sshwsfc/xadmin/zip/django2
```



##### 注册app：dev.py

```python
INSTALLED_APPS = [
    # ...
    # xamin主体模块
    'xadmin',
    # 渲染表格模块
    'crispy_forms',
    # 为模型通过版本控制，可以回滚数据
    'reversion',
]
```



##### xadmin：需要自己的数据库模型类，完成数据库迁移

```python
python manage.py makemigrations
python manage.py migrate
```



##### 设置主路由替换掉admin：主urls.py

```python
# xadmin的依赖
import xadmin
xadmin.autodiscover()
# xversion模块自动注册需要版本控制的 Model
from xadmin.plugins import xversion
xversion.register_models()

urlpatterns = [
    # ...
    path(r'xadmin/', xadmin.site.urls),
]
```



##### 创建超级用户：大luffyapi路径终端

```python
# 在项目根目录下的终端
python manage.py createsuperuser
# 账号密码设置：admin | admin123
```



##### 完成xadmin全局配置：新建home/adminx.py

```python
# home/adminx.py
# xadmin全局配置
import xadmin
from xadmin import views

class GlobalSettings(object):
    """xadmin的全局配置"""
    site_title = "路飞学城"  # 设置站点标题
    site_footer = "路飞学城有限公司"  # 设置站点的页脚
    menu_style = "accordion"  # 设置菜单折叠

xadmin.site.register(views.CommAdminView, GlobalSettings)
```



##### 在adminx.py中注册model：home/adminx.px

```python
from . import models
# 注册
xadmin.site.register(models.Banner)
```



##### 修改app:home的名字：xadmin页面上的显示效果

```python
# home/__init__.py
default_app_config = "home.apps.HomeConfig"

# home/apps.py
from django.apps import AppConfig
class HomeConfig(AppConfig):
    name = 'home'
    verbose_name = '我的首页'
```

