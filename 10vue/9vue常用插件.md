[TOC]

# 项目功能插件

# element-ui

[饿了么开发的框架element：https://element.eleme.cn/#/zh-CN](https://element.eleme.cn/#/zh-CN)

配置element-ui插件
1、安装：`cnpm install element-ui`
2、配置环境

```javascript
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);
```

# bootstrap

[bootstrap：https://v3.bootcss.com/](https://v3.bootcss.com/)

## 安装插件(配置jq和bs的环境)

### jQuery

```
>: cnpm install jquery
```

### vue/cli 3 配置jQuery：在vue.config.js中配置(没有，手动项目根目录下新建)

**vue.config.js是全局配置，并且项目没有直接给出，所以需要自己动手来创建，这一个js文件**

```js
// 修改该文件内容后，只有重启，才能同步配置
const webpack = require("webpack");

module.exports = {
    configureWebpack: {
        plugins: [
            new webpack.ProvidePlugin({
                //下面的都是代表jq
                $: "jquery",
                jQuery: "jquery",
                //"window.$": "jquery",//多一种使用方式，可以去掉
                "window.jQuery": "jquery",
                Popper: ["popper.js", "default"]
            })
        ]
    }
};
```

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191115195738217-286283245.png)

### BootStrap

安装第三个版本

```
>: cnpm install bootstrap@3
```

### vue/cli 3 配置BootStrap：在main.js中配置

```js
import "bootstrap"//加载bs的逻辑，(bootstrap与jquery)
import "bootstrap/dist/css/bootstrap.css"//导入bootstrap
```

# axios

**配置axios来完成前后台ajax请求**

```javascript
安装 axios(ajax)的命令
cnpm install axios
// 为项目配置全局axios，配置环境
import Axios from 'axios'
//Vue.prototype.$ajax = Axios;//命名方式1
Vue.prototype.$axios = Axios;//命名方式2，vue框架常使用
```

```javascript
 export default {
        name: "Cars",
        components: {
            Car
        },
        data() {
            return {
                cars_data: []
            }
        },
        created() {
            // 组件一加载，就从后台请求数据

            // 1、jq完成ajax请求,一般不使用
            /*
            $.ajax({
                url: this.$settings.base_url + '/cars/',
                type: 'get',
                success(response) {
                    console.log(response)
                }
            });
            */

           
        }
    }
```

**使用**

```javascript
 export default {
        name: "Cars",
        components: {
            Car
        },
        data() {
            return {
                cars_data: []
            }
        },
        created() {
// 2、vue有专门用来完成ajax请求的插件：axios
            this.$ajax({
                url: this.$settings.base_url + '/cars/',
                method: 'get',
                params: {
                    // url拼接的数据
                },
                data: {
                    // 请求携带的数据报数据
                }
            }).then((response) => {
                console.log(response);
                this.cars_data = response.data;
            }).catch(error => {
                console.log(error)
            })
```

```javascript

```

# vue-cookies

```javascript
// 配置cookie操作插件命令
// cnpm install vue-cookies
// 为项目配置全局vue-cookie
import Cookies from 'vue-cookies'

// 将插件设置给Vue原型,作为全局的属性,在任何地方都可以通过this.$cookie进行访问
Vue.prototype.$cookies = Cookies;
```

```javascript
// 有响应的登录认证码，存储在cookie中
// 设置(持久化存储val的值到cookie中)：set(key, val, exp)
// 获取cookie中val字段值：get(key)
// 删除cookie键值对：remove(key)
let token =  response.data.result;
if (token) {
// this.$cookies.set('token', token, '2d');
// this.$cookies.set('token', token, 2 * 24 * 3600 * 365);
// console.log(this.$cookies.get('token'));
// this.$cookies.remove('token');
           }
```



# vue-router（vue提前安装）

```
{
    path: '/',
    name: 'home',
    // 路由的重定向
    redirect: '/home'
}

{
    // 一级路由, 在根组件中被渲染, 替换根组件的<router-view/>标签
    path: '/one-view',
    name: 'one',
    component: () => import('./views/OneView.vue')
}

{
    // 多级路由, 在根组件中被渲染, 替换根组件的<router-view/>标签
    path: '/one-view/one-detail',
    component: () => import('./views/OneDetail.vue'),
    // 子路由, 在所属路由指向的组件中被渲染, 替换该组件(OneDetail)的<router-view/>标签
    children: [{
        path: 'show',
        component: () => import('./components/OneShow.vue')
    }]
}
```

```
<!-- router-link渲染为a标签 -->
<router-link to="/">Home</router-link> |
<router-link to="/about">About</router-link> |
<router-link :to="{name: 'one'}">One</router-link> |

<!-- 为路由渲染的组件占位 -->
<router-view />
```

```
a.router-link-exact-active {
    color: #42b983;
}
```



```
// router的逻辑转跳
this.$router.push('/one-view')

// router采用history方式访问上一级
this.$router.go(-1)
```

# vuex（仓库vue提前安装）

```
// 在任何一个组件中,均可以通过this.$store.state.msg访问msg的数据
// state永远只能拥有一种状态值
state: {
    msg: "状态管理器"
},
// 让state拥有多个状态值
mutations: {
    // 在一个一个组件中,均可以通过this.$store.commit('setMsg', new_msg)来修改state中的msg
    setMsg(state, new_msg) {
        state.msg = new_msg
    }
},
// 让mutations拥有多个状态值
actions: {

}
```

