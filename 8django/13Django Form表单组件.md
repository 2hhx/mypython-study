[TOC]



# Form介绍 

我们之前在HTML页面中利用form表单向后端提交数据时，都会写一些获取用户输入的标签并且用form标签把它们包起来。

与此同时我们在好多场景下都需要对用户的输入做校验，比如校验用户是否输入，输入的长度和格式等正不正确。如果用户输入的内容有错误就需要在页面上相应的位置显示对应的错误信息.。

Django form组件就实现了上面所述的功能。

总结一下，其实form组件的主要功能如下:

- 生成页面可用的HTML标签
- 对用户提交的数据进行校验
- 保留上次输入内容



# form组件的作用

1.搭建前端页面                                   								>>>                  渲染页面
2.获取前端用户提交的数据校验                    					 >>>                  校验数据
3.对数据的校验的结果 展示到前端页面给用户查看    	 >>>                  展示错误信息

form组件能够自动帮你完成上面的三件事
1.渲染页面
2.校验数据
3.展示错误信息

## 自定义form校验

### 简单的form校验组件

form校验组件
1.注册功能
用户输入的用户名中 不能包含金瓶梅
如果包含了 就提示用户 输入的内容不符合社会主义核心价值观
用户输入的密码 不能小于三位
如果密码少于三位  提示用户 密码太短了
(******) 
校验数据通常是前后端都有校验,前端校验可有可无，但是后端必须要由校验。

后端

```
def reg(request):
    back_dic = {'username':'',"password":''}
    if request.method=="POST":
        username = request.POST.get("username")
        password = request.POST.get('password')
        if '梅瓶金' in username:
            back_dic['username']='不符合要求'
        if len(password)<3:
            back_dic['password'] = '密码太短'
    return render(request,'reg.html',locals())
```

前端

```html
<form action="" method="post">
    <p>
        username:
        <input type="text" name="username">
        <span style="color: red">{{ back_dic.username }}</span>
    </p>
    <p>
        password:
        <input type="text" name="password">
        <span style="color: red">{{ back_dic.password }}</span>
    </p>
    <input type="submit">
</form>

```



# form组件的使用

```python

1.自己要先写一个类
from django import forms


class MyRegForm(forms.Form):
    username = forms.CharField(min_length=3,max_length=8)
    password = forms.CharField(min_length=3,max_length=8)
    email = forms.EmailField()
    
```

## 如何校验数据

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191029193445587-449630073.png)

```python

from app01 import views
# 1.给自定义的类传一个字典
obj =views.MyRegForm({'username':'jason','password':'12','email':'123'})
# 2.判断数据是否全部合法
obj.is_valid()  # 只有数据全部符合要求才会是True
Out[4]: False
# 3.查看符合校验规则的数据
obj.cleaned_data
Out[5]: {'username': 'jason'}
# 4.查看不符合条件的数据以及不符合的原因是什么
obj.errors
Out[6]: { 'password': ['Ensure this value has at least 3 characters (it has 2).'],'email': ['Enter a valid email address.'] }
# 5.校验数据的时候 默认情况下类里面所有的字段都必须传值
obj = views.MyRegForm({'username':'jason','password':'123'})
obj.is_valid()
Out[12]: False
obj.errors
Out[13]: {'email': ['This field is required.']}
# 6.默认情况下可以多传 但是绝对不能少传
obj=views.MyRegForm({'username':'jason','password':'1233','email':'123@qq.com','xxx':'ooo'})
obj.is_valid()
 Out[15]: True
```

## 如何渲染页面

forms组件只会帮你渲染获取用户输入(输入，选择，下拉框...)的标签  提交按钮需要你自己手动写

### 渲染方式1

```python
<p>第一种渲染前端页面的方式:封装程度太高了 标签样式及参数不方便调整 可扩展性差(不推荐使用)
{{ forma.as_p }}   或者
{{ forma.as_ul }}
{{ forma.as_table}}
</p>
```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191029195330099-666094723.png)

### 渲染方式2

```python
<p>第二种渲染页面的方式:扩展性较高 不足之处在于 需要你手写的代码量比较多(不推荐使用)</p>
<p>{{ forma.username.label }}:{{ forma.username }}</p>
<p>{{ forma.password.label }}:{{ forma.password }}</p>
<p>{{ forma.email.label }}:{{ forma.email }}</p>
```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191029195805611-1885604201.png)

### 渲染方式3(推荐使用)

```python

<p>第三种渲染前端页面的方式:代码量和扩展性都很高(推荐使用)</p>
{% for foo in forma %}
<p>{{ foo.label }}{{ foo }}</p>
{% endfor %}
```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191029200130918-820799754.png)





# form组件的校验

## 如何展示错误信息



#### 后端

```python
from django import forms
class MyRegForm(forms.Form):
    username = forms.CharField(min_length=3,max_length=8)
    password = forms.CharField(min_length=3,max_length=8)
    email = forms.EmailField()
    
    
def formm(request):
    form_obj = MyRegForm()
    if request.method=="POST":
        form_obj = MyRegForm(request.POST)
        if form_obj.is_valid():
            return HttpResponse('你上传的数据没有问题')

    return render(request,'formm.html',locals())
```

#### 前端

**如何取消前端帮我们做的校验 form表单中添加一个参数novalidate即可
           ` <form action="" method="post" novalidate>`**

**展示错误信息   用对象点errors.0**

```html
<form action="" method="post" novalidate>
    {% for foo in form_obj %}
        <p>{{ foo.label }}:{{ foo }}
            <span style="color: red">{{ foo.errors.0}}</span>
        </p>
    {% endfor %}
    <input type="submit">
</form>

```



![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191029213424012-2068046569.png)

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191029213527711-1821336700.png)





## 展示中文错误信息

`from django.forms import widgets`添加样式，可以是bootstrap样式类

`required`设置是否可以为空，默认为true

```python
from django import forms
from django.forms import widgets  # 当你forms点不出来的时候 你可以手动导入一下
from django.core.validators import RegexValidator
class MyRegForm(forms.Form):
username = forms.CharField(min_length=3,max_length=8,label='用户名',
                           error_messages={
                               'min_length':'用户名最短三位',
                               'max_length':'用户名最长八位',
                               'required':'用户名不能为空'
                           },initial='我是初始值',required=False,
                           widget= widgets.TextInput(attrs={'class':'form-control others'})
                           )
password = forms.CharField(min_length=3,max_length=8,label='密码',error_messages={
                               'min_length':'密码最短三位',
                               'max_length':'密码最长八位',
                               'required':'密码不能为空'
                           },widget=widgets.PasswordInput())
confirm_password = forms.CharField(min_length=3, max_length=8, label='确认密码', error_messages={
    'min_length': '确认密码最短三位',
    'max_length': '确认密码最长八位',
    'required': '确认密码不能为空'
})
email = forms.EmailField(label='邮箱',error_messages={
        'required':'邮箱不能为空',
        'invalid':'邮箱格式不正确'
},required=False,widget=widgets.EmailInput(attrs={'class':'form-control'}))
#
```





# Hook方法(钩子方法)

除了上面两种方式，我们还可以在Form类中定义钩子函数，来实现自定义的验证功能。

## 局部钩子

我们在Fom类中定义 clean_字段名() 方法，就能够实现对特定字段进行校验。在写的**form表单类的内部**

举个例子：





```
# 当你需要对某一个字段数据进行额外的一些列校验 你可以考虑使用钩子函数
    # 针对单个字段的  使用局部钩子
    def clean_username(self):
        username = self.cleaned_data.get('username')
        if '金瓶梅' in username:
            # 给username字段下面提示错误信息
            raise ValidationError("用户名不符合社会主义核心价值观")
            # self.add_error('username','用户名不符合社会主义核心价值观')
        return username     #校验完之后需要进行返回

```

## 全局钩子

我们在Fom类中定义 clean() 方法，就能够实现对字段进行全局校验。定义在类之中，与局部钩子相同。

```
    # 针对多个字段的校验 使用全局钩子      eg:校验两次密码是否一致
    def clean(self):
        password = self.cleaned_data.get('password')
        confirm_password = self.cleaned_data.get('confirm_password')
        if not password == confirm_password:
            self.add_error('confirm_password','两次密码不一致')
        return self.cleaned_data 			 #校验完之后需要进行返回

```

# 普通方式手写注册功能

### views.py



```
# 注册
def register(request):
    error_msg = ""
    if request.method == "POST":
        username = request.POST.get("name")
        pwd = request.POST.get("pwd")
        # 对注册信息做校验
        if len(username) < 6:
            # 用户长度小于6位
            error_msg = "用户名长度不能小于6位"
        else:
            # 将用户名和密码存到数据库
            return HttpResponse("注册成功")
    return render(request, "register.html", {"error_msg": error_msg})
```

### login.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
</head>
<body>
<form action="/reg/" method="post">
    {% csrf_token %}
    <p>
        用户名:
        <input type="text" name="name">
    </p>
    <p>
        密码：
        <input type="password" name="pwd">
    </p>
    <p>
        <input type="submit" value="注册">
        <p style="color: red">{{ error_msg }}</p>
    </p>
</form>
</body>
</html>
```





# 使用form组件实现注册功能

### views.py

先定义好一个RegForm类：

```
from django import forms

# 按照Django form组件的要求自己写一个类
class RegForm(forms.Form):
    name = forms.CharField(label="用户名")
    pwd = forms.CharField(label="密码")
```

再写一个视图函数：





```
# 使用form组件实现注册方式
def register2(request):
    form_obj = RegForm()
    if request.method == "POST":
        # 实例化form对象的时候，把post提交过来的数据直接传进去
        form_obj = RegForm(request.POST)
        # 调用form_obj校验数据的方法
        if form_obj.is_valid():
            return HttpResponse("注册成功")
    return render(request, "register2.html", {"form_obj": form_obj})
```





### login2.html





```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册2</title>
</head>
<body>
    <form action="/reg2/" method="post" novalidate autocomplete="off">
        {% csrf_token %}
        <div>
            <label for="{{ form_obj.name.id_for_label }}">{{ form_obj.name.label }}</label>
            {{ form_obj.name }} {{ form_obj.name.errors.0 }}
        </div>
        <div>
            <label for="{{ form_obj.pwd.id_for_label }}">{{ form_obj.pwd.label }}</label>
            {{ form_obj.pwd }} {{ form_obj.pwd.errors.0 }}
        </div>
        <div>
            <input type="submit" class="btn btn-success" value="注册">
        </div>
    </form>
</body>
</html>
```



看网页效果发现 也验证了form的功能：
•	前端页面是form类的对象生成的                                      -->生成HTML标签功能
•	当用户名和密码输入为空或输错之后 页面都会提示        -->用户提交校验功能
•	当用户输错之后 再次输入 上次的内容还保留在input框   -->保留上次输入内容

# Form那些事儿

## 常用字段与插件

创建Form类时，主要涉及到 【字段】 和 【插件】，字段用于对用户请求数据的验证，插件用于自动生成HTML;

### initial

初始值，input框里面的初始值。





```
class LoginForm(forms.Form):
    username = forms.CharField(
        min_length=8,
        label="用户名",
        initial="张三"  # 设置默认值
    )
    pwd = forms.CharField(min_length=6, label="密码")
```





### error_messages

重写错误信息。





```
class LoginForm(forms.Form):
    username = forms.CharField(
        min_length=8,
        label="用户名",
        initial="张三",
        error_messages={
            "required": "不能为空",
            "invalid": "格式错误",
            "min_length": "用户名最短8位"
        }
    )
    pwd = forms.CharField(min_length=6, label="密码")
```





### password





```
class LoginForm(forms.Form):
    ...
    pwd = forms.CharField(
        min_length=6,
        label="密码",
        widget=forms.widgets.PasswordInput(attrs={'class': 'c1'}, render_value=True)
    )
```





### radioSelect

单radio值为字符串





```
class LoginForm(forms.Form):
    username = forms.CharField(
        min_length=8,
        label="用户名",
        initial="张三",
        error_messages={
            "required": "不能为空",
            "invalid": "格式错误",
            "min_length": "用户名最短8位"
        }
    )
    pwd = forms.CharField(min_length=6, label="密码")
    gender = forms..ChoiceField(
        choices=((1, "男"), (2, "女"), (3, "保密")),
        label="性别",
        initial=3,
        widget=forms.widgets.RadioSelect()
    )
```





### 单选Select





```
class LoginForm(forms.Form):
    ...
    hobby = forms.ChoiceField(
        choices=((1, "篮球"), (2, "足球"), (3, "双色球"), ),
        label="爱好",
        initial=3,
        widget=forms.widgets.Select()
    )
```





### 多选Select





```
class LoginForm(forms.Form):
    ...
    hobby = forms.MultipleChoiceField(
        choices=((1, "篮球"), (2, "足球"), (3, "双色球"), ),
        label="爱好",
        initial=[1, 3],
        widget=forms.widgets.SelectMultiple()
    )
```





### 单选checkbox





```
class LoginForm(forms.Form):
    ...
    keep = forms.ChoiceField(
        label="是否记住密码",
        initial="checked",
        widget=forms.widgets.CheckboxInput()
    )
```





### 多选checkbox





```
class LoginForm(forms.Form):
    ...
    hobby = forms.MultipleChoiceField(
        choices=((1, "篮球"), (2, "足球"), (3, "双色球"),),
        label="爱好",
        initial=[1, 3],
        widget=forms.widgets.CheckboxSelectMultiple()
    )
```





### choice字段注意事项

在使用选择标签时，需要注意choices的选项可以配置从数据库中获取，但是由于是静态字段 获取的值无法实时更新，需要重写构造方法从而实现choice实时更新。

方式一：





```
from django.forms import Form
from django.forms import widgets
from django.forms import fields

 
class MyForm(Form):
 
    user = fields.ChoiceField(
        # choices=((1, '上海'), (2, '北京'),),
        initial=2,
        widget=widgets.Select
    )
 
    def __init__(self, *args, **kwargs):
        super(MyForm,self).__init__(*args, **kwargs)
        # self.fields['user'].choices = ((1, '上海'), (2, '北京'),)
        # 或
        self.fields['user'].choices = models.Classes.objects.all().values_list('id','caption')
```





方式二：





```
from django import forms
from django.forms import fields
from django.forms import models as form_model

 
class FInfo(forms.Form):
    authors = form_model.ModelMultipleChoiceField(queryset=models.NNewType.objects.all())  # 多选
    # authors = form_model.ModelChoiceField(queryset=models.NNewType.objects.all())  # 单选
```





## Django Form所有内置字段

```
Field
    required=True,               是否允许为空
    widget=None,                 HTML插件
    label=None,                  用于生成Label标签或显示内容
    initial=None,                初始值
    help_text='',                帮助信息(在标签旁边显示)
    error_messages=None,         错误信息 {'required': '不能为空', 'invalid': '格式错误'}
    validators=[],               自定义验证规则
    localize=False,              是否支持本地化
    disabled=False,              是否可以编辑
    label_suffix=None            Label内容后缀
 
 
CharField(Field)
    max_length=None,             最大长度
    min_length=None,             最小长度
    strip=True                   是否移除用户输入空白
 
IntegerField(Field)
    max_value=None,              最大值
    min_value=None,              最小值
 
FloatField(IntegerField)
    ...
 
DecimalField(IntegerField)
    max_value=None,              最大值
    min_value=None,              最小值
    max_digits=None,             总长度
    decimal_places=None,         小数位长度
 
BaseTemporalField(Field)
    input_formats=None          时间格式化   
 
DateField(BaseTemporalField)    格式：2015-09-01
TimeField(BaseTemporalField)    格式：11:12
DateTimeField(BaseTemporalField)格式：2015-09-01 11:12
 
DurationField(Field)            时间间隔：%d %H:%M:%S.%f
    ...
 
RegexField(CharField)
    regex,                      自定制正则表达式
    max_length=None,            最大长度
    min_length=None,            最小长度
    error_message=None,         忽略，错误信息使用 error_messages={'invalid': '...'}
 
EmailField(CharField)      
    ...
 
FileField(Field)
    allow_empty_file=False     是否允许空文件
 
ImageField(FileField)      
    ...
    注：需要PIL模块，pip3 install Pillow
    以上两个字典使用时，需要注意两点：
        - form表单中 enctype="multipart/form-data"
        - view函数中 obj = MyForm(request.POST, request.FILES)
 
URLField(Field)
    ...
 
 
BooleanField(Field)  
    ...
 
NullBooleanField(BooleanField)
    ...
 
ChoiceField(Field)
    ...
    choices=(),                选项，如：choices = ((0,'上海'),(1,'北京'),)
    required=True,             是否必填
    widget=None,               插件，默认select插件
    label=None,                Label内容
    initial=None,              初始值
    help_text='',              帮助提示
 
 
ModelChoiceField(ChoiceField)
    ...                        django.forms.models.ModelChoiceField
    queryset,                  # 查询数据库中的数据
    empty_label="---------",   # 默认空显示内容
    to_field_name=None,        # HTML中value的值对应的字段
    limit_choices_to=None      # ModelForm中对queryset二次筛选
     
ModelMultipleChoiceField(ModelChoiceField)
    ...                        django.forms.models.ModelMultipleChoiceField
 
 
     
TypedChoiceField(ChoiceField)
    coerce = lambda val: val   对选中的值进行一次转换
    empty_value= ''            空值的默认值
 
MultipleChoiceField(ChoiceField)
    ...
 
TypedMultipleChoiceField(MultipleChoiceField)
    coerce = lambda val: val   对选中的每一个值进行一次转换
    empty_value= ''            空值的默认值
 
ComboField(Field)
    fields=()                  使用多个验证，如下：即验证最大长度20，又验证邮箱格式
                               fields.ComboField(fields=[fields.CharField(max_length=20), fields.EmailField(),])
 
MultiValueField(Field)
    PS: 抽象类，子类中可以实现聚合多个字典去匹配一个值，要配合MultiWidget使用
 
SplitDateTimeField(MultiValueField)
    input_date_formats=None,   格式列表：['%Y--%m--%d', '%m%d/%Y', '%m/%d/%y']
    input_time_formats=None    格式列表：['%H:%M:%S', '%H:%M:%S.%f', '%H:%M']
 
FilePathField(ChoiceField)     文件选项，目录下文件显示在页面中
    path,                      文件夹路径
    match=None,                正则匹配
    recursive=False,           递归下面的文件夹
    allow_files=True,          允许文件
    allow_folders=False,       允许文件夹
    required=True,
    widget=None,
    label=None,
    initial=None,
    help_text=''
 
GenericIPAddressField
    protocol='both',           both,ipv4,ipv6支持的IP格式
    unpack_ipv4=False          解析ipv4地址，如果是::ffff:192.0.2.1时候，可解析为192.0.2.1， PS：protocol必须为both才能启用
 
SlugField(CharField)           数字，字母，下划线，减号（连字符）
    ...
 
UUIDField(CharField)           uuid类型
```

## 字段校验（正则）

### RegexValidator验证器

```
from django.forms import Form
from django.forms import widgets
from django.forms import fields
from django.core.validators import RegexValidator
 
class MyForm(Form):
    user = forms.CharField(
        validators=[RegexValidator(r'^[0-9]+$', '请输入数字'), RegexValidator(r'^159[0-9]+$', '数字必须以159开头')],
    )
```

### 自定义验证函数

```
import re
from django.forms import Form
from django.forms import widgets
from django.forms import fields
from django.core.exceptions import ValidationError
 
 
# 自定义验证规则
def mobile_validate(value):
    mobile_re = re.compile(r'^(13[0-9]|15[012356789]|17[678]|18[0-9]|14[57])[0-9]{8}$')
    if not mobile_re.match(value):
        raise ValidationError('手机号码格式错误')
 
 
class PublishForm(Form):
 
 
    title = fields.CharField(max_length=20,
                            min_length=5,
                            error_messages={'required': '标题不能为空',
                                            'min_length': '标题最少为5个字符',
                                            'max_length': '标题最多为20个字符'},
                            widget=widgets.TextInput(attrs={'class': "form-control",
                                                          'placeholder': '标题5-20个字符'}))
 
 
    # 使用自定义验证规则
    phone = fields.CharField(validators=[mobile_validate, ],
                            error_messages={'required': '手机不能为空'},
                            widget=widgets.TextInput(attrs={'class': "form-control",
                                                          'placeholder': u'手机号码'}))
 
    email = fields.EmailField(required=False,
                            error_messages={'required': u'邮箱不能为空','invalid': u'邮箱格式错误'},
                            widget=widgets.TextInput(attrs={'class': "form-control", 'placeholder': u'邮箱'}))
```







 

# 补充进阶

## 应用Bootstrap样式





```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="x-ua-compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="/static/bootstrap/css/bootstrap.min.css">
  <title>login</title>
</head>
<body>
<div class="container">
  <div class="row">
    <form action="/login2/" method="post" novalidate class="form-horizontal">
      {% csrf_token %}
      <div class="form-group">
        <label for="{{ form_obj.username.id_for_label }}"
               class="col-md-2 control-label">{{ form_obj.username.label }}</label>
        <div class="col-md-10">
          {{ form_obj.username }}
          <span class="help-block">{{ form_obj.username.errors.0 }}</span>
        </div>
      </div>
      <div class="form-group">
        <label for="{{ form_obj.pwd.id_for_label }}" class="col-md-2 control-label">{{ form_obj.pwd.label }}</label>
        <div class="col-md-10">
          {{ form_obj.pwd }}
          <span class="help-block">{{ form_obj.pwd.errors.0 }}</span>
        </div>
      </div>
      <div class="form-group">
      <label class="col-md-2 control-label">{{ form_obj.gender.label }}</label>
        <div class="col-md-10">
          <div class="radio">
            {% for radio in form_obj.gender %}
              <label for="{{ radio.id_for_label }}">
                {{ radio.tag }}{{ radio.choice_label }}
              </label>
            {% endfor %}
          </div>
        </div>
      </div>
      <div class="form-group">
        <div class="col-md-offset-2 col-md-10">
          <button type="submit" class="btn btn-default">注册</button>
        </div>
      </div>
    </form>
  </div>
</div>

<script src="/static/jquery-3.2.1.min.js"></script>
<script src="/static/bootstrap/js/bootstrap.min.js"></script>
</body>
</html>
```



## 批量添加样式

可通过重写form类的init方法来实现。





```
class LoginForm(forms.Form):
    username = forms.CharField(
        min_length=8,
        label="用户名",
        initial="张三",
        error_messages={
            "required": "不能为空",
            "invalid": "格式错误",
            "min_length": "用户名最短8位"
        }
    ...

    def __init__(self, *args, **kwargs):
        super(LoginForm, self).__init__(*args, **kwargs)
        for field in iter(self.fields):
            self.fields[field].widget.attrs.update({
                'class': 'form-control'
            })
```



# ModelForm

通常在Django项目中，我们编写的大部分都是与Django 的模型紧密映射的表单。 举个例子，你也许会有个Book 模型，并且你还想创建一个form表单用来添加和编辑书籍信息到这个模型中。 在这种情况下，在form表单中定义字段将是冗余的，因为我们已经在模型中定义了那些字段。

基于这个原因，Django 提供一个辅助类来让我们可以从Django 的模型创建Form，这就是ModelForm。

## modelForm定义

form与model的终极结合。





```
class BookForm(forms.ModelForm):

    class Meta:
        model = models.Book
        fields = "__all__"
        labels = {
            "title": "书名",
            "price": "价格"
        }
        widgets = {
            "password": forms.widgets.PasswordInput(attrs={"class": "c1"}),
        }
```





### class Meta下常用参数：





```
model = models.Book  # 对应的Model中的类
fields = "__all__"  # 字段，如果是__all__,就是表示列出所有的字段
exclude = None  # 排除的字段
labels = None  # 提示信息
help_texts = None  # 帮助提示信息
widgets = None  # 自定义插件
error_messages = None  # 自定义错误信息
```





### ModelForm的验证

与普通的Form表单验证类型类似，ModelForm表单的验证在调用is_valid() 或访问errors 属性时隐式调用。

我们可以像使用Form类一样自定义局部钩子方法和全局钩子方法来实现自定义的校验规则。

如果我们不重写具体字段并设置validators属性的化，ModelForm是按照模型中字段的validators来校验的。

### save()方法

每个ModelForm还具有一个save()方法。 这个方法根据表单绑定的数据创建并保存数据库对象。 ModelForm的子类可以接受现有的模型实例作为关键字参数instance；如果提供此功能，则save()将更新该实例。 如果没有提供，save() 将创建模型的一个新实例：



```
>>> from myapp.models import Book
>>> from myapp.forms import BookForm

# 根据POST数据创建一个新的form对象
>>> form_obj = BookForm(request.POST)

# 创建书籍对象
>>> new_ book = form_obj.save()

# 基于一个书籍对象创建form对象
>>> edit_obj = Book.objects.get(id=1)
# 使用POST提交的数据更新书籍对象
>>> form_obj = BookForm(request.POST, instance=edit_obj)
>>> form_obj.save()
```





 