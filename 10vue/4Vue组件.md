[TOC]



创建、注册、使用子组件的三部曲

# 组件

1、组件：由html、css、js三部分组成的独立单位，可以类似于变量，重复使用
2、组件其实就是vue实例(对象)，一个组件就是一个vue实例(对象)
3、new Vue()产生的也是实例(对象)，所以也是组件，我们称之为 根组件
    一个页面建议只出现一个根组件（项目开发模式下，一个项目建议只出现一个根组件）
4、组件的html页面结构有 template 实例成员提供
    template提供的html结构是用来构虚拟DOM
      真实DOM最终会被虚拟DOM替换
      根组件一般不提供template，就由挂载点el来提供构建虚拟DOM的页面结构，根组件如果提供了template，还需要设置挂载点作为替换占位
     template模板有且只有一个根标签

- 每一个组件都是一个vue实例
- 每个组件均具有自身的模板template，根组件的模板就是挂载点
- 每个组件模板只能拥有一个根标签
- 子组件的数据具有作用域，以达到组件的复用

# 根组件（父组件）

```
<div id="app">
    <h1>{{ msg }}</h1>
</div>
<script type="text/javascript">
    // 通过new Vue创建的实例就是根组件(实例与组件一一对应,一个实例就是一个组件)
    // 每个组件组件均拥有模板,template
    var app = new Vue({
        // 根组件的模板就是挂载点
        el: "#app",
        data : {
            msg: "根组件"
        },
        // 模板: 由""包裹的html代码块,出现在组件的内部,赋值给组件的$template变量
        // 显式书写模块,就会替换挂载点,但根组件必须拥有挂载点
        template: "<div>显式模板</div>"
    })
    // app.$template
</script>
```

```
<body>
    <div id="app">
        {{ msg }}
    </div>
</body>
<script src="js/vue.js"></script>
<script>

    let c1 = '';
    new Vue({
        el: '#app',
        data: {
            msg: '12345',
            c1: 'red'
        },
        template: `
        <div id="app">
            <p :style="{color: c1}">{{ msg }}</p>
            <p @click="clickAction">{{ msg }}</p>
        </div>
        `,
        methods: {
            clickAction() {
                console.log(this.msg)
            }
        }
    })
</script>
</html>
```



# 局部组件（子组件）

在根组件template中加载的组件，称之为根组件的子组件
1、定义组件
2、注册组件
3、使用组件

如何定义子组件：组件就是一个普通对象，内部采用vue语法结构，被vue注册解释后，就会成为vue组件

重点使用**template** **components**

```html
<div id="app">
    <!--使用组件-->
    <titletag></titletag>
    <titletag/>
    <tag></tag>
    <tag/>
</div>

</body>
<script src="vuejs/vue.js"></script>
<script>
    // 创建子组件
    let titletag = {
        template: `//模板语法
        <p>
            <b>
                这是一个二哈
            </b>
        </p>
        `
    };
    new Vue({
        el:"#app",
        //注册子组件
        components:{
            // titletag,
            tag:titletag
        }
    })
</script>
```



# 全局组件

了解：全局组件，不要注册就可以直接使用

```javascript
<div id="app">
    <global-tag></global-tag>
    <global-tag></global-tag>
</div>
<script>
    Vue.component('global-tag', {
        data () {
            return {
                count: 0
            }
        },
        template: '<button @click="btnAction">全局{{ count }}</button>',
        methods: {
            btnAction () {
                this.count ++
            }
        }
    })
    new Vue({
        el: "#app"
    })
</script>
```

# 父组件传递数据给子组件

- 通过绑定属性的方式进行数据传递
- ​     1.数据在父组件中产生
  ​     2.在父组件中渲染子组件，子组件绑定自定义属性，附上父组件中的数据
  ​     3.子组件自定义属性在子组件的props成员中进行声明(采用字符串发射机制)
  ​     4.在子组件内部，就可以用props声明的属性(直接作为变量)来使用父组件中的数据
- **props**
- 方法1`props:['bb']`
- 方法2` props:{aa:'bb' }`

```js
    <style>
        .wrap {
            width: calc(200px * 4 + 80px);
            margin: 0 auto;
            user-select: none;
        }
        .box {
            width: 200px;
            height: 260px;
            /*border: 1px solid black;*/
            background-color: rgba(10, 200, 30, 0.5);
            border-radius: 10px;
            float: left;
            margin: 10px;
        }
        .box img {
            /*width: 100%;*/
            height: 160px;
            border-radius: 50%;
            margin: 0 auto;
            display: block;
        }
        .box p {
            text-align: center;
        }
    </style>
</head>
<body>
    <div id="app">
        <div class="wrap">
            <!--<div v-for="dog in dogs" class="box">-->
                <!--<img :src="dog.img" alt="">-->
                <!--<p>-->
                    <!--<b>-->
                        <!--{{ dog.title }}-->
                    <!--</b>-->
                <!--</p>-->
            <!--</div>-->
            <tag v-for="dog in dogs" v-bind:dog="dog" :a="1" :b="2" />
        </div>
    </div>
</body>
<script src="js/vue.js"></script>
<script>
    let dogs = [
        { title: '二哈1号', img: 'img/1.jpg', },
        { title: '二哈2号', img: 'img/2.jpg', },
        { title: '二哈3号', img: 'img/3.jpg', },
        { title: '二哈4号', img: 'img/4.jpg', },
        { title: '二哈1号', img: 'img/1.jpg', },
        { title: '二哈2号', img: 'img/2.jpg', },
        { title: '二哈3号', img: 'img/3.jpg', },
        { title: '二哈4号', img: 'img/4.jpg', },
    ];

    let tag = {
        // 在组件内部就可以通过设置的自定义属性，拿到外部选择子组件提供给属性的值
        props: ['dog', 'a', 'b', 'z'],
        template: `
        <div class="box">
            <img :src="dog.img" alt="">
            <p>
                <b>
                    {{ dog.title }}
                </b>
            </p>
            <p @click="fn">
                锤它：<b>{{ num }}下</b>
            </p>
        </div>
        `,
        data () {
            return {
                num: 0,

            }
        },
        methods: {
            fn() {
                this.num ++
            }
        },
    };

	new Vue({
		el: '#app',
        data: {
		    dogs,
        },
        components: {
		    tag,
        }
	});
  
</script>
```

# 子组件传递数据给父组件

- 通过发送事件请求的方式进行数据传递

```
<style>
        ul {
            list-style: none;
        }
        .d-btn {
            font-size: 12px;
            width: 15px;
            display: inline-block;
        }
        .d-btn:hover {
            color: red;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div id="app">
        <input type="text" v-model="msg">
        <button @click="send_comment">留言</button>
        <ul>
            <tag v-for="(v, i) in comments" :msg="v" :index="i" @f1="deleteMsg"/>
        </ul>
    </div>
</body>
<script src="js/vue.js"></script>
<script>
    let tag = {
        props: ['msg', 'index'],
        template: `
        <li>
            <i class="d-btn" @click="fn">x</i>
            <b>{{ msg }}</b>
        </li>
        `,
        methods: {
            fn () {
                // 点击子集，要告诉父级删除第几条数据，因为comments在父级中
                // 需要通知父级
                this.$emit('f1', this.index);
            }
        }
    };


    new Vue({
        el: '#app',
        data: {
            msg: '',
            comments: localStorage.comments ? JSON.parse(localStorage.comments) : [],
        },
        components: {
            tag,
        },
        methods: {
            send_comment() {
                if (this.msg) {
                    this.comments.push(this.msg);
                    this.msg = '';
                    localStorage.comments = JSON.stringify(this.comments);
                }
            },
            deleteMsg(index) {
                this.comments.splice(index, 1);
                localStorage.comments = JSON.stringify(this.comments);
            }
        }
    })
</script>

```

### 精华

```
    let tag = {
        props: ['msg', 'index'],
        template: `
        <li>
            <i class="d-btn" @click="fn">x</i>
            <b>{{ msg }}</b>
        </li>
        `,
        methods: {
            fn () {
                // 点击子集，要告诉父级删除第几条数据，因为comments在父级中
                // 需要通知父级
                this.$emit('f1', this.index);
            }
        }
    };

```

```
 data: {
            msg: '',
            comments: localStorage.comments ? JSON.parse(localStorage.comments) : [],
        },
        components: {
            tag,
        },
        methods: {
            send_comment() {
                if (this.msg) {
                    this.comments.push(this.msg);
                    this.msg = '';
                    localStorage.comments = JSON.stringify(this.comments);
                }
            },
            deleteMsg(index) {
                this.comments.splice(index, 1);
                localStorage.comments = JSON.stringify(this.comments);
            }
        }
    })
```



## 补充

```
<script>
    // localStorage，sessionStorage不能直接存储数组和对象，需要序列化为json
    localStorage.arr = JSON.stringify([1, 2, 3]);
    let res = JSON.parse(localStorage.arr);
    console.log(res, res[2]);
</script>
```



# 父子组件实现todoList

```
<div id="app">
    <div>
        <input type="text" v-model="val">
        <button type="button" @click="submitMsg">提交</button>
    </div>
    <ul>
        <!-- <li v-for="(v, i) in list" :key="i" @click="removeMsg(i)">{{ v }}</li> -->
        <todo-list v-for="(v, i) in list" :key="i" :v="v" :i="i" @delect_action="delect_action"></todo-list>
    </ul>
</div>
<script type="text/javascript">
    Vue.component("todo-list", {
        template: "<li @click='delect_action'><span>第{{ i + 1 }}条: </span><span>{{ v }}</span></li>",
        props: ['v', 'i'],
        methods: {
            delect_action () {
                this.$emit("delect_action", this.i)
            }
        }
    })
    
    new Vue({
        el: "#app",
        data: {
            val: "",
            list: []
        },
        methods: {
            submitMsg () {
                // 往list中添加input框中的value
                if (this.val) {
                    this.list.push(this.val);
                    this.val = ""
                }
            },
            delect_action(index) {
                this.list.splice(index, 1)
            }
        }
    })
</script>
```

# 扩展

## 扩展1

子组件中可以继续注册子组件

本题展示多个样本二哈图片

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title></title>
    <style>
        .wrap {
            width: calc(200px * 4 + 80px);
            margin: 0 auto;
            user-select: none;
        }
        .box {
            width: 200px;
            height: 260px;
            /*border: 1px solid black;*/
            background-color: rgba(10, 200, 30, 0.5);
            border-radius: 10px;
            float: left;
            margin: 10px;
        }
        .box img {
            width: 100%;
            /*height: 200px;*/
            border-radius: 50%;
        }
        .box p {
            text-align: center;
        }
    </style>
</head>
<body>
    <div id="app">
        <div class="wrap">
            <tag></tag>
            <tag></tag>
            <tag></tag>
            <tag></tag>
        </div>
    </div>
</body>
<script src="js/vue.js"></script>
<script>
    let titleTag = {
        template: `
        <p>
            <b>
                这是一种纯二哈
            </b>
        </p>
        `,
    };

    let tag = {
        template: `
        <div class="box">
            <img src="img/001.jpg" alt="">
            <title-tag />
            <p @click="fn">
                锤它：<b>{{ num }}下</b>
            </p>
        </div>
        `,
        // 能被复用的组件(除了根组件)，数据都要做局部化处理，因为复用组件后，组件的数据是相互独立的
        // data的值为绑定的方法的返回值，返回值是存放数据的字典
        data () {
            return {
                num: 0
            }
        },
        methods: {
            fn() {
                this.num ++
            }
        },
        components: {
            titleTag,
        }
    };

	new Vue({
		el: '#app',
        components: {
		    tag,
        }
	});

    `
    class P:
        num = 0
        def __init__(n):
            this.n = n
    p1 = P()
    p2 = P()
    P.num = 10
    p1.num
    p2.num
    `

</script>
</html>
```

## 扩展2

子传父扩展，子组件输入修改父组件的标题

```
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
<div id="app">
    <h1> {{ title }} </h1>
    <!-- 组件标签不能添加系统事件，只能添加自定义事件，自定义事件在组件内部通过$emit主动触发 -->
    <tag @self_action="changeTitle"/>
</div>
</body>
<script src="js/vue.js"></script>
<script>
    let tag = {
        template: `
        <div>
            <input v-model="sub_title" />
        </div>
        `,
        data() {
            return {
                sub_title: ''
            }
        },
        watch: {
            sub_title() {
                // 将sub_title与父级的title建立关联
                // 激活(触发)self_action自定义事件
                this.$emit('self_action', this.sub_title)
            }
        }
    };

    new Vue({
        el: '#app',
        components: {
            tag,
        },
        data: {
            title: '父级初始标题'
        },
        methods: {
            changeTitle(sub_title) {
                this.title = sub_title ? sub_title : '父级初始标题';
            }
        }
    })
</script>
</html>
```

