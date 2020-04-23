

# 序列化

1. drf的核心：序列化模块
2. Serializer类（了解）- 偏底层，开发效率不高
3. ModelSerializer类（重中之重） - 开发运用阶段才有的序列化方式，开发效率高
4. ListSerializer类（正常）- 完成群增，群改接口的辅助序列化类

##  为什么要使用序列化

后台的数据多以后台类的对象存在，经过序列化后，就可以格式化成能返回给前台的数据

# 自定义序列化

## 数据准备

models.py

```python
from django.db import models


# Create your models here.
class User(models.Model):
    CHOICE_SEX = ((0, '男'), (1, '女'))
    name = models.CharField(max_length=64)
    age = models.IntegerField(default=0)
    height = models.DecimalField(max_digits=5, decimal_places=2, default=0)
    icon = models.ImageField(upload_to='icon', default='default.png')
    sex = models.IntegerField(choices=CHOICE_SEX, default=0)

    # pwd = models.CharField(max_length=64,default='123456')

    class Meta:
        db_table = 'ob_user'
        verbose_name = '用户表'
        verbose_name_plural = verbose_name

    def __str__(self):
        return self.name
```

api/urls.py

```python
from django.conf.urls import url, include
from . import views
urlpatterns = [
    url(r'^user/$', views.UserAPIView.as_view()),
    url(r'^user/(?P<pk>\d+)/$', views.UserAPIView.as_view()),

]
```

settings.py

```python
# 规定用户上传的静态文件都在这个media文件夹下 
MEDIA_URL='/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

### settings.py国际化

```python
LANGUAGE_CODE = 'zh-hans'

TIME_ZONE = 'Asia/Shanghai'

USE_I18N = True

USE_L10N = True

USE_TZ = False
```

## 自定义序列化

```python
#返回的数据类型
return Response({
                    'status': 1,
                    'msg': '单查 error'
                })
```

```python
from . import models
from rest_framework.views import APIView
from rest_framework.response import Response
from django.conf import settings
class UserAPIView(APIView):
    def get(self, request, *args, **kwargs):
        print(kwargs)
        pk = kwargs.get('pk')
        print(pk)

        if pk:
            # ORM操作数据库拿到资源数据
            # 格式化成能返回给前台的数据(序列化)
            # 返回格式化后的数据
            user_obj = models.User.objects.filter(pk=pk).first()
            if not user_obj:
                return Response({
                    'status': 1,
                    'msg': '单查 error'
                })

            # 完成序列化
            user_data = {
                'name': user_obj.name,
                'age': user_obj.age,
                'height': user_obj.height,
                'sex': user_obj.get_sex_display(),
                'icon': 'http://127.0.0.1:8000%s%s' % (settings.MEDIA_URL, user_obj.icon),
            }
            # class MySerializer: pass
            # user_data = MySerializer(user_obj)

            return Response({
                'status': 0,
                'msg': '单查 ok',
                'results': user_data
            })

        # 群查
        user_query = models.User.objects.all()
        if not user_query:
            return Response({
                'static': 7,
                'msg': '请求错误',
            })

        # 完成序列化
        user_list_data = []
        for user_obj in user_query:
            user_list_data.append(
                {
                    'name': user_obj.name,
                    'age': user_obj.age,
                    'height': user_obj.height,
                    'sex': user_obj.get_sex_display(),
                    'icon': 'http://127.0.0.1:8000%s%s' % (settings.MEDIA_URL, user_obj.icon),
                }
            )
        # class MySerializer: pass
        # user_list_data = MySerializer(user_query, many=True)

        return Response({
            'status': 0,
            'msg': '群查 ok',
            'results': user_list_data
        })
```



# Serializer组件

## 序列化

一、视图类的三步操作
1）ORM操作数据库拿到资源数据
2）格式化(序列化)成能返回给前台的数据
3）返回格式化后的数据

二、视图类的序列化操作
1）直接将要序列化的数据传给序列化类
2）要序列化的数据如果是单个对象，序列化的参数many为False，数据如果是多个对象(list,queryset)序列化的参数many为True

三、序列化类
1）model了中要反馈给前台的字段，在序列化类中要进行声明，属性名必须就是model的字段名，且Field类型也要保持一致(不需要明确规则)
2）model了中不需要反馈给前台的字段，在序列化类中不需要声明(省略)
3）自定义序列化字段用 SerializerMethodField() 作为字段类型，该字段的值来源于 get_自定义字段名(self, obj) 方法的返回值

```python
# @Author : OceanSkychen # @File : serializers.py
from rest_framework import serializers
class UserSerializer(serializers.Serializer):
    # model已有的属性字段
    #   如果要参与序列化，名字一定要与model的属性同名
    #   如果不参与序列化的model属性，在序列化类中不做声明

    name = serializers.CharField()
    age = serializers.IntegerField()
    height = serializers.DecimalField(max_digits=5, decimal_places=2)
    # sex = serializers.IntegerField()

    # 自定义序列化字段，序列化的属性值由方法来提供，
    #   方法的名字：固定为 get_属性名，
    #   方法的参数：序列化对象，序列化的model对象
    #   强烈建议自定义序列化字段名不要与model已有的属性名重名

    gender = serializers.SerializerMethodField()
    def get_gender(self, obj):
        # print('>>>>>>', type(self))
        # print('>>>>>>', type(obj))
        return obj.get_sex_display()
```



```python
from rest_framework.views import APIView
from rest_framework.response import Response
from . import models, serializers
from rest_framework import status
class UserAPIView(APIView):
    # 单查群查
    def get(self, request, *args, **kwargs):
        pk = kwargs.get('pk')

        if pk:
            # ORM操作数据库拿到资源数据
            # 格式化成能返回给前台的数据(序列化)
            # 返回格式化后的数据
            user_obj = models.User.objects.filter(pk=pk).first()
            if not user_obj:
                return Response({
                    'status': 1,
                    'msg': '单查 error'
                })

            # 完成序列化
            user_ser = serializers.UserSerializer(user_obj, many=False)
            user_data = user_ser.data

            return Response({
                'status': 0,
                'msg': '单查 ok',
                'results': user_data
            })

        # 群查
        user_query = models.User.objects.all()

        # 完成序列化
        user_list_data = serializers.UserSerializer(user_query, many=True).data


        return Response({
            'status': 0,
            'msg': '群查 ok',
            'results': user_list_data
        })
```

## 反序列化

一、视图类的三步操作
1）从请求对象中拿到前台的数据
2）校验前台数据是否合法
3）反序列化成后台Model对象与数据库交互

二、视图类的反序列化操作
1）将要反序列化的数据传给序列化类的data参数
2）要反序列化的数据如果是单个字典，反序列化的参数many为False，数据如果是多个字典的列表，反序列化的参数many为True

三、反序列化类
1）系统的字段，可以在Field类型中设置系统校验规则（name=serializers.CharField(min_length=3)）
2）required校验规则绝对该字段是必校验还是可选校验字段（默认required为True，数据库字段有默认值或可以为空的字段required可以赋值为False）
3）自定义的反序列字段，设置系统校验规则同系统字段，但是需要在自定义校验规则中（局部、全局钩子）将自定义反序列化字段取出（返回剩余的数据与数据库交互）
4）局部钩子的方法命名 validate_属性名(self, 属性的value)，校验规则为 成功返回属性的value 失败抛出校验错误的异常
5）全局钩子的方法命名 validate(self, 所有属性attrs)，校验规则为 成功返回attrs 失败抛出校验错误的异常

```python
# @Author : OceanSkychen # @File : serializers.py
from rest_framework import serializers
from . import models
class UserDeserializer(serializers.Serializer):
    # 系统校验规则

    # 系统必须反序列化的字段
    # 序列化属性名不是必须与model属性名对应，但是与之对应会方便序列化将校验通过的数据与数据库进行交互
    name = serializers.CharField(min_length=3, max_length=64, error_messages={
        'required': '姓名必填',
        'min_length': '太短',
    })
    pwd = serializers.CharField(min_length=3, max_length=64)

    # 系统可选的反序列化字段：没有提供不进行校验(数据库中有默认值或可以为空)，提供了就进行校验
    age = serializers.IntegerField(min_value=0, max_value=150, required=False)

    # 自定义反序列化字段：一定参与校验，且要在校验过程中，将其从入库的数据中取出，剩余与model对应的数据才会入库
    re_pwd = serializers.CharField(min_length=3, max_length=64)

    # 自定义校验规则：局部钩子，全局钩子
    # 局部钩子：validate_字段名(self, 字段值)
    # 规则：成功返回value，失败抛异常
    def validate_aaa(self, value):
        if 'g' in value.lower():
            raise serializers.ValidationError('名字中不能有g')
        return value

    # 全局钩子：validate(self, 所有校验的数据字典)
    # 规则：成功返回attrs，失败抛异常
    def validate(self, attrs):
        # 取出联合校验的字段们：需要入库的值需要拿到值，不需要入库的需要从校验字段中取出
        pwd = attrs.get('pwd')
        re_pwd = attrs.pop('re_pwd')
        if pwd != re_pwd:
            raise serializers.ValidationError({'re_pwd': '两次密码不一致'})
        return attrs

    # create重写，完成入库
    def create(self, validated_data):
        return models.User.objects.create(**validated_data)


```

```python
  # 单增
    def post(self, request, *args, **kwargs):
        # 从请求对象中拿到前台的数据
        # 校验前台数据是否合法
        # 反序列化成后台Model对象与数据库交互
        request_data = request.data

        # 反序列化目的：封装数据的校验过程，以及数据库交互的过程
        user_ser = serializers.UserDeserializer(data=request_data)
        # 调用反序列化的校验规则：系统规则，自定义规则(局部钩子，全局钩子)
        result = user_ser.is_valid()

        if result:
            # 校验通过，可以与数据库进行交互：增(create)，改(update)
            user_obj = user_ser.save()
            return Response({
                'status': 0,
                'msg': 'ok',
                'results': serializers.UserSerializer(user_obj).data
            })
        else:
            # 校验失败，返回错误信息
            return Response({
                'status': 1,
                'msg': user_ser.errors
            }, status=status.HTTP_400_BAD_REQUEST)

```



# 序列化器-Serializer



## 定义序列化器

Django REST framework中的Serializer使用类来定义，须继承自rest_framework.serializers.Serializer。

例如，我们已有了一个数据库模型类BookInfo

```
class BookInfo(models.Model):
    btitle = models.CharField(max_length=20, verbose_name='名称')
    bpub_date = models.DateField(verbose_name='发布日期', null=True)
    bread = models.IntegerField(default=0, verbose_name='阅读量')
    bcomment = models.IntegerField(default=0, verbose_name='评论量')
    image = models.ImageField(upload_to='booktest', verbose_name='图片', null=True)
```

我们想为这个模型类提供一个序列化器，可以定义如下：

```
class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    id = serializers.IntegerField(label='ID', read_only=True)
    btitle = serializers.CharField(label='名称', max_length=20)
    bpub_date = serializers.DateField(label='发布日期', required=False)
    bread = serializers.IntegerField(label='阅读量', required=False)
    bcomment = serializers.IntegerField(label='评论量', required=False)
    image = serializers.ImageField(label='图片', required=False)
```

**注意：serializer不是只能为数据库模型类定义，也可以为非数据库模型类的数据定义。**serializer是独立于数据库之外的存在。

**常用字段类型**：

| 字段                    | 字段构造方式                                                 |
| ----------------------- | ------------------------------------------------------------ |
| **BooleanField**        | BooleanField()                                               |
| **NullBooleanField**    | NullBooleanField()                                           |
| **CharField**           | CharField(max_length=None, min_length=None, allow_blank=False, trim_whitespace=True) |
| **EmailField**          | EmailField(max_length=None, min_length=None, allow_blank=False) |
| **RegexField**          | RegexField(regex, max_length=None, min_length=None, allow_blank=False) |
| **SlugField**           | SlugField(max_length=50, min_length=None, allow_blank=False) 正则字段，验证正则模式 [a-zA-Z0-9*-]+ |
| **URLField**            | URLField(max_length=200, min_length=None, allow_blank=False) |
| **UUIDField**           | UUIDField(format='hex_verbose') format: 1) `'hex_verbose'` 如`"5ce0e9a5-5ffa-654b-cee0-1238041fb31a"` 2） `'hex'` 如 `"5ce0e9a55ffa654bcee01238041fb31a"` 3）`'int'` - 如: `"123456789012312313134124512351145145114"` 4）`'urn'` 如: `"urn:uuid:5ce0e9a5-5ffa-654b-cee0-1238041fb31a"` |
| **IPAddressField**      | IPAddressField(protocol='both', unpack_ipv4=False, **options) |
| **IntegerField**        | IntegerField(max_value=None, min_value=None)                 |
| **FloatField**          | FloatField(max_value=None, min_value=None)                   |
| **DecimalField**        | DecimalField(max_digits, decimal_places, coerce_to_string=None, max_value=None, min_value=None) max_digits: 最多位数 decimal_palces: 小数点位置 |
| **DateTimeField**       | DateTimeField(format=api_settings.DATETIME_FORMAT, input_formats=None) |
| **DateField**           | DateField(format=api_settings.DATE_FORMAT, input_formats=None) |
| **TimeField**           | TimeField(format=api_settings.TIME_FORMAT, input_formats=None) |
| **DurationField**       | DurationField()                                              |
| **ChoiceField**         | ChoiceField(choices) choices与Django的用法相同               |
| **MultipleChoiceField** | MultipleChoiceField(choices)                                 |
| **FileField**           | FileField(max_length=None, allow_empty_file=False, use_url=UPLOADED_FILES_USE_URL) |
| **ImageField**          | ImageField(max_length=None, allow_empty_file=False, use_url=UPLOADED_FILES_USE_URL) |
| **ListField**           | ListField(child=, min_length=None, max_length=None)          |
| **DictField**           | DictField(child=)                                            |

**选项参数：**

| 参数名称            | 作用             |
| ------------------- | ---------------- |
| **max_length**      | 最大长度         |
| **min_lenght**      | 最小长度         |
| **allow_blank**     | 是否允许为空     |
| **trim_whitespace** | 是否截断空白字符 |
| **max_value**       | 最小值           |
| **min_value**       | 最大值           |

通用参数：

| 参数名称           | 说明                                          |
| ------------------ | --------------------------------------------- |
| **read_only**      | 表明该字段仅用于序列化输出，默认False         |
| **write_only**     | 表明该字段仅用于反序列化输入，默认False       |
| **required**       | 表明该字段在反序列化时必须输入，默认True      |
| **default**        | 反序列化时使用的默认值                        |
| **allow_null**     | 表明该字段是否允许传入None，默认False         |
| **validators**     | 该字段使用的验证器                            |
| **error_messages** | 包含错误编号与错误信息的字典                  |
| **label**          | 用于HTML展示API页面时，显示的字段名称         |
| **help_text**      | 用于HTML展示API页面时，显示的字段帮助提示信息 |

# 创建Serializer对象

定义好Serializer类后，就可以创建Serializer对象了。

Serializer的构造方法为：

```
Serializer(instance=None, data=empty, **kwarg)
```

说明：

1）用于序列化时，将模型类对象传入**instance**参数

2）用于反序列化时，将要被反序列化的数据传入**data**参数

3）除了instance和data参数外，在构造Serializer对象时，还可通过**context**参数额外添加数据，如

```
serializer = AccountSerializer(account, context={'request': request})
```

**通过context参数附加的数据，可以通过Serializer对象的context属性获取。**

1. 使用序列化器的时候一定要注意，序列化器声明了以后，不会自动执行，需要我们在视图中进行调用才可以。
2. 序列化器无法直接接收数据，需要我们在视图中创建序列化器对象时把使用的数据传递过来。
3. 序列化器的字段声明类似于我们前面使用过的表单系统。
4. 开发restful api时，序列化器会帮我们把模型数据转换成字典.
5. drf提供的视图会帮我们把字典转换成json,或者把客户端发送过来的数据转换字典.

# 序列化器的使用

序列化器的使用分两个阶段：

1. 在客户端请求时，使用序列化器可以完成对数据的反序列化。
2. 在服务器响应时，使用序列化器可以完成对数据的序列化。

## 序列化

### 基本使用

1） 先查询出一个图书对象

```
from booktest.models import BookInfo

book = BookInfo.objects.get(id=2)
```

2） 构造序列化器对象

```
from booktest.serializers import BookInfoSerializer

serializer = BookInfoSerializer(book)
```

3）获取序列化数据

通过data属性可以获取序列化后的数据

```
serializer.data
# {'id': 2, 'btitle': '天龙八部', 'bpub_date': '1986-07-24', 'bread': 36, 'bcomment': 40, 'image': None}
```

4）如果要被序列化的是包含多条数据的查询集QuerySet，可以通过添加**many=True**参数补充说明

```
book_qs = BookInfo.objects.all()
serializer = BookInfoSerializer(book_qs, many=True)
serializer.data
# [OrderedDict([('id', 2), ('btitle', '天龙八部'), ('bpub_date', '1986-07-24'), ('bread', 36), ('bcomment', 40), ('image', N]), OrderedDict([('id', 3), ('btitle', '笑傲江湖'), ('bpub_date', '1995-12-24'), ('bread', 20), ('bcomment', 80), ('image'ne)]), OrderedDict([('id', 4), ('btitle', '雪山飞狐'), ('bpub_date', '1987-11-11'), ('bread', 58), ('bcomment', 24), ('ima None)]), OrderedDict([('id', 5), ('btitle', '西游记'), ('bpub_date', '1988-01-01'), ('bread', 10), ('bcomment', 10), ('im', 'booktest/xiyouji.png')])]
```

## 反序列化

### 数据验证（钩子函数）

使用序列化器进行反序列化时，需要对数据进行验证后，才能获取验证成功的数据或保存成模型类对象。

在获取反序列化的数据前，必须调用**is_valid()**方法进行验证，验证成功返回True，否则返回False。

验证失败，可以通过序列化器对象的**errors**属性获取错误信息，返回字典，包含了字段和字段的错误。如果是非字段错误，可以通过修改REST framework配置中的**NON_FIELD_ERRORS_KEY**来控制错误字典中的键名。

验证成功，可以通过序列化器对象的**validated_data**属性获取数据。

在定义序列化器时，指明每个字段的序列化类型和选项参数，本身就是一种验证行为。

如我们前面定义过的BookInfoSerializer

```
class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    id = serializers.IntegerField(label='ID', read_only=True)
    btitle = serializers.CharField(label='名称', max_length=20)
    bpub_date = serializers.DateField(label='发布日期', required=False)
    bread = serializers.IntegerField(label='阅读量', required=False)
    bcomment = serializers.IntegerField(label='评论量', required=False)
    image = serializers.ImageField(label='图片', required=False)
```

通过构造序列化器对象，并将要反序列化的数据传递给data构造参数，进而进行验证

```
from booktest.serializers import BookInfoSerializer
data = {'bpub_date': 123}
serializer = BookInfoSerializer(data=data)
serializer.is_valid()  # 返回False
serializer.errors
# {'btitle': [ErrorDetail(string='This field is required.', code='required')], 'bpub_date': [ErrorDetail(string='Date has wrong format. Use one of these formats instead: YYYY[-MM[-DD]].', code='invalid')]}
serializer.validated_data  # {}

data = {'btitle': 'python'}
serializer = BookInfoSerializer(data=data)
serializer.is_valid()  # True
serializer.errors  # {}
serializer.validated_data  #  OrderedDict([('btitle', 'python')])
```

is_valid()方法还可以在验证失败时抛出异常serializers.ValidationError，可以通过传递**raise_exception=True**参数开启，REST framework接收到此异常，会向前端返回HTTP 400 Bad Request响应。

```
# Return a 400 response if the data was invalid.
serializer.is_valid(raise_exception=True)
```

如果觉得这些还不够，需要再补充定义验证行为，可以使用以下三种方法：

#### 1) validate_字段名

对`<field_name>`字段进行验证，如

```
class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    ...

    def validate_btitle(self, value):
        if 'django' not in value.lower():
            raise serializers.ValidationError("图书不是关于Django的")
        return value
```

测试

```
from booktest.serializers import BookInfoSerializer
data = {'btitle': 'python'}
serializer = BookInfoSerializer(data=data)
serializer.is_valid()  # False   
serializer.errors
#  {'btitle': [ErrorDetail(string='图书不是关于Django的', code='invalid')]}
```

#### 2) validate

在序列化器中需要同时对多个字段进行比较验证时，可以定义validate方法来验证，如

```
class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    ...

    def validate(self, attrs):
        bread = attrs['bread']
        bcomment = attrs['bcomment']
        if bread < bcomment:
            raise serializers.ValidationError('阅读量小于评论量')
        return attrs
```

测试

```
from booktest.serializers import BookInfoSerializer
data = {'btitle': 'about django', 'bread': 10, 'bcomment': 20}
s = BookInfoSerializer(data=data)
s.is_valid()  # False
s.errors
#  {'non_field_errors': [ErrorDetail(string='阅读量小于评论量', code='invalid')]}
```

#### 3) validators

在字段中添加validators选项参数，也可以补充验证行为，如

```
def about_django(value):
    if 'django' not in value.lower():
        raise serializers.ValidationError("图书不是关于Django的")

class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    id = serializers.IntegerField(label='ID', read_only=True)
    btitle = serializers.CharField(label='名称', max_length=20, validators=[about_django])
    bpub_date = serializers.DateField(label='发布日期', required=False)
    bread = serializers.IntegerField(label='阅读量', required=False)
    bcomment = serializers.IntegerField(label='评论量', required=False)
    image = serializers.ImageField(label='图片', required=False)
```

测试：

```
from booktest.serializers import BookInfoSerializer
data = {'btitle': 'python'}
serializer = BookInfoSerializer(data=data)
serializer.is_valid()  # False   
serializer.errors
#  {'btitle': [ErrorDetail(string='图书不是关于Django的', code='invalid')]}
```

## 反序列化-保存数据

前面的验证数据成功后,我们可以使用序列化器来完成数据反序列化的过程.这个过程可以把数据转成模型类对象.

可以通过实现create()和update()两个方法来实现。

```
class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    ...

    def create(self, validated_data):
        """新建"""
        return BookInfo(**validated_data)

    def update(self, instance, validated_data):
        """更新，instance为要更新的对象实例"""
        instance.btitle = validated_data.get('btitle', instance.btitle)
        instance.bpub_date = validated_data.get('bpub_date', instance.bpub_date)
        instance.bread = validated_data.get('bread', instance.bread)
        instance.bcomment = validated_data.get('bcomment', instance.bcomment)
        return instance
```

如果需要在返回数据对象的时候，也将数据保存到数据库中，则可以进行如下修改

```
class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    ...

    def create(self, validated_data):
        """新建"""
        return BookInfo.objects.create(**validated_data)

    def update(self, instance, validated_data):
        """更新，instance为要更新的对象实例"""
        instance.btitle = validated_data.get('btitle', instance.btitle)
        instance.bpub_date = validated_data.get('bpub_date', instance.bpub_date)
        instance.bread = validated_data.get('bread', instance.bread)
        instance.bcomment = validated_data.get('bcomment', instance.bcomment)
        instance.save()
        return instance
```

实现了上述两个方法后，在反序列化数据的时候，就可以通过save()方法返回一个数据对象实例了

```
book = serializer.save()
```

如果创建序列化器对象的时候，没有传递instance实例，则调用save()方法的时候，create()被调用，相反，如果传递了instance实例，则调用save()方法的时候，update()被调用。

```
from db.serializers import BookInfoSerializer
data = {'btitle': '封神演义'}
serializer = BookInfoSerializer(data=data)
serializer.is_valid()  # True
serializer.save()  # <BookInfo: 封神演义>

from db.models import BookInfo
book = BookInfo.objects.get(id=2)
data = {'btitle': '倚天剑'}
serializer = BookInfoSerializer(book, data=data)
serializer.is_valid()  # True
serializer.save()  # <BookInfo: 倚天剑>
book.btitle  # '倚天剑'
```

# 附加说明

1） 在对序列化器进行save()保存时，可以额外传递数据，这些数据可以在create()和update()中的validated_data参数获取到

```
# request.user 是django中记录当前登录用户的模型对象
serializer.save(owner=request.user)
```

2）默认序列化器必须传递所有required的字段，否则会抛出验证异常。但是我们可以使用partial参数来允许部分字段更新

```
# Update `comment` with partial data
serializer = CommentSerializer(comment, data={'content': u'foo bar'}, partial=True)
```



