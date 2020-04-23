# ListModelSerializer模块

# 自定义反序列化字段

```
# 一些只参与反序列化的字段，但是不是与数据库关联的
# 在序列化类中规定，并在校验字段时从校验的参数字典中剔除
class PublishModelSerializer(serializers.ModelSerializer):
    # 自定义不入库的 反序列化 字段
    re_name = serializers.CharField(write_only=True)
    class Meta:
        model = models.Publish
        fields = ('name', 're_name', 'address')
    def validate(self, attrs):
        name = attrs.get('name')
        re_name = attrs.pop('re_name')  # 剔除
        if name != re_name:
            raise serializers.ValidationError({'re_name': '确认名字有误'})
        return attrs
```

# 模型类中自定义序列化深度

```
# model类中自定义插拔的外键序列化字段，可以采用外键关联表的序列化类来完成深度查询
class Book(BaseModel):
    # ...
    @property
    def publish_detail(self):
        from . import serializers
        data = serializers.PublishModelSerializer(self.publish).data
        return data
```

# 接口操作总结

```
# 单查群查、单删群删、单增群增、单改群改
```

## 路由层：api/url.py

```
from django.conf.urls import url
from . import views
urlpatterns = [
    # ...
    url(r'^v3/books/$', views.BookV3APIView.as_view()),
    url(r'^v3/books/(?P<pk>.*)/$', views.BookV3APIView.as_view()),
]
```

## 模型层：api/models.py

```
# 修改部分：取消book类 name 与 publish 联合唯一，
from django.db import models
from utils.model import BaseModel
class Book(BaseModel):
    name = models.CharField(max_length=64)
    price = models.DecimalField(max_digits=5, decimal_places=2)
    img = models.ImageField(upload_to='img', default='img/default.png')
    publish = models.ForeignKey(to='Publish', null=True,
                                related_name='books',
                                db_constraint=False,
                                on_delete=models.DO_NOTHING,
                                )
    authors = models.ManyToManyField(to='Author', null=True,
                                     related_name='books',
                                     db_constraint=False,
                                     )

    @property
    def publish_name(self):
        return self.publish.name

    @property
    def authors_info(self):
        author_list = []
        for author in self.authors.all():
            author_list.append({
                'name': author.name,
                'age': author.age,
                'mobile': author.detail.mobile
            })
        return author_list

    @property
    def publish_bac(self):
        from . import serializers
        data = serializers.PublishModelSerializer(self.publish).data
        return data

    class Meta:
        db_table = 'old_boy_book'
        verbose_name = '书籍'
        verbose_name_plural = verbose_name
        # 联合唯一
        # unique_together = ('name', 'publish')

    def __str__(self):
        return self.name


class Publish(BaseModel):
    name = models.CharField(max_length=64)
    address = models.CharField(max_length=64)

    class Meta:
        db_table = 'old_boy_publish'
        verbose_name = '出版社'
        verbose_name_plural = verbose_name

    def __str__(self):
        return self.name


class Author(BaseModel):
    name = models.CharField(max_length=64)
    age = models.IntegerField()

    @property
    def mobile(self):
        return self.detail.mobile

    class Meta:
        db_table = 'old_boy_author'
        verbose_name = '作者'
        verbose_name_plural = verbose_name

    def __str__(self):
        return self.name


class AuthorDetail(BaseModel):
    mobile = models.CharField(max_length=11)

    author = models.OneToOneField(to='Author', null=True,
                                  related_name='detail',
                                  db_constraint=False,
                                  on_delete=models.CASCADE
                                  )

    class Meta:
        db_table = 'old_boy_author_detail'
        verbose_name = '作者详情'
        verbose_name_plural = verbose_name

    def __str__(self):
        return '%s的详情' % self.author.name
```

## modelserializer实现群增加和修改序列化层：api/serializers.py

```
# 群增与群改反序列化实现
# 1）ModelSerializer默认不通过群改功能，需要在Meta中设置 list_serializer_class
# 2）自定义ListSerializer子类，重写update方法，将子类绑定给 list_serializer_class
# 3）重写update方法中通过 代表要更新的对象们instance 及 提供的更新数据们validated_data
#       得到 更新后的对象们instance 返回
class BookV3ListSerializer(serializers.ListSerializer):
    def update(self, instance, validated_data):
        '''
        :param instance: [book_obj1, ..., book_obj2]
        :param validated_data: [{更新数据的字段}, ..., {更新数据的字段}]
        :return: [book_new_obj1, ..., book_new_obj2]
        '''
        for index, obj in enumerate(instance):  # type: int, models.Book
            # 单个对象数据更新 - 一个个属性更新值
            for attr, value in validated_data[index].items():
                # 对象有更新数据字典的key对应的属性，才完成该属性的值更新
                if hasattr(obj, attr):
                    setattr(obj, attr, value)
            # 信息同步到数据库
            obj.save()
        return instance

class BookV3ModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.Book
        fields = ('name', 'price', 'publish', 'authors', 'img', 'publish_name', 'authors_info')
        list_serializer_class = BookV3ListSerializer
        extra_kwargs = {
            'publish': {
                'required': True,
                'write_only': True
            },
            'authors': {
                'required': True,
                'write_only': True
            },
            'img': {
                'read_only': True
            }
        }
    def validate_name(self, value):
        if 'sb' in value:
            raise serializers.ValidationError('书名有敏感词汇')
        return value
    def validate(self, attrs):
        name = attrs.get('name')
        publish = attrs.get('publish')
        if models.Book.objects.filter(name=name, publish=publish):
            raise serializers.ValidationError({'book': '书籍以存在'})
        return attrs
```

## 视图层：api/views.py

```
class BookV3APIView(APIView):
    # 单查群查
    def get(self, request, *args, **kwargs):
        pk = kwargs.get('pk')
        if pk:
            book_obj = models.Book.objects.filter(is_delete=False, pk=pk).first()
            if not book_obj:
                return APIResponse(1, 'pk error')
            book_ser = serializers.BookV3ModelSerializer(book_obj)
            return APIResponse(0, 'ok', results=book_ser.data)
        book_query = models.Book.objects.filter(is_delete=False).all().order_by('-id')
        book_ser = serializers.BookV3ModelSerializer(book_query, many=True)
        return APIResponse(0, 'ok', results=book_ser.data)
    
    
    # 单增群增
    def post(self, request, *args, **kwargs):
        # 单增 /books/  data  {}
        # 群增 /books/  data  [{}, ..., {}]
        request_data = request.data
        if isinstance(request_data, dict):
            data = [request_data, ]
        elif isinstance(request_data, list):
            data = request_data
        else:
            return APIResponse(1, '数据格式有误')
        book_ser = serializers.BookV3ModelSerializer(data=data, many=True)
        if book_ser.is_valid():
            book_obj_list = book_ser.save()
            return APIResponse(0, 'ok',
                results=serializers.BookV3ModelSerializer(book_obj_list, many=True).data
            )
        else:
            return APIResponse(1, '添加失败', results=book_ser.errors)

        
    """
    1）先确定要更新的对象 主键们 与 数据们
    2）通过校验数据库剔除 已删除的对象 与 不存在的对象
        主键们 => 剔除过程 => 可以修改的对象们
        数据们 => 剔除过程 => 可以修改的对象们对应的数据们
    3）反序列化及校验过程
        通过 => save() 完成更新
        失败 => ser_obj.errors 返回
    """
    # 单改群改
    def patch(self, request, *args, **kwargs):
        # 单改 /books/(pk)/  data  {"name": "new_name", ...}
        # 群改 /books/  data  [{"pk": 1, "name": "new_name", ...}, ...,{"pk": n, "name": "new_name", ...}]
        # 结果：
        # pks = [1, ..., n] => book_query => instance
        # data = [{"name": "new_name", ...}, ..., {"name": "new_name", ...}] => data

        # 数据预处理
        pk = kwargs.get('pk')
        request_data = request.data
        if pk:
            if not isinstance(request_data, dict):
                return APIResponse(1, '单改数据有误')
            pks = [pk, ]
            data = [request_data, ]
        elif isinstance(request_data, list):
            try:
                pks = []
                for dic in request_data:
                    pks.append(dic.pop('pk'))
                data = request_data
            except:
                return APIResponse(1, '群改数据有误')
        else:
            return APIResponse(1, '数据格式有误')

        # 将 已删除的书籍 与 不存在的书籍 剔除 (提供修改的数据相对应也剔除)
        book_query = []
        filter_data = []
        for index, pk in enumerate(pks):
            book_obj = models.Book.objects.filter(is_delete=False, pk=pk).first()
            if book_obj:
                book_query.append(book_obj)
                filter_data.append(data[index])
        # 反序列化完成数据的修改
        book_ser = serializers.BookV3ModelSerializer(instance=book_query, data=filter_data, many=True, partial=True)
        if book_ser.is_valid():
            book_obj_list = book_ser.save()
            return APIResponse(1, 'ok',
                results=serializers.BookV3ModelSerializer(book_obj_list, many=True).data
            )
        else:
            return APIResponse(1, '更新失败', results=book_ser.errors)

    # 单删群删  
    def delete(self, request, *args, **kwargs):
        # 单删 /books/(pk)/
        # 群删 /books/  数据包携带 pks => request.data
        pk = kwargs.get('pk')
        if pk:
            pks = [pk]
        else:
            pks = request.data.get('pks')
        if not pks:
            return APIResponse(1, '删除失败')
        book_query = models.Book.objects.filter(is_delete=False, pk__in=pks)
        if not book_query.update(is_delete=True):  # 操作的记录大于0就记录删除成功
            return APIResponse(1, '删除失败')
        return APIResponse(0, '删除成功')
```

