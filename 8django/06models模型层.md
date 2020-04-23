[TOC]

#  ORM简介

查询数据层次图解：如果操作mysql，ORM是在pymysq之上又进行了一层封装

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191024170422849-557823843.png)



- MVC或者MTV框架中包括一个重要的部分，就是ORM，它实现了数据模型与数据库的解耦，即数据模型的设计不需要依赖于特定的数据库，通过简单的配置就可以轻松更换数据库，这极大的减轻了开发人员的工作量，不需要面对因数据库变更而导致的无效劳动
- ORM是“对象-关系-映射”的简称。

```
#sql中的表                                                      

 #创建表:
     CREATE TABLE employee(                                     
                id INT PRIMARY KEY auto_increment ,                    
                name VARCHAR (20),                                      
                gender BIT default 1,                                  
                birthday DATA ,                                         
                department VARCHAR (20),                                
                salary DECIMAL (8,2) unsigned,                          
              );


  #sql中的表纪录                                                  

  #添加一条表纪录:                                                          
      INSERT employee (name,gender,birthday,salary,department)            
             VALUES   ("alex",1,"1985-12-12",8000,"保洁部");               

  #查询一条表纪录:                                                           
      SELECT * FROM employee WHERE age=24;                               

  #更新一条表纪录:                                                           
      UPDATE employee SET birthday="1989-10-24" WHERE id=1;              

  #删除一条表纪录:                                                          
      DELETE FROM employee WHERE name="alex"                             





#python的类
class Employee(models.Model):
     id=models.AutoField(primary_key=True)
     name=models.CharField(max_length=32)
     gender=models.BooleanField()
     birthday=models.DateField()
     department=models.CharField(max_length=32)
     salary=models.DecimalField(max_digits=8,decimal_places=2)


 #python的类对象
      #添加一条表纪录:
          emp=Employee(name="alex",gender=True,birthday="1985-12-12",epartment="保洁部")
          emp.save()
      #查询一条表纪录:
          Employee.objects.filter(age=24)
      #更新一条表纪录:
          Employee.objects.filter(id=1).update(birthday="1989-10-24")
      #删除一条表纪录:
          Employee.objects.filter(name="alex").delete()
```

# Django ORM中常用字段和参数



一些说明：

- 表myapp_person的名称是自动生成的，如果你要自定义表名，需要在model的Meta类中指定 db_table 参数，强烈建议使用小写表名，特别是使用MySQL作为后端数据库时。
- id字段是自动添加的，如果你想要指定自定义主键，只需在其中一个字段中指定 primary_key=True 即可。如果Django发现你已经明确地设置了Field.primary_key，它将不会添加自动ID列。
- 本示例中的CREATE TABLE SQL使用PostgreSQL语法进行格式化，但值得注意的是，Django会根据配置文件中指定的数据库后端类型来生成相应的SQL语句。
- Django支持MySQL5.5及更高版本。

## 常用字段

### AutoField

int自增列，必须填入参数 primary_key=True。当model中如果没有自增列，则自动会创建一个列名为id的列。

### IntegerField

一个整数类型,范围在 -2147483648 to 2147483647。(一般不用它来存手机号(位数也不够)，直接用字符串存，)

### CharField

字符类型，必须提供max_length参数， max_length表示字符长度。

这里需要知道的是Django中的CharField对应的MySQL数据库中的varchar类型，没有设置对应char类型的字段，但是Django允许我们自定义新的字段，下面我来自定义对应于数据库的char类型

![img](https://images2018.cnblogs.com/blog/1342004/201806/1342004-20180620142424835-1741683926.png)







### DateField

日期字段，日期格式  YYYY-MM-DD，相当于Python中的datetime.date()实例。

### DateTimeField

日期时间字段，格式 YYYY-MM-DD HH:MM[:ss[.uuuuuu]][TZ]，相当于Python中的datetime.datetime()实例。



## 字段参数

### null

用于表示某个字段可以为空。

### **unique**

如果设置为unique=True 则该字段在此表中必须是唯一的 。

### **db_index**

如果db_index=True 则代表着为此字段设置索引。

### **default**

为该字段设置默认值。

## DateField和DateTimeField

### auto_now_add

配置auto_now_add=True，创建数据记录的时候会把当前时间添加到数据库。

### auto_now

配置上auto_now=True，每次更新数据记录的时候会更新该字段。

# 关系字段

## ForeignKey

外键类型在ORM中用来表示外键关联关系，一般把ForeignKey字段设置在 '一对多'中'多'的一方。

ForeignKey可以和其他表做关联关系同时也可以和自身做关联关系。

### 字段参数

#### to

设置要关联的表

#### to_field

设置要关联的表的字段

#### **on_delete**

当删除关联表中的数据时，当前表与其关联的行的行为。

**models.CASCADE**

删除关联数据，与之关联也删除

#### db_constraint

是否在数据库中创建外键约束，默认为True。



其余字段参数

```
models.DO_NOTHING
删除关联数据，引发错误IntegrityError


models.PROTECT
删除关联数据，引发错误ProtectedError


models.SET_NULL
删除关联数据，与之关联的值设置为null（前提FK字段需要设置为可空）


models.SET_DEFAULT
删除关联数据，与之关联的值设置为默认值（前提FK字段需要设置默认值）


models.SET

删除关联数据，
a. 与之关联的值设置为指定值，设置：models.SET(值)
b. 与之关联的值设置为可执行对象的返回值，设置：models.SET(可执行对象)
```



## OneToOneField

一对一字段。

通常一对一字段用来扩展已有字段。(通俗的说就是一个人的所有信息不是放在一张表里面的，简单的信息一张表，隐私的信息另一张表，之间通过一对一外键关联)

### 字段参数

#### to

设置要关联的表。

#### to_field

设置要关联的字段。

#### on_delete

当删除关联表中的数据时，当前表与其关联的行的行为。(参考上面的例子)

## ManyToManyField

用于表示多对多的关联关系。在数据库中通过第三张表来建立关联关系

#### to

设置要关联的表

#### related_name

同ForeignKey字段。

#### related_query_name

同ForeignKey字段。

#### symmetrical

仅用于多对多自关联时，指定内部是否创建反向操作的字段。默认为True。

举个例子：

```
class Person(models.Model):
    name = models.CharField(max_length=16)
    friends = models.ManyToManyField("self")
```

此时，person对象就没有person_set属性。

```
class Person(models.Model):
    name = models.CharField(max_length=16)
    friends = models.ManyToManyField("self", symmetrical=False)
```

此时，person对象现在就可以使用person_set属性进行反向查询。

#### through

在使用ManyToManyField字段时，Django将自动生成一张表来管理多对多的关联关系。

但我们也可以手动创建第三张表来管理多对多关系，此时就需要通过through来指定第三张表的表名。

#### **through_fields**

设置关联的字段。

#### db_table

默认创建第三张表时，数据库中表的名称。

##  多对多关联关系的三种方式

### 方式一：自行创建第三张表



```
class Book(models.Model):
    title = models.CharField(max_length=32, verbose_name="书名")


class Author(models.Model):
    name = models.CharField(max_length=32, verbose_name="作者姓名")


# 自己创建第三张表，分别通过外键关联书和作者
class Author2Book(models.Model):
    author = models.ForeignKey(to="Author")
    book = models.ForeignKey(to="Book")

    class Meta:
        unique_together = ("author", "book")
```

### 方式二：通过ManyToManyField自动创建第三张表

```
class Book(models.Model):
    title = models.CharField(max_length=32, verbose_name="书名")


# 通过ORM自带的ManyToManyField自动创建第三张表
class Author(models.Model):
    name = models.CharField(max_length=32, verbose_name="作者姓名")
    books = models.ManyToManyField(to="Book", related_name="authors")
```

### 方式三：设置ManyTomanyField并指定自行创建的第三张表

```
class Book(models.Model):
    title = models.CharField(max_length=32, verbose_name="书名")


# 自己创建第三张表，并通过ManyToManyField指定关联
class Author(models.Model):
    name = models.CharField(max_length=32, verbose_name="作者姓名")
    books = models.ManyToManyField(to="Book", through="Author2Book", through_fields=("author", "book"))
    # through_fields接受一个2元组（'field1'，'field2'）：
    # 其中field1是定义ManyToManyField的模型外键的名（author），field2是关联目标模型（book）的外键名。


class Author2Book(models.Model):
    author = models.ForeignKey(to="Author")
    book = models.ForeignKey(to="Book")

    class Meta:
        unique_together = ("author", "book")
```





 

注意：

当我们需要在第三张关系表中存储额外的字段时，就要使用第三种方式。

但是当我们使用第三种方式创建多对多关联关系时，就无法使用set、add、remove、clear方法来管理多对多的关系了，需要通过第三张表的model来管理多对多关系。



##  元信息

ORM对应的类里面包含另一个Meta类，而Meta类封装了一些数据库的信息。主要字段如下:

#### **db_table**

ORM在数据库中的表名默认是 **app_**类名，可以通过**db_table**可以重写表名。

#### index_together

联合索引。

#### unique_together

联合唯一索引。

#### ordering

指定默认按什么字段排序。

只有设置了该属性，我们查询到的结果才可以被reverse()。





```
    class UserInfo(models.Model):
        nid = models.AutoField(primary_key=True)
        username = models.CharField(max_length=32)

        class Meta:
            # 数据库中生成的表名称 默认 app名称 + 下划线 + 类名
            db_table = "table_name"

            # 联合索引
            index_together = [
                ("pub_date", "deadline"),
            ]

            # 联合唯一索引
            unique_together = (("driver", "restaurant"),)
            
            ordering = ('name',)
            
            # admin中显示的表名称
            verbose_name='哈哈'

            # verbose_name加s
            verbose_name_plural=verbose_name
```





## 自定义字段（了解）

自定义字段在实际项目应用中可能会经常用到，这里需要对他留个印象！

自定义char类型字段：

```
from django.db import models

# Create your models here.
#Django中没有对应的char类型字段，但是我们可以自己创建
class FixCharField(models.Field):
    '''
    自定义的char类型的字段类
    '''
    def __init__(self,max_length,*args,**kwargs):
        self.max_length=max_length
        super().__init__(max_length=max_length,*args,**kwargs)

    def db_type(self, connection):
        '''
        限定生成的数据库表字段类型char，长度为max_length指定的值
        :param connection:
        :return:
        '''
        return 'char(%s)'%self.max_length
#应用上面自定义的char类型
class Class(models.Model):
    id=models.AutoField(primary_key=True)
    title=models.CharField(max_length=32)
    class_name=FixCharField(max_length=16)
    gender_choice=((1,'男'),(2,'女'),(3,'保密'))
    gender=models.SmallIntegerField(choicesgender_choice,default=3)
```



## 字段合集（争取记忆）

```
AutoField(Field)
        - int自增列，必须填入参数 primary_key=True

    BigAutoField(AutoField)
        - bigint自增列，必须填入参数 primary_key=True

        注：当model中如果没有自增列，则自动会创建一个列名为id的列
        from django.db import models

        class UserInfo(models.Model):
            # 自动创建一个列名为id的且为自增的整数列
            username = models.CharField(max_length=32)

        class Group(models.Model):
            # 自定义自增列
            nid = models.AutoField(primary_key=True)
            name = models.CharField(max_length=32)

    SmallIntegerField(IntegerField):
        - 小整数 -32768 ～ 32767

    PositiveSmallIntegerField(PositiveIntegerRelDbTypeMixin, IntegerField)
        - 正小整数 0 ～ 32767
    IntegerField(Field)
        - 整数列(有符号的) -2147483648 ～ 2147483647

    PositiveIntegerField(PositiveIntegerRelDbTypeMixin, IntegerField)
        - 正整数 0 ～ 2147483647

    BigIntegerField(IntegerField):
        - 长整型(有符号的) -9223372036854775808 ～ 9223372036854775807

    BooleanField(Field)
        - 布尔值类型

    NullBooleanField(Field):
        - 可以为空的布尔值

    CharField(Field)
        - 字符类型
        - 必须提供max_length参数， max_length表示字符长度

    TextField(Field)
        - 文本类型

    EmailField(CharField)：
        - 字符串类型，Django Admin以及ModelForm中提供验证机制

    IPAddressField(Field)
        - 字符串类型，Django Admin以及ModelForm中提供验证 IPV4 机制

    GenericIPAddressField(Field)
        - 字符串类型，Django Admin以及ModelForm中提供验证 Ipv4和Ipv6
        - 参数：
            protocol，用于指定Ipv4或Ipv6， 'both',"ipv4","ipv6"
            unpack_ipv4， 如果指定为True，则输入::ffff:192.0.2.1时候，可解析为192.0.2.1，开启此功能，需要protocol="both"

    URLField(CharField)
        - 字符串类型，Django Admin以及ModelForm中提供验证 URL

    SlugField(CharField)
        - 字符串类型，Django Admin以及ModelForm中提供验证支持 字母、数字、下划线、连接符（减号）

    CommaSeparatedIntegerField(CharField)
        - 字符串类型，格式必须为逗号分割的数字

    UUIDField(Field)
        - 字符串类型，Django Admin以及ModelForm中提供对UUID格式的验证

    FilePathField(Field)
        - 字符串，Django Admin以及ModelForm中提供读取文件夹下文件的功能
        - 参数：
                path,                      文件夹路径
                match=None,                正则匹配
                recursive=False,           递归下面的文件夹
                allow_files=True,          允许文件
                allow_folders=False,       允许文件夹

    FileField(Field)
        - 字符串，路径保存在数据库，文件上传到指定目录
        - 参数：
            upload_to = ""      上传文件的保存路径
            storage = None      存储组件，默认django.core.files.storage.FileSystemStorage

    ImageField(FileField)
        - 字符串，路径保存在数据库，文件上传到指定目录
        - 参数：
            upload_to = ""      上传文件的保存路径
            storage = None      存储组件，默认django.core.files.storage.FileSystemStorage
            width_field=None,   上传图片的高度保存的数据库字段名（字符串）
            height_field=None   上传图片的宽度保存的数据库字段名（字符串）

    DateTimeField(DateField)
        - 日期+时间格式 YYYY-MM-DD HH:MM[:ss[.uuuuuu]][TZ]

    DateField(DateTimeCheckMixin, Field)
        - 日期格式      YYYY-MM-DD

    TimeField(DateTimeCheckMixin, Field)
        - 时间格式      HH:MM[:ss[.uuuuuu]]

    DurationField(Field)
        - 长整数，时间间隔，数据库中按照bigint存储，ORM中获取的值为datetime.timedelta类型

    FloatField(Field)
        - 浮点型

    DecimalField(Field)
        - 10进制小数
        - 参数：
            max_digits，小数总长度
            decimal_places，小数位长度

    BinaryField(Field)
        - 二进制类型
```

## 字段属性

```python
(1)null
 
如果为True，Django 将用NULL 来在数据库中存储空值。 默认值是 False.
 
(1)blank
 
如果为True，该字段允许不填。默认为False。
要注意，这与 null 不同。null纯粹是数据库范畴的，而 blank 是数据验证范畴的。
如果一个字段的blank=True，表单的验证将允许该字段是空值。如果字段的blank=False，该字段就是必填的。
 
(2)default
 
字段的默认值。可以是一个值或者可调用对象。如果可调用 ，每有新对象被创建它都会被调用。
 
(3)primary_key
 
如果为True，那么这个字段就是模型的主键。如果你没有指定任何一个字段的primary_key=True，
Django 就会自动添加一个IntegerField字段做为主键，所以除非你想覆盖默认的主键行为，
否则没必要设置任何一个字段的primary_key=True。
 
(4)unique
 
如果该值设置为 True, 这个数据字段的值在整张表中必须是唯一的
 
(5)choices
由二元组组成的一个可迭代对象（例如，列表或元组），用来给字段提供选择项。 如果设置了choices ，默认的表单将是一个选择框而不是标准的文本框，<br>而且这个选择框的选项就是choices 中的选项。     
```



### orm字段与MySQL字段对应关系

```
对应关系：
    'AutoField': 'integer AUTO_INCREMENT',
    'BigAutoField': 'bigint AUTO_INCREMENT',
    'BinaryField': 'longblob',
    'BooleanField': 'bool',
    'CharField': 'varchar(%(max_length)s)',
    'CommaSeparatedIntegerField': 'varchar(%(max_length)s)',
    'DateField': 'date',
    'DateTimeField': 'datetime',
    'DecimalField': 'numeric(%(max_digits)s, %(decimal_places)s)',
    'DurationField': 'bigint',
    'FileField': 'varchar(%(max_length)s)',
    'FilePathField': 'varchar(%(max_length)s)',
    'FloatField': 'double precision',
    'IntegerField': 'integer',
    'BigIntegerField': 'bigint',
    'IPAddressField': 'char(15)',
    'GenericIPAddressField': 'char(39)',
    'NullBooleanField': 'bool',
    'OneToOneField': 'integer',
    'PositiveIntegerField': 'integer UNSIGNED',
    'PositiveSmallIntegerField': 'smallint UNSIGNED',
    'SlugField': 'varchar(%(max_length)s)',
    'SmallIntegerField': 'smallint',
    'TextField': 'longtext',
    'TimeField': 'time',
    'UUIDField': 'char(32)',
```



# choice参数

```python
choice参数
	可以用做
        用户性别
        用户学历
        用户工作状态
        客户来源
        ...
class Userinfo(models.Model):
    name = models.CharField(max_length=32)
    password = models.IntegerField(default=123)
    choice = (
        (1,'male'),
        (2,'female'),
        (3,'other')
    )
    gender = models.IntegerField(choices=choice)
        """
        1.如果我存的是上面元组中数字会怎么样
        2.如果我存的数字不在元组范围内又会怎样
            1.数字没有对应关系 是可以存的
        """
  
```

前端获取

```python
{{ user_obj.get_gender_display }}
```



后端获取

```python
      
        
from app01 import models
ser_obj = models.Userinfo.objects.filter(pk=4).first()
        print(user_obj.username)
        print(user_obj.gender)
        # 针对choices字段 如果你想要获取数字所对应的中文 你不能直接点字段
        # 固定句式   数据对象.get_字段名_display()  当没有对应关系的时候 该句式获取到的还是数字
        print(user_obj.get_gender_display())
```

可以这样使用

```python
record_choices = (('checked', "已签到"),
                      ('vacate', "请假"),
                      ('late', "迟到"),
                      ('noshow', "缺勤"),
                      ('leave_early', "早退"),
                      )
        record = models.CharField("上课纪录", choices=record_choices, default="checked", 
    
    
        
        
        score_choices = ((100, 'A+'),
                     (90, 'A'),
                     (85, 'B+'),
                     (80, 'B'),
                     (70, 'B-'),
                     (60, 'C+'),
                     (50, 'C'),
                     (40, 'C-'),
                     (0, ' D'),
                     (-1, 'N/A'),
                     (-100, 'COPY'),
                     (-1000, 'FAIL'),
                     )
        score = models.IntegerField("本节成绩", choices=score_choices, default=-1)
```



# 数据模型定义（批量操作数据）

```
from django.db import models
# Create your models here.
class Test(models.Model):
    name = models.CharField(max_length=32, null=True, default=None)
    age = models.IntegerField(max_length=32, null=True, default=None)
```

![img](https://img2018.cnblogs.com/blog/1445976/201811/1445976-20181107143201826-1148065607.jpg)

## 在urls.py文件添加一个路径

```
urlpatterns = [
    path('admin/', admin.site.urls),
    path("favicon.ico", RedirectView.as_view(url='static/favicon.ico')),
    re_path('^index/',views.index),
]
```

## 在views.py添加数据

### 批量添加单个字段

```python
def index(request):
    product_list_to_insert = list()
	for i in range(1000):
        product_list_to_insert.append(models.Test(name='123'))
    models.Test.objects.bulk_create(product_list_to_insert)
    
```



### 批量添加多个字段

```
# ############### 添加数据 ###############    
def index(request):
    import random

    product_list_to_insert = list()
    for x in range(100):
       #注意下面的添加方式 product_list_to_insert.append(models.Test(name='apollo' + str(x), age=random.randint(18, 89)))
    models.Test.objects.bulk_create(product_list_to_insert)
    return render(request, 'index.html')
```



## 批量更新数据

```
批量更新数据时，先进行数据过滤，然后再调用update方法进行一次性地更新。下面的语句将生成类似update....frrom....的SQL语句。
# ############### 更新数据 ###############
Test.objects.filter(name__contains='apollo1').update(name='Jack')
```

## 批量删除数据

```
批量更新数据时，先是进行数据过滤，然后再调用delete方法进行一次性删除。
下面的语句讲生成类似delete from ... where ... 的SQL语句。
# ############### 删除数据 ###############
Test.objects.filter(name__contains='jack').delete()
```

# 创建多对多表关系的三种方式

## 方式1：全自动创建（推荐使用**）

优点：好处在于 django orm会自动帮你创建第三张关系表
缺点：但是它只会帮你创建两个表的关系字段（俩个book_id,author_id） 不会再额外添加字段
            虽然方便 但是第三张表的扩展性较差  无法随意的添加额外的字段
           

```
class Book(models.Model):
    ...
    authors = models.ManyToManyField(to='Author')

class Author(models.Models):
	...
```

## 方式2：纯手动（不推荐使用）

自己手动添加外键

优点：好处在于第三张表是可以任意的添加额外的字段

缺点：不足之处是在于orm查询的时候，很多方法不支持，查询的时候非常的麻烦



```python
class Book(models.Model):
                ...
                
class Author(models.Models):
	...
            
class Book2Author(models.Model):
	book_id = models.ForeignKey(to='Book')
	author_id = models.ForeignKey(to='Author')
	create_time = models.DateField(auto_now_add=True)
```

## 方式3半自动(推荐使用，扩展性高)

3.半自动(推荐使用******)
手动建表 但是你会告诉orm 第三张表是你自己建的
orm只需要给我提供方便的查询方法
第三种虽然可以使用orm查询方法，但是不支持使用下面的方法：
    add()
    set()
    remove()
    clear()

```python
class Book(models.Model):
    ...
    authors = models.ManyToManyField(to='Author', through='Book2Author', through_fields=('book','author'))#告诉django,是和那个表多对多关联，关联表是什么，关联的字段是什么
class Author(models.Model):
    ...
    #books = models.ManyToManyField(to='Book', through='Book2Author', through_fields=('author', 'book'))
class Book2Author(models.Model):
    book = models.ForeignKey(to='Book')
    author = models.ForeignKey(to='Author')
    create_time = models.DateField(auto_now_add=True)#建立额外的字段
```

**注意**

1. 半自动 一定要加两个额外的参数： through='Book2Author', through_fields=('book','author')
2. 后面字段的顺序：由第三张表通过哪个字段查询单表 就把哪个字段放前面
3. 在设计项目的时候 一定要给自己留后路 防止后续的迭代更新 ，因此推荐使用方法3







