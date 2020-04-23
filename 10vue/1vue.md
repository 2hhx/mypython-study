[TOC]

# Vue



[vue官网：https://vuejs.org/](https://vuejs.org/)

[github：https://github.com/vuejs/vue]( https://github.com/vuejs/vue)

学习网站：[https://www.runoob.com/vue2/vue-tutorial.html](https://www.runoob.com/vue2/vue-tutorial.html)

官方文档：[http://vuejs.org/v2/guide/syntax.html](http://vuejs.org/v2/guide/syntax.html)

中文文档: [https://cn.vuejs.org/v2/guide/syntax.html](https://cn.vuejs.org/v2/guide/syntax.html)

Webpack 是一个前端资源加载/打包工具。它将根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。

Webpack 入门教程：[http://www.runoob.com/w3cnote/webpack-tutorial.html](https://www.runoob.com/w3cnote/webpack-tutorial.html)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191111172012829-309175060.png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191111172009508-2101187094.png)

# vue的引入

#### 本地的引入

在body和html之间

```
</body>
	<script src="vuejs/vue.js"></script>
</html>
```



#### [CDN的引入](https://cn.vuejs.org/v2/guide/installation.html#CDN)

对于制作原型或学习，你可以这样使用最新版本：

```
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

对于生产环境，我们推荐链接到一个明确的版本号和构建文件，以避免新版本造成的不可预期的破坏：

```
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.0"></script>
```

#### NPM

在用 Vue 构建大型应用时推荐使用 NPM 安装[1]。NPM 能很好地和诸如 webpack 或 Browserify 模块打包器配合使用。同时 Vue 也提供配套工具来开发单文件组件。

```
npm install vue
```



#### 构建工具 (CLI)

```
npm install -g @vue/cli
vue create my-project
```

# [Vue](https://cn.vuejs.org/)基础

### *渐进式 JavaScript 框架*

```
通过对框架的了解与运用程度，来决定其在整个项目中的应用范围，最终可以独立以框架方式完成整个web前端项目
```

# 走进Vue

## what -- 什么是Vue

```
可以独立完成前后端分离式web项目的JavaScript框架
vue框架：渐进式JavaScript框架
vue一个环境：可以只控制页面中一个标签、可以控制一组标签、可以控制整个页面、可以控制整个项目
vue可以根据实际需求，选择控制前端项目的区域范围
```

### 定义

- Vue 是一套用于构建用户界面的渐进式框架
- 使用Vue框架，可以完全在浏览器端渲染页面，服务端只提供数据
- 使用Vue框架可以非常方便的构建 单页面应用 (SPA)

## 为什么学习vue

```python
"""
1、html、css、js直接开发项目，项目杂乱，不方便管理，要才有前端框架进行开发，规范项目
2、Angular、React、Vue三个前端框架，吸取前两个框架的所有优点，摒弃缺点，一手文档是中文
3、Vue框架优点：
	轻量级
	数据驱动
	数据的双向绑定
	虚拟DOM（嵌套页面架构的缓存）
	组件化开发
	由全球社区维护
	
	单页面应用、MVVM设计模式
"""
总结：
三大主流框架之一：Angular React Vue
先进的前端设计模式：MVM
可以完全脱离服务器端，以前端代码复用的方式渲染整个页面：组件化开发
```

## special--Vue的优点

- 不存在依赖
- 轻量级（25k min）
- 数据驱动
- 数据的双向绑定
- 虚拟DOM（嵌套页面架构的缓存）
- 组件化开发
- 由全球社区维护
- 适用范围广(大中小型项目、PC、移动端、混合开发)
- 本土框架,社区非常活跃
- 语法简单、学习成本低
- 双向数据绑定（所见即所得）
- 单页面web应用

## 前端框架

###  三足鼎立

- React
- Angular
- Vue

### Angular、Vue、React的区别

#### Vue与React

- React与Vue 都采用虚拟DOM
- 核心功能都在核心库中，其他类似路由这样的功能则由其他库进行处理
- React的生态系统更庞大，由ReactNative来进行混合App开发; Vue更轻量
- React由独特的JSX语法; Vue是基于传统的Web计数进行扩展(HTML、CSS、JavaScript)，更容易学习

#### Vue与Angular

- Angular1和Angular2以后的版本 是完全不同的两个框架； 一般Angular1被称作Angular.js, Angular之后的版本被称作 Angular
- Vue与Angular的语法非常相似
- Vue没有像Angular一样深入开发，只保证了基本功能。 Angular过于笨重
- Vue的运行速度比Angular快得多
- Angular的脏检查机制带来诸多性能问题

### MVM

- Model
- View
- ViewModel





# 使用框架开展一个项目的时候，需要考虑哪些方面？

1.性能

如果一个网站在性能方面存在问题，它将会损失超过一半以上的用户。

对于框架性能，你可以在网上查询到各类测试，你可以了解框架的代码结构、逻辑处理，判断是否能够满足你对性能的需求。

2.扩展性

对于一个需要长期维护的项目而言，经常会有各种各样的功能添加进来，这时扩展性就显得尤为重要，如果你在前期选择了一款满足前期的框架，但后期你需要使用某个插件来完成某个功能，或者基于什么完成一个功能，这时候你发现网上并没有检索到相关内容，内心是否充满了心塞。

3.维护性

一个项目的生命周期不是三天两天，而前端的发展则是爆炸式的。在你选择框架的时候是否考虑过官方在后续的一段时间是否会一直对框架进行更新维护？如果不确定，是否已经有了官方放弃维护后的解决方案？

4.兼容性

这里的兼容性指的不是浏览器兼容，而是框架与其他框架及工具的兼容，使用这个框架对于你的开发环境是否有影响，对于你的开发 IDE 是否有影响。

Vue.js 适用具有以下性质的项目：

- 对浏览器兼容要求不高，不需要兼容至IE6-8；
- SPA开发；
- 对性能较高要求;
- 组件化。

总的来说，如果你是一个 MVVM 框架新手，那么 Vue.js 就是你最好的进阶工具，如果你是一个已经掌握了其他 MVVM 框架的老手，那你会发现 Vue.js 更加简单轻便。



# 多页面应用和单页面应用

##  多页面应用（MultiPage Application，MPA）

多页面跳转刷新所有资源，每个公共资源(js、css等)需选择性重新加载，常用于 app 或 客户端等

 

## 单页面应用（SinglePage Web Application，SPA）

只有一张Web页面的应用，是一种从Web服务器加载的富客户端，单页面跳转仅刷新局部资源 ，公共资源(js、css等)仅需加载一次，常用于PC端官网、购物等网站

![img](data:image/jpeg;base64,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)

## 两者对比

|                   | 单页面应用                                                   | 多页面应用                                   |
| ----------------- | ------------------------------------------------------------ | -------------------------------------------- |
| 组成              | 一个外壳页面和多个页面片段组成                               | 多个完整页面构成                             |
| 资源公用(css,js)  | 共用，只需在外壳部分加载                                     | 不共用，每个页面都需要加载                   |
| 刷新方式          | 页面局部刷新或更改                                           | 整页刷新                                     |
| url 模式          | a.com/#/pagetwo a.com/#/pagetwo                              | a.com/pageone.html a.com/pagetwo.html        |
| 用户体验          | 页面片段间的切换快，用户体验良好                             | 页面切换加载缓慢，流畅度不够，用户体验比较差 |
| 转场动画          | 容易实现                                                     | 无法实现                                     |
| 数据传递          | 容易                                                         | 依赖 url传参、或者cookie 、localStorage等    |
| 搜索引擎优化(SEO) | 需要单独方案、实现较为困难、适用于追求高度支持搜索引擎的应用 | 实现方法简易                                 |
| 试用范围          | 高要求的体验度、追求界面流畅的应用                           | 适用于追求高度支持搜索引擎的应用             |
| 开发成本          | 较高，常需借助专业的框架                                     | 较低 ，但页面重复代码多                      |
| 维护成本          | 相对容易                                                     | 相对复杂                                     |





#  Vue基本演示

# vue挂载点

```javascript
<div class="" id="app">{{ }}</div>
```

```javascript
<script>
    // console.log(Vue);
    new Vue({
        el: "#app",
    })


</script>
```

1. el为挂载点，建立关联后控制页面标签，就是vue与页面的关联
2. 采有css3选择器语法与页面标签进行绑定，决定该vue对象控制的页面范围
3. 挂载点只检索页面中第一个匹配到的结果，所以挂载点一般都才有id选择器
4. html与body标签不可以作为挂载点（组件中解释）
5. 被渲染的内容一般都放在父标签下面，并且父标签为挂载点
6. 当div作为挂载点的时候，里面的所有的标签都可以被渲染
7. 可以创建多个

# vue变量

1. 页面中可以出现多个实例，并且可以进行交互
2. 实例成员中的data是为vue页面模板通过数据变量的

```javascript
<div id="app">
    <p>{{ msg }}</p>
    <p>{{ info }}</p>

</div>
```



```javascript
<script src="vuejs/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            msg: "message",
            info: "vue变量信息"
        }

    });
</script>
```

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113113502849-1522921540.png)

# vue实例变量（创建实例）

1. 创建vue实例（new Vue）传进去的字典(对象)的key，称之为vue实例成员(变量)
2. 给vue命名 let
3. 访问vue成员，用.点

```javascript
<div id="app">
    <p>{{ msg }}</p>
    <p>{{ info }}</p>

</div>
```



```javascript
let app=new Vue({
        el: '#app',
        data: {
            msg: "message",
            info: "vue变量信息"
        }

    });

console.log(app.info);
console.log(main.info)
```

访问实例成员，用 vue实例`.$成员名, eg：app.$el`

```javascript
console.log(app.$el);
console.log(app.$data);
console.log(app.$data.info);
console.log(app.info);
```



# vue事件



```javascript
<div id="app">
    <p class="p1" v-on:click="fn">这是一个段落，被点击了{{ count }}下</p>

</div>

</body>
<script src="vuejs/vue.js"></script>
<script>
 let app = new Vue({
        el: '#app',
        data: {
            count: 0,
            fn: function () {
                console.log(app.count);
                app.count ++ ;
                console.log(this)//this不是该vue实例对象，是顶级Window对象，并不是指当前对象


            }

        }
    })
</script>
```

## methods事件

就是为vue实例提供事件函数的

```javascript
<script>
    let app = new Vue({
        el: '#app',
        data: {
            count: 0,
            },
            methods:{
                fn:function () {
                    this.count ++// this代表当前该vue实例对象

                }
            }
    },

    );
</script>
```



```javascript
<div id="app">
        <p class="p2" v-on:mouseover="overAction" v-on:mouseout="outAction" >该便签被{{ action }}</p>

        <div v-on:mouseover="overAction" v-on:mouseout="outAction">div被{{ action }}</div>
    </div>
```



```javascript
let app = new Vue({
        el: '#app',
        action: '渲染',
        data: {
            count: 0,
        },
        methods: {
            fn: function () {
                this.count++// this代表当前该vue实例对象
                

            },
            overAction: function () {
                this.action = '悬浮'
            },
			outAction: function () {
                this.action = '离开'
            }
        }
    });

```

# js对象

1. js中没有字典，只要对象类型，可以把对象当做字典来使用

2. key本质是属性名，所以都是字符串类型（可以出现1，true），其实都是省略引号的字符串

   

```javascript
<script>
    let sex='男';
    let hobby='篮球';
    let dic={
        'name':'Ocean',
        1:100,
        true:12345,
        age:18,
        sex:sex,
        hobby,//相同的时候是可以省略一个的

    };
    console.log(dic['name']);
    console.log(dic['1']);
    console.log(dic[1]);
    console.log(dic['age']);
    console.log(dic['hobby']);
    console.log(dic['sex']);
    console.log(dic.sex);   
</script>
```

```javascript
 dic.price=3.5;
console.log(dic.price);
console.log(dic['proce']);//不能这样调用
```

## 定义类

```javascript
声明类创建对象，类可以实例化n个对象，哪个对象调用，this就代表谁
<script>
    function People(name,age) {
        this.name=name;
        this.age=age;
        this.eat=function () {
            console.log(this.name+'正在开炮');
            return 123

        }

    }
let p1 = new People('ocean',18);
console.log(p1.name);
let res=p1.eat();
console.log(res);
</script>
```

直接声明对象，{}内的key都属于当前对象的

{}中的方法通常会简写

```javascript
let stu1={
        name:'张三',
        age:18,
     // eat: function () {
        //     console.log(this.name + '在吃饭');
        eat(){
            console.log(this.name+"打炮中")
        }
    };
    stu1.eat()
```

**总结**
1.{}中直接写一个变量：key与value是同名，value有该名变量提供值
2.es5下，所有的函数都可以作为类，类可以new声明对象，在函数中用 this.资源 为声明的对象提供资源
3.{}中出现的函数叫方法，方法可以简写 { fn: function(){} } => { fn(){} }



# 文本指令

## 插值表达式

1、插值表达式，能完成变量渲染，变量基础运算，变量方法调用，不能完成复杂运算(一步解决不了的，不能出现；)

```javascript
<div id="app">
    <p>{{ msg }}</p>
    <p>{{ msg + '拼接内容' }}</p>//字符串拼接
    <p>{{ msg[1] }}</p>//字符串索引取值
    <p>{{ msg.slice(2,4) }}</p>//字符串切片
    <hr>
</div>

</body>
<script src="vuejs/vue.js"></script>
<script>
    new Vue({
        el:"#app",
        data:{
            msg:'文本信息'
        }
    })
</script>
```

2、v-text：将所有内容做文本渲染

```javascript
<p v-text="msg + '拼接内容'"></p>
```

3、v-html：可以解析html语法标签的文本，字符串拼接的时候出现字符串的时候方便使用

```javascript
<p v-text="'<b>' + msg + '</b>'"></p>
<p v-html="'<b>' + msg + '</b>'"></p>


<p v-text="`<b>${msg}</b>`"></p>
<p v-html="`<b>${msg}</b>`"></p>
```

# 过滤器

在传值的时候是各不干扰，个传个的

```javascript
<div id="app">
    <!-- 默认将msg作为参数传给filterFn -->
    <p>{{ msg | filterFn }}</p>

</div>

</body>
<script src="vuejs/vue.js"></script>
<script>
    new Vue({
        el:"#app",
        data:{
            msg:"文本内容",
            num:1
        },
        filters:{
            filterFn(v1){
                console.log(v1);
                return `<b>${v1}</b>`;
            },
        }
    })
```

## 过滤器串联（多层过滤器）

```javascript
<p>{{ num | f1 | f2 }}</p>//可以多层过滤   
```

```javascript
new Vue({
        el: '#app',
        data: {
            msg: '文本内容',
            num: 1
        },
        filters: {
            f1(v1) {
                return v1 * 100;
            },
            f2(v1) {
                return v1 * 100;
            }
        }
    })
```

1. 可以同时对多个变量进行过滤，变量用，分割，过滤器还可以额外传入参数辅助过滤
2. 过滤器方法接收所有传入的参数，按传入的位置进行接收

```javascript
 <p>{{ msg, num | f3(666, '好的') }}</p>

filters: {
            f3(v1, v2, v3, v4) {
                console.log(v1);
                console.log(v2);
                console.log(v3);
                console.log(v4);
            }
        }
```

# 事件指令

```javascript
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>事件指令</title>
    <style>
        .box {
            width: 200px;
            height: 200px;
            background-color: yellowgreen;
        }
    </style>
</head>
<body>
    <div id="app">
        <!--事件指令：v-on:事件名="事件函数"  -->
        <!--简写：@事件名="事件函数"  -->
        <p v-on:click="f1">被点击了{{ count }}下</p>
        <hr>
        <p @click="f2">{{ p2 }}</p>
        <hr>
        <!--绑定的事件函数可以添加()，添加括号就代表要传递参数-->
        <ul>
            <li @click="f3(100)">{{ arr[0] }}</li>
            <li @click="f3(200)">{{ arr[1] }}</li>
            <li @click="f3(300)">{{ arr[2] }}</li>
        </ul>
        <ul>
            <li @click="f4(0)">{{ arr[0] }}</li>
            <li @click="f4(1)">{{ arr[1] }}</li>
            <li @click="f4(2)">{{ arr[2] }}</li>
        </ul>
        <hr>
        <!--绑定的事件函数如果没有传递参数，默认传入 事件对象 -->
        <div class="box" @click="f5"></div>
        <hr>
        <!--事件函数一旦添加传参()，系统就不再传递任何参数，需要事件对象时，可以手动传入 $event -->
        <div class="box" @click="f6(10, $event)"></div>
    </div>
</body>
<script src="js/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            count: 0,
            p2: '第二个p',
            arr: [1, 2, 3],
        },
        methods: {
            f1() {
                this.count ++
            },
            f2() {
                console.log(this.p2)
            },
            f3(num) {
                console.log(num);
            },
            f4(index) {
                console.log(this.arr[index]);
            },
            f5(ev, b) {
                console.log(ev);
                console.log(b);
            },
            f6(num, ev) {
                console.log(num);
                console.log(ev);
            }
        },
    })

</script>
</html>
```

# 属性指令

```javascript
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>属性指令</title>
    <style>
        .b1 {
            width: 100px;
            height: 100px;
            background-color: red;
        }

        .box1 {
            width: 150px;
            height: 150px;
            background-color: darkturquoise;
            transition: .3s;
        }
        .box2 {
            width: 300px;
            height: 100px;
            background-color: darkgoldenrod;
            transition: .3s;
        }

        .circle {
            border-radius: 50%;
        }
    </style>
</head>
<body>
    <div id="app">
        <!--1.下方的class、id、title、abc等是div标签的属性，属性指令就是控制它们的-->
        <div class="b1" id="b1" v-bind:title="title" :abc="xyz"></div>
        <!--2.属性指令：v-bind:属性名="变量"，简写方式 :属性名="变量" -->

        <!--3.属性指令操作 style 属性-->
        <div style="width: 200px;height: 200px;background-color: greenyellow"></div>
        <!-- 通常：变量值为字典 -->
        <div :style="mys1"></div>
        <!-- 了解：{中可以用多个变量控制多个属性细节} -->
        <div :style="{width: w,height: '200px',backgroundColor: 'deeppink'}"></div>


        <!--重点：一般vue都是结合原生css来完成样式控制 -->
        <!--<div :class="c1"></div>-->

        <!--class可以写两份，一份写死，一份有vue控制-->
        <div class="box1" :class="c2"></div>

        <!--{}控制类名，key为类名，key对应的值为bool类型，决定该类名是否起作用-->
        <div :class="{box2:true, circle:cable}" @mouseover="changeCable(1)" @mouseout="changeCable(0)"></div>


        <!--[]控制多个类名-->
        <div :class="[c3, c4]"></div>
    </div>

    <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
</body>
<script src="js/vue.js"></script>
<script>
    let app = new Vue({
        el: '#app',
        data: {
            title: '12345',
            xyz: 'opq',
            mys1: {
                width: '200px',
                height: '200px',
                // 'background-color': 'greenyellow'
                backgroundColor: 'pink',
            },
            w: '200px',
            c1: 'box1',
            c2: 'circle',
            cable: false,
            c3: 'box1',
            c4: 'circle'
        },
        methods: {
            changeCable(n) {
                this.cable = n;
            }
        }
    });

    setInterval(function () {
        // app.c1 = app.c1 === 'box1' ? 'box2' : 'box1';
        if (app.c1 === 'box1') {
            app.c1 = 'box2';
        } else {
            app.c1 = 'box1';
        }
    }, 300)


</script>
</html>
```

# 实战演练

1. 有 红、黄、蓝 三个按钮，以及一个200x200矩形框box，点击不同的按钮，box的颜色会被切换为指定的颜色

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="box">
    <div :style="box"></div>
    <hr>
    <button @click="b1">按钮1</button>
    <button @click="b2">按钮2</button>
    <button @click="b3">按钮3</button>
</div>
</body>
<script src="vuejs/vue.js"></script>
<script>
    let app = new Vue({
        el: "#box",
        data: {
            box: {
                width: "200px",
                height: "200px",
                backgroundColor: "red"
            }
        },
        methods: {
            b1(v1, v2) {
                app.box = {
                    width: "200px",
                    height: "200px",
                    backgroundColor: "black"
                }
            },

            b2(v1, v2) {
                app.box = {
                    width: "200px",
                    height: "200px",
                    backgroundColor: "yellow"
                }
            },
            b3(v1, v2) {
                app.box = {
                    width: "200px",
                    height: "200px",
                    backgroundColor: "blue"
                }

            }

        }
    })

</script>
</html>
```

2. 有一个200x200矩形框wrap，点击wrap本身，记录点击次数，如果是1次wrap为pink色，2次wrap为green色，3次wrap为cyan色，4次重新回到pink色，依次类推

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <div id="app" :style="dic1" @click="fn">{{ count }}</div>
   </body>
   <script src="vuejs/vue.js"></script>
   <script>
       let app = new Vue({
           el: "#app",
           data: {
               count: 0,
               dic1: {
                   width: "200px",
                   height: "200px",
                   backgroundColor: "red"
               },
               arr: ['red', 'green', 'cyan']
   
           },
           methods: {
               fn() {
                   app.count++;
                   let n = this.count % 3;
                   this.dic1.backgroundColor = this.arr[n]
   
               }
           }
   
       })
   </script>
   </html>
   ```

   ![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191112085046321-114720866.png)

3. 如图：图形初始左红右绿，点击一下后变成上红下绿，再点击变成右红左绿，再点击变成下红上绿，依次类推

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
       <style>
           .box {
               width: 200px;
               height: 200px;
               border: 1px #0f0f0f solid;
               border-radius: 50%;
               overflow: hidden;
               position: relative;
           }
   
           .b1 {
               background-color: red;
               position: absolute;
           }
   
           .b2 {
               background-color: green;
               position: absolute;
           }
   
           .left {
               width: 100px;
               height: 200px;
               float: left;
               left: 0;
   
           }
   
           .right {
               width: 100px;
               height: 200px;
               float: left;
               right: 0;
           }
   
           .top {
               width: 200px;
               height: 100px;
               top: 0;
   
           }
   
           .bottom {
               width: 200px;
               height: 100px;
               bottom: 0;
   
           }
   
       </style>
   </head>
   <body>
   <div id="app">
       <div class="box" @click="clickAction">
           <div class="b1 " :class="c1"></div>
           <div class="b2" :class="c2"></div>
       </div>
   </div>
   
   </body>
   <script src="vuejs/vue.min.js"></script>
   <script>
       let app=new Vue({
           el:'#app',
           data:{
               count:1,
               c1:'left',
               c2:'right',
               c1Arr:['left','top','right','bottom'],
               c2Arr: ['right','bottom','left','top']
           },
           methods:{
               clickAction(){
                   let n = this.count ++;
                   this.c1 = this.c1Arr[n % 4];
                   this.c2 = this.c2Arr[n % 4]
               }
           }
       })
   </script>
   </html>
   ```

4. 可以将图编程自动旋转

   基于第三题的编辑

   利用时钟定时器进行编辑

   方法1

   ```
    setInterval(function () {
           let n = app.count++;
           app.c1 = app.c1Arr[n % 4];
           app.c2 = app.c2Arr[n % 4]
   
       }, 500)
   ```

   方法2

   ```html
   setInterval(function () {
           app.clickAction();
   
       },500)
   ```

   

# 知识点总结

```javascript
1、在html页面中用script标签导入vue环境
	<script src="js/vue.js"></script>
```

```javascript
2、new Vue({ el: "#app" })挂载页面标签，建立关联后控制页面标签
	挂载点才有css3选择器语法
	挂载点就是vue与页面的关联
	挂载点只检索第一个匹配结果
```

```javascript
3、插值表达式{{ }}可以完成基础运算
	num | num + 10 | str.split() + "拼接" 
```

```javascript
4、插值表达式中的变量有实例成员 data 来提供
	<p id="app">{{ msg }}</P>
	let msg = '12345'
	new Vue({
		el: "#app",
		data: {
			msg,
		}
	})
```

```javascript
5、v-on指令可以给标签绑定事件，事件函数由实例成员 methods 来提供
```

```javascript
6、插值表达式{{ 变量 | 过滤器 }}的过滤器由实例成员 filters 来提供
	<p id="app" @click="fn">{{ msg | f1(1), 10 | f2(100, 200) }}</P>
	let msg = '12345'
	new Vue({
		el: "#app",
		data: {
			msg,
		},
		methods: {
			fn(){}
		},
		filters: {
			f1(v1,v2){return v1+v2},
			f2(v1,v2,v3,v4){return v1+v2+v3+v4}
		}
	})
```

```javascript
7、面向对象js： { 变量, } | function Fn(值){ this.属性 = 值 } | obj = { 方法(){} }
	function Fn(v1, v2){
		this.n1 = v1;
		this.n2 = v2;
	}
	let f1 = new Fn(10, 20);
	f1.n1
```

```javascript
8、文本指令：{{ }} | v-text=""  | v-html=""
```

```javascript
9、事件指令: v-on:事件名="" | @事件名="" | :事件名="fn" | :事件名="fn($event, 自定义参数)"
	@click="fn"  |  @click="fn()"  |  @click="fn(10， 20)"  |  @click="fn($event, 10)"
```

```javascript
10、属性指令：v-bind:属性名="" | :title="变量" | :style="字段变量" | 
			:class="变量" | :class="[变量1, 变量2]" | :class="{类1:真假, 类2:真假}"
	:title="var1" | :style="dic1" | :class="var2" | :class="[var3, var4]" | 
	:class="{box: true|false}"
	
	var2 = 'box' | var2 = 'box circle'
```





参考

- 百科介绍： [https://baike.baidu.com/item/%E5%B0%A4%E9%9B%A8%E6%BA%AA/2281470?fr=aladdin](https://baike.baidu.com/item/尤雨溪/2281470?fr=aladdin)
- 微博： https://weibo.com/arttechdesign?is_hot=1#1528176039582