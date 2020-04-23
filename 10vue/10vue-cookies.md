# vue之vue前端操作cookies

安装：

```
npm install vue-cookies
```

使用：

```
import Vue from 'Vue'
import VueCookies from 'vue-cookies'
Vue.use(VueCookies)
```

```javascript
// 有响应的登录认证码，存储在cookie中
// 设置：set(key, val, exp)
// 取值：get(key)
// 删除：remove(key)
let token =  response.data.result;
if (token) {
// this.$cookies.set('token', token, '2d');
// this.$cookies.set('token', token, 2 * 24 * 3600 * 365);
// console.log(this.$cookies.get('token'));
// this.$cookies.remove('token');
           }
```

Api：

　　设置 cookie：

```
this.$cookies.set(key, val, exp)   
//return this
```

　　获取cookie

```
this.$cookies.get(key)       
// return value   
```

　　删除 cookie

```
this.$cookies.remove(key)   
// return  false or true , warning： next version return this； use isKey(keyname) return true/false,please
```

　　查看一个cookie是否存在（通过`keyName`）

```
this.$cookies.isKey(key)        
// return false or true
```

　　获取所有cookie名称

```
this.$cookies.keys()  
// return a array
```

来源：https://www.cnblogs.com/liuqingzheng/articles/9850874.html