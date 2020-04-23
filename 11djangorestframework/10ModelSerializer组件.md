# ModelSerializer组件

1）序列化与反序列功能可以整合成一个类，该类继承ModelSerializer
2）继承ModelSerializer类的资源序列化类，内部包含三部分
    Meta子类、局部钩子、全局钩子
    注：create和update方法ModelSerializer已经重写了，使用不需要重写
3）在Meta子类中：
    用model来绑定关联的Model类
    用fields来设置所有的序列化反序列化字段
    用extra_kwargs来设置系统的校验规则
4）重要的字段校验规则：
    read_only校验规则，代表该字段只参与序列化
    write_only校验规则，代表该字段只参与反序列化
    required校验规则，代表该字段在反序列化是是否是必填(True)还是选填(False)，不能和read_only一起使用(规则冲突)
    规则细节：
        如果一个字段有默认值或是可以为空，没设置required规则，默认为False，反之默认值为True
        如果一个Model字段即没有设置read_only也没设置write_only，该字段默认参与序列化及反序列化
5）自定义序列化字段：在Model类中，定义方法属性（可以返回特殊值，还可以完成连表操作），在序列化类的fields属性中可以选择性插拔
6）自定义反序列化字段：在Serializer类中，自定义校验字段，校验规则也只能在声明字段时设置，自定义的反序列化字段（如re_pwd）,
必须设置write_only为True

```python
from rest_framework.serializers import ModelSerializer
from . import models
# 整合序列化与反序列化
class UserModelSerializer(ModelSerializer):
    # 将序列化类与Model类进行绑定
    # 设置序列化与反序列化所有字段（并划分序列化字段与反序列化字段）
    # 设置反序列化的局部与全局钩子

    # 自定义反序列化字段，校验规则只能在声明自定义反序列化字段时设置，且一定是write_only
    re_pwd = serializers.CharField(min_length=3, max_length=64, write_only=True)

    class Meta:
        model = models.User
        # 序列化
        fields = ['name', 'age', 'height', 'gender', 'pwd', 're_pwd']
        # 反序列化
        extra_kwargs = {
            'name': {
                'required': True,
                'min_length': 3,
                'error_messages': {
                    'min_length': '太短'
                }
            },
            'age': {
                'required': True,  # 数据库有默认值或可以为空字段，required默认为False
                'min_value': 0
            },
            'pwd': {
                'required': True,
                'write_only': True,  # 只参与反序列化
            },
            'gender': {
                'read_only': True,  # 只参与序列化
            },
        }

    def validate_name(self, value):
        if 'g' in value.lower():
            raise serializers.ValidationError('名字中不能有g')
        return value

    def validate(self, attrs):
        pwd = attrs.get('pwd')
        re_pwd = attrs.pop('re_pwd')
        if pwd != re_pwd:
            raise serializers.ValidationError({'re_pwd': '两次密码不一致'})
        return attrs

    # ModelSerializer已经帮我们重写了入库方法
```

```python
    # 自定义插拔序列化字段：替换了在Serializer类中自定义的序列化字段（SerializerMethodField）
    # 自定义插拔序列化字段一定不参与反序列化过程
    @property
    def gender(self):
        return self.get_sex_display()
```

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from . import models, serializers
from rest_framework import status
class UserV2APIView(APIView):
    # 单查群查
    def get(self, request, *args, **kwargs):
        pk = kwargs.get('pk')
        if pk:
            user_obj = models.User.objects.filter(pk=pk).first()
            if not user_obj:
                return Response({
                    'status': 1,
                    'msg': '单查 error'
                })

            # 完成序列化
            user_data = serializers.UserModelSerializer(user_obj, many=False).data

            return Response({
                'status': 0,
                'msg': '单查 ok',
                'results': user_data
            })

        # 群查
        user_query = models.User.objects.all()
        # 完成序列化
        user_list_data = serializers.UserModelSerializer(user_query, many=True).data
        return Response({
            'status': 0,
            'msg': '群查 ok',
            'results': user_list_data
        })

    # 单增
    def post(self, request, *args, **kwargs):
        request_data = request.data

        user_ser = serializers.UserModelSerializer(data=request_data)

        # 校验失败，直接抛异常，反馈异常信息给前台，只要校验通过，代码才会往下执行
        result = user_ser.is_valid()
        if result:
            user_obj = user_ser.save()
            return Response({
                'status': 0,
                'msg': 'ok',
                'results': serializers.UserModelSerializer(user_obj).data
            })
        else:
            # 校验失败，返回错误信息
            return Response({
                'status': 1,
                'msg': user_ser.errors
            }, status=status.HTTP_400_BAD_REQUEST)


```

# 模型类序列化器

如果我们想要使用序列化器对应的是Django的模型类，DRF为我们提供了ModelSerializer模型类序列化器来帮助我们快速创建一个Serializer类。

ModelSerializer与常规的Serializer相同，但提供了：

- 基于模型类自动生成一系列字段
- 基于模型类自动为Serializer生成validators，比如unique_together
- 包含默认的create()和update()的实现

# 定义

比如我们创建一个BookInfoSerializer

```
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo
        fields = '__all__'
```

- model 指明参照哪个模型类
- fields 指明为模型类的哪些字段生成

我们可以在python manage.py shell中查看自动生成的BookInfoSerializer的具体实现

```
>>> from booktest.serializers import BookInfoSerializer
>>> serializer = BookInfoSerializer()
>>> serializer
BookInfoSerializer():
    id = IntegerField(label='ID', read_only=True)
    btitle = CharField(label='名称', max_length=20)
    bpub_date = DateField(allow_null=True, label='发布日期', required=False)
    bread = IntegerField(label='阅读量', max_value=2147483647, min_value=-2147483648, required=False)
    bcomment = IntegerField(label='评论量', max_value=2147483647, min_value=-2147483648, required=False)
    image = ImageField(allow_null=True, label='图片', max_length=100, required=False)
```

# 指定字段

1) 使用**fields**来明确字段，`__all__`表名包含所有字段，也可以写明具体哪些字段，如

```
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo
        fields = ('id', 'btitle', 'bpub_date')
```

2) 使用**exclude**可以明确排除掉哪些字段

```
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo
        exclude = ('image',)
```

3) 显示指明字段，如：

```
class HeroInfoSerializer(serializers.ModelSerializer):
    hbook = BookInfoSerializer()

    class Meta:
        model = HeroInfo
        fields = ('id', 'hname', 'hgender', 'hcomment', 'hbook')
```

4) 指明只读字段

可以通过**read_only_fields**指明只读字段，即仅用于序列化输出的字段

```
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo
        fields = ('id', 'btitle', 'bpub_date'， 'bread', 'bcomment')
        read_only_fields = ('id', 'bread', 'bcomment')
```

# 添加额外参数

我们可以使用**extra_kwargs**参数为ModelSerializer添加或修改原有的选项参数

```
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo
        fields = ('id', 'btitle', 'bpub_date', 'bread', 'bcomment')
        extra_kwargs = {
            'bread': {'min_value': 0, 'required': True},
            'bcomment': {'min_value': 0, 'required': True},
        }

# BookInfoSerializer():
#    id = IntegerField(label='ID', read_only=True)
#    btitle = CharField(label='名称', max_length=20)
#    bpub_date = DateField(allow_null=True, label='发布日期', required=False)
#    bread = IntegerField(label='阅读量', max_value=2147483647, min_value=0, required=True)
#    bcomment = IntegerField(label='评论量', max_value=2147483647, min_value=0, required=True)
```



