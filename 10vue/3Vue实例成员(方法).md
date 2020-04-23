# Vue实例

# el：创建实例

```
new Vue({
    el: '#app'
})
// 实例与页面挂载点一一对应
// 一个页面中可以出现多个实例对应多个挂载点
// 实例只操作挂载点内部内容
```

- 每个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始的
- 一个 Vue 应用由一个通过 new Vue 创建的根 Vue 实例，以及可选的嵌套的、可复用的组件树组成。

## 数据与方法

###  数据

- 当一个 Vue 实例被创建时，它向 Vue 的响应式系统中加入了其 data 对象中能找到的所有的属性。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。
- 只有当实例被创建时 data 中存在的属性才是响应式的
- 如果你知道你会在晚些时候需要一个属性，但是一开始它为空或不存在，那么你仅需要设置一些初始值

### 实例方法

Vue 实例还暴露了一些有用的实例属性与方法。它们都有前缀 $，以便与用户定义的属性区分开来

- vm.$el
- vm.$data
- vm.$watch(dataAttr, fn)

## data：数据

```
<div id='app'>
    {{ msg }}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            msg: '数据',
        }
    })
    console.log(app.$data.msg);
    console.log(app.msg);
</script>
<!-- data为插件表达式中的变量提供数据 -->
<!-- data中的数据可以通过Vue实例直接或间接访问-->
```

# methods：方法

##  methods

methods用来装载可以调用的函数，你可以直接通过 Vue 实例访问这些方法，或者在指令表达式中使用。方法中的 this 自动绑定为 Vue 实例。

**注意**，不应该使用箭头函数来定义 methods 函数（例如 plus: () => this.a++）。理由是箭头函数绑定了父级作用域的上下文，所以 this 将不会按照期望指向 Vue 实例，this.a 将是 undefined。示例代码如下。

如果你要通过对 DOM 的操作来触发这些函数，那么应该使用 v-on 对操作和事件进行绑定



```
<style>
    .box { background-color: orange }
</style>
<div id='app'>
    <p class="box" v-on:click="pClick">测试</p>
    <p class="box" v-on:mouseover="pOver">测试</p>
</div>
<script>
    var app = new Vue({
        el: '#app',
        methods: {
            pClick () {
                // 点击测试
            },
            pOver () {
                // 悬浮测试
            }
        }
    })
</script>
<!-- 了解v-on:为事件绑定的指令 -->
<!-- methods为事件提供实现体-->
```

# computed：计算

1、computed中定义的是方法属性，data中定义的也是属性，所以不需要重复定义(省略data中的)
2、方法属性的值来源于绑定的方法的返回值
3、方法属性必须在页面中渲染后，绑定的方法才会被启用(调用)
4、方法中出现的所有变量都会被监听，任何变量发生值更新都会调用一次绑定的方法，重新更新一下方法属性的值
5、方法属性值不能手动设置，必须通过绑定的方法进行设置

模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护,这时候应该使用计算属性



```
<body>
    <div id="app">
        <input type="text" v-model="v1">
        +
        <input type="text" v-model="v2">
        =
        <button>{{ res }}</button>

    </div>
</body>
<script src="js/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            v1: '',
            v2: '',
            // res: '结果',
        },
        computed: {
            res () {
                console.log('该方法被调用了');
                return this.v1 && this.v2 ? +this.v1 + +this.v2 : '结果';
            }
        }
    })
</script>
<script>
    console.log(1 + '2');
    console.log(1 - '2');
    console.log(+'2');
</script>
```

# watch：监听

1、watch中给已有的属性设置监听方法
2、监听的属性值一旦发生更新，就会调用监听方法，在方法中完成相应逻辑
3、监听方法不需要返回值(返回值只有主动结束方法的作用)

虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。这就是为什么 Vue 通过 watch 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的

```html
<body>
    <div id="app">
        <p>
            姓名：<input type="text" v-model="full_name">
        </p>
        <p>
            姓：<span>{{ first_name }}</span>
        </p>
        <p>
            名：<span>{{ last_name }}</span>
        </p>
    </div>
</body>
<script src="js/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            full_name: '',
            first_name: '',
            last_name: '',
        },
        watch: {
            full_name() {
                if (this.full_name.length === 2) {
                    k_v_arr = this.full_name.split('');
                    this.first_name = k_v_arr[0];
                    this.last_name = k_v_arr[1];
                }
            }
        }
    })
</script>
```

# methodes,computed,watch三者区别

它们三者都是以函数为主体，但是它们之间却各有区别。

## 计算属行与方法

我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是计算属性是基于它们的依赖进行缓存的。计算属性只有在它的相关依赖发生改变时才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。

相比之下，每当触发重新渲染时，调用方法将总会再次执行函数。

## 计算属性与侦听属性

- watch擅长处理的场景：一个数据影响多个数据
- computed擅长处理的场景：一个数据受多个数据影响

# delimiters：分隔符

用来修改插值表达式符号

`delimiters: ['{[', ']}']`

```
<body>
    <div id="app">
        <p>{{ num }}</p>
        <!--<p>{[ num ]}</p>-->
        <p>{ num ]}</p>
    </div>
</body>
<script src="js/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            num: 100
        },
        // 用来修改插值表达式符号
        // delimiters: ['{[', ']}'],
        delimiters: ['{', ']}'],
    })
</script>
```