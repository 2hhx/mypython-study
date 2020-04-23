## ECMAScript和JavaScript的关系



1996年11月，JavaScript的创造者--Netscape公司，决定将JavaScript提交给国际标准化组织ECMA，希望这门语言能够成为国际标准。次年，ECMA发布262号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言称为ECMAScript，这个版本就是1.0版。

该标准一开始就是针对JavaScript语言制定的，但是没有称其为JavaScript，有两个方面的原因。一是商标，JavaScript本身已被Netscape注册为商标。而是想体现这门语言的制定者是ECMA，而不是Netscape，这样有利于保证这门语言的开发性和中立性。

因此ECMAScript和JavaScript的关系是，前者是后者的规格，后者是前者的一种实现。

## ECMAScript的历史



| 年份 | 名称           | 描述                                             |
| ---- | -------------- | ------------------------------------------------ |
| 1997 | ECMAScript 1   | 第一个版本                                       |
| 1998 | ECMAScript 2   | 版本变更                                         |
| 1999 | ECMAScript 3   | 添加正则表达式添加try/catch                      |
|      | ECMAScript 4   | 没有发布                                         |
| 2009 | ECMAScript 5   | 添加"strict mode"严格模式添加JSON支持            |
| 2011 | ECMAScript 5.1 | 版本变更                                         |
| 2015 | ECMAScript 6   | 添加类和模块                                     |
| 2016 | ECMAScript 7   | 增加指数运算符（**）增加Array.prototype.includes |

 

*注：ES6就是指ECMAScript 6。*

尽管 ECMAScript 是一个重要的标准，但它并不是 JavaScript 唯一的部分，当然，也不是唯一被标准化的部分。实际上，一个完整的 JavaScript 实现是由以下 3 个不同部分组成的：

- 核心（ECMAScript） 
- 文档对象模型（DOM） Document object model (整合js，css，html)
- 浏览器对象模型（BOM） Broswer object model（整合js和浏览器）

简单地说，ECMAScript 描述了JavaScript语言本身的相关内容。

JavaScript 是脚本语言
JavaScript 是一种轻量级的编程语言。

JavaScript 是可插入 HTML 页面的编程代码。

JavaScript 插入 HTML 页面后，可由所有的现代浏览器执行。

## JavaScript引入方式



### Script标签内写代码

```html
<script> // 在这里写你的JS代码 </script>
```




### 引入额外的JS文件

```html
<script src="myscript.js"></script>
```




## JavaScript语言规范



### 注释（注释是代码之母）

```js
// 这是单行注释 
/* 这是多行注释 */
```



 

### 结束符

JavaScript中的语句要以分号（;）为结束符。

## JavaScript语言基础



### 变量声明

1. JavaScript的变量名可以使用_，数字，字母，$组成，不能以数字开头。
2. 声明变量使用 var 变量名; 的格式来进行声明

```js
var name = "Alex";

var age = 18;
```



 

注意：

变量名是区分大小写的。

推荐使用驼峰式命名规则。

保留字不能用做变量名。

*补充：*

*ES6新增了let命令，用于声明变量。其用法类似于var，但是所声明的变量只在let命令所在的代码块内有效。例如：for循环的计数器就很适合使用let命令。*

```js
for (let i=0;i<arr.length;i++){...}
```



 

*ES6新增const用来声明常量。一旦声明，其值就不能改变。*

```js
const PI = 3.1415;

PI // 3.1415

PI = 3 // TypeError: "PI" is read-only

 
```



再次补充：

```
abstract            摘要
boolean             布尔
byte                字节
char                字符
class               类
const               常量
debugger            调试器
double              双
enum                枚举
export              出口
extends             扩展
final               最后
float               浮动
goto                转到
implements          实现了
import              进口
int                 int
interface           接口
long                长
native              本地的
package             包
private             私人
protected           受保护的
public              公共
short               短
static              静态
super               超级
synchronized        同步
throws              抛出
transient           瞬态
volatile            挥发性
```



## JavaScript数据类型



**JavaScript拥有动态类型**

```js
var x; // 此时x是undefined 
var x = 1; // 此时x是数字 
var x = "Alex" // 此时x是字符串
```



 

### **数值(Number)**

JavaScript不区分整型和浮点型，就只有一种数字类型。

```js
var a = 12.34;

var b = 20;

var c = 123e5; // 12300000

var d = 123e-5; // 0.00123
```



 

还有一种NaN，表示不是一个数字（Not a Number）。

常用方法：

```js
parseInt("123") // 返回123

parseInt("ABC") // 返回NaN,NaN属性是代表非数字值的特殊值。该属性用于指示某个值不是数字。

parseFloat("123.456") // 返回123.456
```



 

### **字符串(String)**

```js
var a = "Hello"

var b = "world;

var c = a + b;

console.log(c); // 得到Helloworld
```



 

常用方法：

| 方法                       | 说明               |
| -------------------------- | ------------------ |
| .length                    | 返回长度           |
| .trim()                    | 移除空白           |
| .trimLeft()                | 移除左边的空白     |
| .trimRight()               | 移除右边的空白     |
| .charAt(n)                 | 返回第n个字符      |
| .concat(value, ...)        | 拼接               |
| .indexOf(substring, start) | 子序列位置         |
| .substring(from, to)       | 根据索引获取子序列 |
| .slice(start, end)         | 切片               |
| .toLowerCase()             | 小写               |
| .toUpperCase()             | 大写               |
| .split(delimiter, limit)   | 分割               |

 

拼接字符串一般使用“+”

 ```js
string.slice(start, stop)和string.substring(start, stop)：

两者的相同点：
如果start等于end，返回空字符串
如果stop参数省略，则取到字符串末
如果某个参数超过string的长度，这个参数会被替换为string的长度

substirng()的特点：
如果 start > stop ，start和stop将被交换
如果参数是负数或者不是数字，将会被0替换

silce()的特点：
如果 start > stop 不会交换两者
如果start小于0，则切割从字符串末尾往前数的第abs(start)个的字符开始(包括该位置的字符)
如果stop小于0，则切割在从字符串末尾往前数的第abs(stop)个字符结束(不包含该位置字符)
 ```



*补充：*

*ES6中引入了模板字符串。模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当做普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。*



```js
// 普通字符串

`这是普通字符串！`

// 多行文本

`这是多行的 文本`

// 字符串中嵌入变量

var name = "cxw",

time = "today";

`Hello ${name}, how are you ${time}?`
```

*注意：*

*如果模板字符串中需要使用反引号，则在其前面要用反斜杠转义。*

JSHint启用ES6语法支持: jshint esversion: 6 

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191014171054570-2139255197.png)

### 布尔值(Boolean)

区别于Python，true和false都是小写。

```js
var a = true;

var b = false;
```



 

==""(空字符串)、0、null、undefined、NaN都是false。==

### null和undefined

- null表示值是空，一般在需要指定或清空一个变量时才会使用，如 name=null;
- undefined表示当声明一个变量但未初始化时，该变量的默认值是undefined。还有就是函数无明确的返回值时，返回的也是undefined。

null表示变量的值是空(null可以手动清空一个变量的值，使得该变量变为object类型，值为null)，undefined则表示只声明了变量，但还没有赋值。

还不明白，上图吧！

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191014173028524-1036102142.png)



### 对象(Object)

JavaScript 中的所有事物都是对象：字符串、数值、数组、函数...此外，JavaScript 允许自定义对象。

JavaScript 提供多个内建对象，比如 String、Date、Array 等等。

对象只是带有属性和方法的特殊数据类型。

### **数组**

数组对象的作用是：使用单独的变量名来存储一系列的值。类似于Python中的列表。

```js
var a = [123, "ABC"];

console.log(a[1]);

// 输出"ABC"
```



 

 常用方法：

| 方法               | 说明                                       |
| ------------------ | ------------------------------------------ |
| .length            | 数组的大小                                 |
| .push(ele)         | 尾部追加元素                               |
| .pop()             | 获取尾部的元素                             |
| .unshift(ele)      | 头部插入元素                               |
| .shift()           | 头部移除元素                               |
| .slice(start, end) | 切片                                       |
| .reverse()         | 反转                                       |
| .join(seq)         | 将数组元素连接成字符串                     |
| .concat(val, ...)  | 连接数组                                   |
| .sort()            | 排序                                       |
| .forEach()         | 将数组的每个元素传递给回调函数             |
| .splice()          | 删除元素，并向数组添加新元素。             |
| .map()             | 返回一个数组元素调用函数处理后的值的新数组 |



![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191014174423546-405492598.png)





### forEach()

**语法：**

```js
forEach(function(currentValue, index, arr), thisValue)
```



**参数：**

| 参数                                 | 描述                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| *function(currentValue, index, arr)* | 必需。 数组中每个元素需要调用的函数。 函数参数:参数描述*currentValue*必需。当前元素*index*可选。当前元素的索引值。*arr*可选。当前元素所属的数组对象。 |
| *thisValue*                          | 可选。传递给函数的值一般用 "this" 值。 如果这个参数为空， "undefined" 会传递给 "this" 值 |



![img](https://images2018.cnblogs.com/blog/1342004/201807/1342004-20180711095145556-100767260.png)





![img](https://images2018.cnblogs.com/blog/1342004/201807/1342004-20180711095200369-891844168.png)





### splice()

**语法：**

```js
splice(index,howmany,item1,.....,itemX)
```



**参数：** 

| 参数                  | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| *index*               | 必需。规定从何处添加/删除元素。 该参数是开始插入和（或）删除的数组元素的下标，必须是数字。 |
| *howmany*             | 必需。规定应该删除多少元素。必须是数字，但可以是 "0"。 如果未规定此参数，则删除从 index 开始到原数组结尾的所有元素。 |
| *item1*, ..., *itemX* | 可选。要添加到数组的新元素                                   |

###  

### map()

**语法：**

```js
map(function(currentValue,index,arr), thisValue)
```

<table><thead><tr><td>参数</td><td>描述</td></tr></thead></table>
<br /><table><thead><tr><td>参数</td><td>描述</td></tr></thead></table><br /><br />dfddfdf

**参数：**

| 参数                                | 描述                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| *function(currentValue, index,arr)* | 必须。函数，数组中的每个元素都会执行这个函数 函数参数: *currentValue*必须。当前元素的值<br />*index*可选。当期元素的索引值<br />*arr*可选。当期元素属于的数组对象<br /> |
| *thisValue*                         | 可选。对象作为该执行回调时使用，传递给函数，用作 "this" 的值。 如果省略了 thisValue ，"this" 的值为 "undefined" |

![img](https://images2018.cnblogs.com/blog/1342004/201807/1342004-20180711095244157-693529215.png)

注意：

```js
关于sort()需要注意：

如果调用该方法时没有使用参数，将按字母顺序对数组中的元素进行排序，说得更精确点，是按照字符编码的顺序进行排序。要实现这一点，首先应把数组的元素都转换成字符串（如有必要），以便进行比较。

如果想按照其他标准进行排序，就需要提供比较函数，该函数要比较两个值，然后返回一个用于说明这两个值的相对顺序的数字。比较函数应该具有两个参数 a 和 b，其返回值如下：

若 a 小于 b，在排序后的数组中 a 应该出现在 b 之前，则返回一个小于 0 的值。
若 a 等于 b，则返回 0。
若 a 大于 b，则返回一个大于 0 的值。

function sortNumber(a,b){
    return a - b
}
var arr1 = [11, 100, 22, 55, 33, 44]
arr1.sort(sortNumber)
```



可以使用以下方式遍历数组中的元素：

```js
var a = [10, 20, 30, 40];

for (var i=0;i<a.length;i++)

{

　　 console.log(i);

}

 
```





**补充：**

*ES6新引入了一种新的原始数据类型（Symbol），表示独一无二的值。它是JavaScript语言的第7种数据类型。*

### 类型查询

```js
typeof "abc" // "string"

typeof null // "object"

typeof true // "boolean"

typeof 123 // "number"
```



 

typeof是一个一元运算符（就像++，--，！，- 等一元运算符），不是一个函数，也不是一个语句。

对变量或值调用 typeof 运算符将返回下列值之一：

- undefined - 如果变量是 Undefined 类型的
- boolean - 如果变量是 Boolean 类型的
- number - 如果变量是 Number 类型的
- string - 如果变量是 String 类型的
- object - 如果变量是一种引用类型或 Null 类型的

 

## 运算符



### 算数运算符



```js
\+ - * / % ++ --

var x=10;
var res1=x++;
var res2=++x;

res1;
10
res2;
12

**这里由于的x++和++x在出现赋值运算式，x++会先赋值再进行自增1运算，而++x会先进行自增运算再赋值！**
```



### **比较运算符**

```js
>
 >=
 <
 <=
 !=
 ==
 ===
 !==
```



 

**注意：**

```js
1 == “1” // true 弱等于 
1 === "1" // false 强等于
//上面这张情况出现的原因在于JS是一门弱类型语言(会自动转换数据类型)，所以当你用两个等号进行比较时，JS内部会自动先将
//数值类型的1转换成字符串类型的1再进行比较，所以我们以后写JS涉及到比较时尽量用三等号来强制限制类型，防止判断错误
```



### **逻辑运算符**

&& || !

 

### **赋值运算符**

= += -= *= /=

 

## 流程控制



### if-else



```js
var a = 10;

if (a > 5)

{

　　console.log("yes");

}else {

　　console.log("no");

}
```



### if-else if-else 



```js
var a = 10;

if (a > 5){

　　 console.log("a > 5");

}else if (a < 5){

　　console.log("a < 5");

}else {

　　console.log("a = 5");

}
```





### switch



```js
 var day = new Date().getDay();

switch (day) {

　　 case 0:

　　 console.log("Sunday");

　　 break;

　　case 1:

　　console.log("Monday");

　　 break;

default:

console.log("...") }
```





switch中的case子句通常都会加break语句，否则程序会继续执行后续case中的语句。

### for

```
for (var i=0;i<10;i++) {

　　console.log(i);

}
```



 

### while

```
var i = 0;

while (i < 10) {

　　 console.log(i); i++;

}
```



 

### 三元运算



```js
var a = 1;

var b = 2;

var c = a > b ? a : b
//这里的三元运算顺序是先写判断条件a>b再写条件成立返回的值为a,条件不成立返回的值为b;三元运算可以嵌套使用；

var a=10,b=20;
var x=a>b ?a:(b=="20")?a:b;

x
10
```



## 函数



### 函数定义

JavaScript中的函数和Python中的非常类似，只是定义方式有点区别。

```js
// 普通函数定义

function f1(){

　　 console.log("Hello world!");

}

// 带参数的函数

function f2(a, b) {

　　 console.log(arguments);

　　// 内置的arguments对象

　　 console.log(arguments.length);

　　 console.log(a, b);

}

// 带返回值的函数

function sum(a, b){

　　 return a + b;

}

sum(1, 2); // 调用函数

// 匿名函数方式

var sum = function(a, b){

　　 return a + b;

}

sum(1, 2);

 

// 立即执行函数 书写立即执行的函数，**首先先写两个括号()()这样防止书写混乱**

(function(a, b){

　　 return a + b;

})(1, 2);
```



*补充：*

```js
ES6中允许使用“箭头”（=>）定义函数。

var f = v => v; // 等同于

var f = function(v){

　　 return v;

}
```



 

如果箭头函数不需要参数或需要多个参数，就是用圆括号代表参数部分：

```js
var f = () => 5;

// 等同于

var f = function(){return 5};

var sum = (num1, num2) => num1 + num2;

// 等同于

var sum = function(num1, num2){

　　 return num1 + num2; //这里的return只能返回一个值，如果想返回多个值需要自己手动给他们包一个数组或对象中

}
```



### 函数中的arguments参数



```js
function add(a,b){

　　 console.log(a+b);

　　 console.log(arguments.length);
　　　**console.log(arguments[0]);//arguments相当于将出传入的参数全部包含，这里取得就是第一个元素1**

}

add(1,2)

 



输出：

3

2
1
```



 

*注意：*

函数只能返回一个值，如果要返回多个值，只能将其放在数组或对象中返回。

### 函数的全局变量和局部变量

**局部变量**：

在JavaScript函数内部声明的变量（使用 var）是局部变量，所以只能在函数内部访问它（该变量的作用域是函数内部）。只要函数运行完毕，本地变量就会被删除。

**全局变量：**

在函数外声明的变量是*全局*变量，网页上的所有脚本和函数都能访问它。

**变量生存周期：**

JavaScript变量的生命期从它们被声明的时间开始。

局部变量会在函数运行以后被删除。

全局变量会在页面关闭后被删除。

### 作用域

首先在函数内部查找变量，找不到则到外层函数查找，逐步找到最外层。与python作用域关系查找一模一样！

**几个例子：**

1.

```
var city = "BeiJing";

function f() {

　　 var city = "ShangHai";

　　 function inner(){

　　　　 var city = "ShenZhen";

　　　　 console.log(city);

　　　}

　　　inner();

}

f(); //输出结果是？
```



 

2.



```js
var city = "BeiJing";

function Bar() {

　　 console.log(city);

}

function f() {

　　 var city = "ShangHai";

　　 return Bar;

}

var ret = f();

　ret(); // 打印结果是？
```



 



3.闭包



```js
var city = "BeiJing";

function f(){

　　 var city = "ShangHai";

　　 function inner(){

　　　　 console.log(city);

　　 }

　　 return inner;

}

var ret = f();

ret();
```



 

## 词法分析（尝试理解）



JavaScript中在调用函数的那一瞬间，会先进行词法分析。

**词法分析的过程：**

当函数调用的前一瞬间，会先形成一个激活对象：Avtive Object（AO），并会分析以下3个方面：

1:函数参数，如果有，则将此参数赋值给AO，且值为undefined。如果没有，则不做任何操作。
2:函数局部变量，如果AO上有同名的值，则不做任何操作。如果没有，则将此变量赋值给AO，并且值为undefined。
3:函数声明，如果AO上有，则会将AO上的对象覆盖。如果没有，则不做任何操作。

函数内部无论是使用参数还是使用局部变量都到AO上找。

看两个例子：

```js
var age = 18; function foo(){ console.log(age); var age = 22; console.log(age); } foo(); // 问：执行foo()之后的结果是？
```



 

 ![img](https://images2018.cnblogs.com/blog/1342004/201806/1342004-20180618153449893-1988346820.png)

第二题：

```javascript
var age = 18; function foo(){ console.log(age); var age = 22; console.log(age); function age(){ console.log("呵呵"); } console.log(age); } foo(); // 执行后的结果是？
```



 

 ![img](https://images2018.cnblogs.com/blog/1342004/201806/1342004-20180618153712114-1827301393.png)

 



```
词法分析过程：
1、分析参数，有一个参数，形成一个 AO.age=undefine;
2、分析变量声明，有一个 var age, 发现 AO 上面已经有一个 AO.age，因此不做任何处理
3、分析函数声明，有一个 function age(){...} 声明， 则把原有的 age 覆盖成 AO.age=function(){...};

最终，AO上的属性只有一个age，并且值为一个函数声明

执行过程：
注意：执行过程中所有的值都是从AO对象上去寻找

1、执行第一个 console.log(age) 时，此时的 AO.age 是一个函数，所以第一个输出的一个函数
2、这句 var age=22; 是对 AO.age 的属性赋值， 此时AO.age=22 ，所以在第二个输出的是 2
3、同理第三个输出的还是22, 因为中间再没有改变age值的语句了
```



## 内置对象和方法



JavaScript中的所有事物都是对象：字符串、数字、数组、日期，等等。在JavaScript中，对象是拥有属性和方法的数据。

我们在学习基本数据类型的时候已经带大家了解了，JavaScript中的Number对象、String对象、Array对象等。

注意var s1 = "abc"和var s2 = new String("abc")的区别：typeof s1 --> string而 typeof s2 --> Object

![img](https://images2018.cnblogs.com/blog/867021/201803/867021-20180311224027096-1902975546.png)

### 自定义对象

JavaScript的对象（Object）本质上是键值对的集合（Hash结构），但是只能用字符串作为键。

```
var a = {"name": "Alex", "age": 18};

console.log(a.name);

console.log(a["age"]);

 
```



遍历对象中的内容：

```
var a = {"name": "Alex", "age": 18};

for (var i in a){

　　 console.log(i, a[i]);

}
```



**创建对象：**

```js
var person=new Object(); // 创建一个person对象 
person.name="Alex"; // person对象的name属性 
person.age=18; // person对象的age属性
```



 

*注意：*

*ES6中提供了Map数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当做键。*

*也就是说，Object结构提供了“字符串--值”的对应，Map结构提供了“值--值”的对应，是一种更完善的Hash结构实现。*

```js
var m = new Map();
var o = {p: "Hello World"}

m.set(o, "content"}
m.get(o)  // "content"

m.has(o)  // true
m.delete(o)  // true
m.has(o) // false
```

 **扩展：**js面向对象继承



```
// 父类构造函数
var Car = function (loc) {
  this.loc = loc;
};

// 父类方法
Car.prototype.move = function () {
  this.loc ++;
};

// 子类构造函数
var Van = function (loc) {
  Car.call(this, loc);
};

// 继承父类的方法
Van.prototype = Object.create(Car.prototype);
// 修复 constructor
Van.prototype.constructor = Van;
// 扩展方法
Van.prototype.grab = function () {
  /* ... */
};
```



### Date对象

**创建Date对象**



```
//方法1：不指定参数

var d1 = new Date();

console.log(d1.toLocaleString());

//方法2：参数为日期字符串

var d2 = new Date("2004/3/20 11:12");

console.log(d2.toLocaleString());

var d3 = new Date("04/03/20 11:12");

console.log(d3.toLocaleString());

//方法3：参数为毫秒数

var d3 = new Date(5000);

console.log(d3.toLocaleString());

console.log(d3.toUTCString());

//方法4：参数为年月日小时分钟秒毫秒

var d4 = new Date(2004,2,20,11,12,0,300);

console.log(d4.toLocaleString()); //毫秒并不直接显示
```





**Date对象的方法：**



```


var d = new Date();

//getDate() 获取日

//getDay () 获取星期

//getMonth () 获取月（0-11）

//getFullYear () 获取完整年份

//getYear () 获取年

//getHours () 获取小时

//getMinutes () 获取分钟

//getSeconds () 获取秒

//getMilliseconds () 获取毫秒

//getTime () 返回累计毫秒数(从1970/1/1午夜)
```





**练习：**

编写代码，将当前日期按“2017-12-27 11:11 星期三”格式输出。

```html
const WEEKMAP = {  
  0:"星期天",
  1:"星期一",
  2:"星期二",
  3:"星期三",
  4:"星期四",
  5:"星期五",
  6:"星期六"
};  //定义一个数字与星期的对应关系对象


function showTime() {
    var d1 = new Date();
    var year = d1.getFullYear();
    var month = d1.getMonth() + 1;  //注意月份是从0~11
    var day = d1.getDate();
    var hour = d1.getHours();
    var minute = d1.getMinutes() < 10?"0"+d1.getMinutes():d1.getMinutes();  //三元运算

    var week = WEEKMAP[d1.getDay()];  //星期是从0~6

    var strTime = `
    ${year}-${month}-${day} ${hour}:${minute} ${week}
    `;
    console.log(strTime)
};

showTime();
```



### JSON对象

```js
var str1 = '{"name": "Alex", "age": 18}';

var obj1 = {"name": "Alex", "age": 18};

// JSON字符串转换成对象

var obj = JSON.parse(str1);

// 对象转换成JSON字符串

var str = JSON.stringify(obj1);
```

### RegExp对象（正则表达式）

```js
// 定义正则表达式两种方式
var reg1 = new RegExp("^[a-zA-Z][a-zA-Z0-9]{5,11}");
var reg2 = /^[a-zA-Z][a-zA-Z0-9]{5,9}$/;

// 正则校验数据
reg1.test('jason666')
reg2.test('jason666')

/*第一个注意事项，正则表达式中不能有空格*/ 

// 全局匹配
var s1 = 'egondsb dsb dsb';
s1.match(/s/)
s1.match(/s/g)
var reg2 = /^[a-zA-Z][a-zA-Z0-9]{5,9}$/g
reg2.test('egondsb');
reg2.test('egondsb');
reg2.lastIndex;
/*第二个注意事项，全局匹配时有一个lastIndex属性*/

// 校验时不传参数
var reg2 = /^[a-zA-Z][a-zA-Z0-9]{5,9}$/
reg2.test();
reg2.test(undefined);

var reg3 = /undefined/;
reg3.test();
```

### Math对象

```js
abs(x)      返回数的绝对值。
exp(x)      返回 e 的指数。
floor(x)    对数进行下舍入。
log(x)      返回数的自然对数（底为e）。
max(x,y)    返回 x 和 y 中的最高值。
min(x,y)    返回 x 和 y 中的最低值。
pow(x,y)    返回 x 的 y 次幂。
random()    返回 0 ~ 1 之间的随机数。
round(x)    把数四舍五入为最接近的整数。
sin(x)      返回数的正弦。
sqrt(x)     返回数的平方根。
tan(x)      返回角的正切。
```

