[TOC]

# 补充

## pip是什么

相当于终端的应用商城，可以找到你需要的资源并且进行下载

pip list 打印你下载的所有资源

pip unstaill ‘资源包’ 卸载资源包

## nmp

npm 相当于pip

安装node产生npm

node是c++编写的，执行js



# Vue-CLI 项目搭建

## 环境搭建

- 安装node

[node下载网站](https://nodejs.org/zh-cn/download/)

```
官网下载安装包，傻瓜式安装：https://nodejs.org/zh-cn/

```

安装的时候只要修改一下安装路径就可以（也可以不修改），其他的可以不选

安装完成后

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113222203745-1787040088.png)

ctrl+c退出

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113222229388-497302251.png)

- nmp都是从国外的网站进行下载资源的，速度比较慢
- 安装cnpm，换成国内的网站，让速度加快
- 这一步是更换镜像，让我们下载的速度比较快，

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113222305187-1659081298.png)

- 安装脚手架
- 就像盖房子一样的脚手架，就是从这里演示过来的
- 和django不同，安装之后直接就会安装好diango的脚手架
  - 安装完成后可在cmd中进行输入`vue`查看vue的环境

```
cnpm install -g @vue/cli
```

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113222322410-958253734.png)

- 清空缓存处理
- 上面安装如何出现错误，终端安装失败时，可以清空 npm缓存 再重复执行失败的步骤

```
npm cache clean --force
```

或者

```
cnpm cache clean --force
```



# 项目的创建

## 创建项目

- 会在当前目录下进行创建，所以我们要先进入自己想创建项目的路径

```
vue create 项目名
// 要提前进入目标目录(项目应该创建在哪个目录下)
/ 选择自定义方式创建项目，选取Router, Vuex插件

```

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113222502412-1348833661.png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113222532380-688321647.png)





![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113224922959-626906047.png)

### 选择默认配置会直接进行安装 



![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113222609706-1040882412.png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113224925402-840796491.png)

### 下面选择自己的配置

一般情况下给出的选择给出的大写选择，大写选择是推荐选择

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113225011911-1712877650.png)
![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113225013735-132424345.png)

babel：在vue中采用es6语法，Babel框架就是把es6语言转换成es5语言执行的框架

typescript:用ts语言来写，我们用原生的js

progressive：是一个前端的集合，技术集合，优化我们的项目，就像su优化,

router：路由，把指定的连接请求分配到相应的界面

vuex：仓库，用来存储信息，全局的单列，在任何组件都额能拿到，可以进行组件之间的传参，一般不适用，存储的时候浏览器不饿能刷新，刷新会重置

css：预处理器

linter/formatter:格式,给配置文件，格式化代码

unit testing:测试脚本

E2E Testing：测试脚本

**一般选择router/vuex**



检查语法

ESLint与错误预防

只有ESLint + Airbnb配置默认配置

ESLint标准配置

ESLint +漂亮

采用某种已经有的配置，默认第一个

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191114194902000-971808329.png)

可以不选择语法检测，避免语法检测

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191114194918113-1994450244.png)

配置文件的存储问题

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191114194910025-1143329489.png)



在接下来的选项中

与git有关

最后一个选择是否保存自己的配置，并且下次下载的时候，会直接下载相同的配置

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113225015563-58455320.png)

接下来下载项目依赖，完成项目的创建

## 启动/停止项目

```
// 要提前进入项目根目录
//启动项目
npm run serve
或者
cnmp run serve

//停止项目
ctrl+c
```

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191114092519282-375352371.png)

network是局域网络可以访问

## 打包项目

```
npm run build
// 要在项目根目录下进行打包操作
```

## 创建项目第二种方式

```
vue ui
1.启动一个socket，可以进行创建，实现可视化创建

```

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113222550408-1186785913.png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191113222558224-495065345.png)



# 认识项目

## vue项目目录结构分析

```
├── v-proj
|	├── node_modules  	// 当前项目所有依赖，一般不可以移植给其他电脑环境
|	├── public		//共有的资源	
|	|	├── favicon.ico	// 标签图标
|	|	└── index.html	// 当前项目唯一的页面，也是我们写带代码的地方
|	├── src
|	|	├── assets		// 静态资源img、css、js
|	|	├── components	// 小组件
|	|	├── views		// 页面组件
|	|	├── App.vue		// 根组件
|	|	├── main.js		// 全局脚本文件（项目的入口）
|	|	├── router
|	|	|	└── index.js// 路由脚本文件（配置路由 url链接 与 页面组件的映射关系）
|	|	└── store	
|	|	|	└── index.js// 仓库脚本文件（vuex插件的配置文件，数据仓库）状态库文件
|	├── README.md
└	└── package.json等配置文件
```

配置文件：vue.config.js

```
module.exports={
    devServer: {
        port: 8888
    }
}
// 修改端口,选做
```

- main.js

```
new Vue({
    el: "#app",
    router: router,
    store: store,
    render: function (h) {
        return h(App)
    }
})
```

- .vue文件

```

<template>
    <!-- 模板区域 -->
</template>
<script>
    // 逻辑代码区域
    // 该语法和script绑定出现
    export default {
        
    }
</script>
<style scoped>
    /* 样式区域 */
    /* scoped表示这里的样式只适用于组件内部, scoped与style绑定出现 */
</style>
```

## vue组件(.vue文件)

```python
# 1) template：有且只有一个根标签
# 2) script：必须将组件对象导出 export default {}
# 3) style: style标签明确scoped属性，代表该样式只在组件内部起作用(样式的组件化)
```

```vue
<template>
    <div class="test">
        
    </div>
</template>

<script>
    export default {
        name: "Test"
    }
</script>

<style scoped>

</style>
```





## 全局脚本文件main.js（项目入口）

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

Vue.config.productionTip = false

new Vue({
    router,
    store,
    render: h => h(App)
}).$mount('#app')

```

### 改写

```js
import Vue from 'vue'  // 加载vue环境
import App from './App.vue'  // 加载根组件
import router from './router'  // 加载路由环境
import store from './store'  // 加载数据仓库环境

Vue.config.productionTip = false
new Vue({
    el: '#app',
    router,
    store,
    render: function (readFn) {
        return readFn(App);
    },
});

```



# 拿取别人的vue代码

1. 创建一个文件

2. 拉取别人的文件内的文件

   除了node_modules不拉取，其他都可以拉取，主要拉取下面文件夹

   public/src/package.json

   

4. 执行

   * 切换目录
   * `cnpm install`下载依赖包
   * 然后可以执行`cnpm run serve`



