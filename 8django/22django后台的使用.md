# 自建用户表

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191125203537781-667405909.png)

用户表主要控制3张表

```
# 修改auth模块的用户表指向
AUTH_USER_MODEL = 'api.User'
```

```
# 重点：
# 1）auth认证6表必须在第一次数据库迁移前确定，第一次数据库迁移完成
# 2）完成数据库迁移，出现了auth的用户表迁移异常，需要删除的数据库迁移文件
#       User表所在的自定义应用下的、admin组件下的、auth组件下的
```

```python
from django.db import models

# Create your models here.

from django.contrib.auth.models import AbstractUser


class User(AbstractUser):
    mobile = models.CharField(max_length=11, unique=True)

    class Meta:
        db_table = 'od_user'  # 重新给表命名
        verbose_name = '用户表'  # 后台登陆显示表名
        verbose_name_plural = verbose_name

    def __str__(self):  # 用一个字段来表示一个表
        return self.username
```

# auth组件中的六个表

[RABC：基于角色的访问权限](https://baike.baidu.com/item/%E5%9F%BA%E4%BA%8E%E8%A7%92%E8%89%B2%E7%9A%84%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6/8795406?fr=aladdin&fromtitle=RBAC&fromid=1328788)

理解参照下表



![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191125184214145-1346430829.png)
![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191125184215817-22352623.png)
![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191125184218890-793628274.png)



# Content_type表

content_type 主要是和permission提供作用，是django建立的表

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191125184221305-2118236131.png)

```python
# 给Django中的所有模块中的所有表进行编号存储到content_type表中
# 应用一：权限表的权限是操作表的，所有在权限表中有一个content_type表的外键，标识该权限具体操作的是哪张表
# 应用二：价格策略

"""
Course：
name、type、days、price、vip_type
基础	免费课  7		0
中级	学位课	 180	69
究极	会员课	 360    	 至尊会员


Course：
name、type、days、content_type_id
基础	免费课  7	  null
中级	学位课	 180   1
究极	会员课	 360   2

app01_course_1
id、price

app01_course_2
id vip_type

content_type表（Django提供）
id、app_label、model
1	app01	 course_1
2	app01	 course_2
"""
```

# 后台显示

## 注册

在admin.py文件夹中进行注册，在后台可以显示

```python
from django.contrib import admin

# Register your models here.

from . import models
admin.site.register(models.User)#注册user表
```

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191125203545155-1516067311.png)

## 注册显示重写

如果不重写的话，在添加用户的时候不能直接添加，因为密码是明文的，所以在后台添加的时候，需要进行重写

`from django.contrib.auth.admin import UserAdmin`

在后台所有的展示和输入都可以由这个模块进行控制

可以自己控制重写的内容

主要是重写里里面的属性



```python
from django.contrib import admin

from . import models


# admin注册自定义User表：密文操作密码
from django.contrib.auth.admin import UserAdmin as AuthUserAdmin
class UserAdmin(AuthUserAdmin):
    add_fieldsets = (
        (None, {
            'classes': ('wide',),
            # 添加用户界面可操作的字段
            'fields': ('username', 'password1', 'password2', 'mobile', 'email', 'is_staff', 'is_active'),
        }),
    )
    list_display = ('username', 'mobile', 'email', 'is_staff', 'is_active')

# 明文操作密码，admin可视化添加的用户密码都是明文，登录时用的是密文，所以用户无法登录
# admin.site.register(models.User)
admin.site.register(models.User, UserAdmin)

```

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191125203540128-314261190.png)
![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191125203542699-1678624792.png)

## admin扩展功能

```python
class ArticleConfig(admin.ModelAdmin):
    list_display = ['title','create_time']  # 配置展示字段
    list_display_links = ['title','create_time']  # 指定多个跳转标签
    search_fields = ['title']  # 指定查询  多个字段默认是or的关系

    def patch_init(self,queryset):
    pass
    patch_init.short_description = '批量更新'
    actions = [patch_init,]  # 自定义批量处理函数

    list_filter = ['tags','category']  # 定义外键字段的过滤

```







![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191111145335757-1318783305.png)

