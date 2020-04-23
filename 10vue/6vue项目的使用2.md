

[TOC]

# 项目的过程

1. vue项目的请求生命周期：main.js完成环境的加载与根组件的渲染；router的index.js完成路由映射
2. 项目启动：加载main.js：index.html、new Vue()、Vue、router、store、完成App.vue的挂载
3. 请求：请求路径 => router路由 => 页面组件(小组件) => 替换`<router-view />完成页面渲染 => <router-link>（this.$router.push()）`完成请求路径的切换



# vue内的文件

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191114194851142-446272105.png)

## public下面的index.html

```
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="utf-8">

    <meta http-equiv="X-UA-Compatible" content="IE=edge">
<!--  适配移动端   疏放程度是1倍-->
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
<!--  图标-->
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
<!--  标题，这个可以自己进行修改-->
<!--    <title>b-proj</title>-->
    <title>vue项目</title>

</head>
<body>
<!--当浏览器不支持js 的时候，会打印下面的这段话-->
<!--现在都支持,所以可以不需要-->
<noscript>
    <strong>We're sorry but b-proj doesn't work properly without JavaScript enabled. Please enable it to
        continue.</strong>
</noscript>
<!--下面的挂采点-->
<div id="app"></div>
<!-- built files will be auto injected -->
</body>
</html>

```

修改后



```
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title>vue项目</title>
</head>
<body>
<div id="app"></div>
</body>
</html>

```



## app.vue

style中写入的全局配置，这个可以进行删除

```
<style>
  /*全局配置*/
#app {
  /*浏览器配置*/
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

```

app.vue修改后保留

```
<script src="main.js"></script>
<template>
    <div id="app">
        <!--提供一个视图组件占位符，占位符被哪个views下的视图组件替换，浏览器就显示哪个页面组件-->
        <router-view/>
    </div>
</template>
```

## main.js

 1.入口文件：加载vue、router、store等配置 以及 加载自定义配置(自己的js、css，第三方的js、css)
2.创建项目唯一根组件，渲染App.vue，挂载到index.html中的挂载点 => 项目页面显示的就是 App.vue 组件
3.在App.vue中设置页面组件占位符
4.浏览器带着指定 url链接 访问vue项目服务器，router组件就会根据 请求路径 匹配 映射组件，去替换App.vue中设置页面组件占位符
      eg: 请求路径 /user => 要渲染的组件 User.vue => 替换App.vue中的 `<router-view/>`

```
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

Vue.config.productionTip = false;

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app');

```

修改后

```
import Vue from 'vue'  // 加载vue环境
import App from './App.vue'  // 导入根组件
import router from './router'  // 加载路由环境 vue-router
import store from './store'  // 加载仓库环境 vuex

Vue.config.productionTip = false;  // Tip提示
new Vue({
    el: '#app',//挂载点
    router: router,//路由
    store,
    render: function (read_root_vue) {//read_root_vue任意的一个函数,和h相同
        return read_root_vue(App)
    }
});


```

## store下的index.js

## 前端的浏览器存储

1. cookie:可以设置过期时间
2. sessionStore:关闭浏览器消失
3. localStore:永久存储
4. store创库：刷新消失

## store

store是全局的单列

1. 在任何一个组件逻辑中：this.$store.state.car 访问或是修改
2. 在任何一个组件模板中：$store.state.car 访问或是修改
3. 页面重新加载，数据就重置（面向移动端开发）

存储

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex);
export default new Vuex.Store({
//在store厂库的内部，一般在state内写要存入的数据
    state: {
        //car: {
            //name: '默认',
            //price: 0
        //}
    },
    mutations: {},
    actions: {},
    modules: {}
})

```

存储

```javascript
//this代表的是vue对象
//this.$store.state.car = car;


export default {
        name: "Car",
        props: ['car'],
        methods: {
            goDetail(car) {
                // 先将要显示的汽车对象存储到仓库，详情页加载后，自己去仓库中获取
                // console.log(this.$store.state);
                this.$store.state.car = car;
                this.$router.push('/car/detail')
            }
        }
    }
```

取出,一般使用，创建的时候create

```javascript
//this.car = this.$store.state.car;


    export default {
        name: "CarDetail",
        data() {
            return {
                car: {}
            }
        },
        created() {
            // console.log(this.$store.state.car);
            // this.car = {name: '五菱宏光', price: 120}
            this.car = this.$store.state.car;
        }
    }
```



# 组件的使用规则

1. .vue文件就是一个组件：html、css、js
2. html由template标签提供：有且只有一个根标签
3. css由style标签管理：style标签添加scope属性，保证样式只在该组件内部起作用(样式组件化)
4. js由script标签管理：内部书写的就是组件{}语法，但是一定要导出 export default

**导出：**

```javascript
    export default {
        name: 'home',
        components: {
        },
        data() {
            return {
            }
        }
    }
```

**导入：**

```javascript
import Home from '../views/Home.vue
```

**注册**

```javascript
    export default {
        name: 'home',
        // 2、注册要使用的小组件
        components: {
            Nav,
            Footer,
            Book,
        },
        data() {
            return {
                books
            }
        }
    }
```

**使用**

在`<template></template>`内部使用

```javascript
<template>
    <div class="home">
        <!--vue项目环境下，模板也受vue环境控制，使用标签支持大小写-->
        <!--3、使用导入的小组件-->
        <Nav></Nav>

        <div class="main">
            <!-- key属性是为标签建立缓存的标识，不能重复，且在循环组件下，必须设置 -->
            <Book v-for="book in books" :book="book" :key="book.title" />
        </div>

        <Footer></Footer>
    </div>
    
</template>
```



# 在components文件夹下的组件路由跳转的方式

1. router-link会别解析为a标签，但是不能直接写a，因为a跳转回刷新页面

## 方式1

```
<router-link :to="'/book/detail/' + book.id">{{ book.title }}</router-link>
```

## 方式2

```
<router-link :to="{name: 'book-detail', params: {pk: book.id}}">{{ book.title }}</router-link>

```



## 方式3

```
this.$router.push(`/book/detail/${id}`);
```



## 方式4

```
this.$router.push({
    name: 'book-detail',
    params: {pk: id},
});
```

## 方式5

```javascript
this.$router.go(-1)//向后跳转1页
this.$router.go(-2)//向后跳转2页
this.$router.go(1)//向前跳转1页
this.$router.go(2)//向前跳转2页
```



# 在roter下的index.js路由配置的方式

1. 路由规则表：注册页面组件，与url路径形成映射关系
2. 首先都需要将所需要的组件导入到本文件夹下，如：`import Home from '../views/Home.vue'`

## 方式1：默认链接

```javascript
   {
        path: '/',
        name: 'home',
        component: Home
    },
```

## 方式2：重定向

```javascript
    {
        path: '/index',
        redirect: '/'
    },
```

## 方式3：新链接

```javascript
    {
        path: '/user',
        name: 'user',
        component: User
    },
```

## 方式4：有名分组

```javascript
    {
        path: '/book/detail/:pk',
        name: 'book-detail',
        component: BookDetail
    },
```

## 方式5：另外一种链接

另外一种导入方式

`component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')`等同于`component: () => import( '../views/About.vue')`等同于`import About from '../views/About.vue`

```javascript
{
    path: '/about',
    name: 'about',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
}
```

# src文件下静态资源如何加载

1. 当组件自己使用的时候，静态资源的加载可以使用相对路径`'../assets/img/西游记.jpg'`
2. 前台自己提供的静态资源，在传输后再使用(组件间交互)，要才有require函数来加载资源`let img1 = require('../assets/img/西游记.jpg');`就是require(资源的相对路径)

# vue的配置问题

## 配置全局css样式

```javascript
//import '@/assets/css/global.css' 方法1
require('@/assets/css/global.css');
```

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191115195735762-1353738911.png)

## 配置全局settings.js

```javascript
import settings from '@/assets/js/settings'
Vue.prototype.$settings = settings;//原生的配置，在调用的时候可以方便使用
```

```
//settings.js
//导出
export default {
    base_url: 'http://localhost:8000',
}

```

**使用**

```javascript
this.$settings.base_url
```

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191115195733429-1135226043.png)

# Vue之v-for循环中key属性注意事项

当Vue用 v-for 正在更新已渲染过的元素列表是，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue将不是移动DOM元素来匹配数据项的改变，而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。

为了给Vue一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性。key属性的类型只能为 string或者number类型。