# 1 if-else

```go

//if判断
package main

func main() {
	//var a=16
	//1 简单使用
	//if a==10{ //这个位置不能回车，不能换行
	//	fmt.Println("10")
	//}

	//2 if-else if
	//if a==10{
	//	fmt.Println(10)
	//}else if a==11{
	//	fmt.Println(11)
	//}
	//3 if-else if -else
	//if a==10{
	//	fmt.Println(10)
	//}else if a==11 {
	//	fmt.Println(11)
	//}else {
	//	fmt.Println("不知道")
	//}
	//4 变形使用：在条件中可以加变量定义

	//if a :=10;a==10 {
	//	fmt.Println(10)
	//}


}
```



# 2 循环

```go

//循环(go中只有for循环，没有while循环，for循环可以代替while)
//关键字package ，func，if ，else
package main

func main() {
	/*
		for 初始化; 条件; 自增/自减 {

		}
	 */
	//1 简单使用
	//for a:=0;a<10 ;a++  {
	//	fmt.Println(a)
	//}
	//2 省略初始化(跟上面有什么不同？a的作用域范围不同)
	//a :=0
	//for ;a<10 ;a++  {
	//	fmt.Println(a)
	//}
	//fmt.Println(a)
	//3 省略初始化和自增自减
	//a:=0
	//for ; a<10;  {
	//	fmt.Println(a)
	//	a++
	//}
	//4 省略初始化和自增自减--进阶版，两个分号省略（while循环）
	//for 条件{}
	//a:=0
	//for a<10 {
	//	fmt.Println(a)
	//	a++
	//}
	//4 死循环
	//for true{
	//	fmt.Println("xx")
	//}
	//5 死循环之二（三部分都省略）
	//for {
	//	fmt.Println("xxx")
	//	break
	//}
	//6 continue和break

}
```

# 3 switch

```go
//switch:条件判断
package main

func main() {
	//1 基本使用
	//var a=10
	//switch a {
	//case 1:
	//	fmt.Println(1)
	//case 9:
	//	fmt.Println(9)
	//case 10:
	//	fmt.Println(10)
	//}
	//2 default
	//var a=15
	//switch a {
	//case 1:
	//	fmt.Println(1)
	//case 9:
	//	fmt.Println(9)
	//case 10:
	//	fmt.Println(10)
	//default:
	//	fmt.Println("不知道")
	//}
	//3 多表达式判断(只要符合其中的一个，就会执行响应代码)
	//var a=4
	//switch a {
	//case 1,2,3,4,5:
	//	fmt.Println(1)
	//case 7,8,9:
	//	fmt.Println(9)
	//case 10,11,12:
	//	fmt.Println(10)
	//default:
	//	fmt.Println("不知道")
	//}
	//4 无表达式
	//go当中（and）和的条件 &&    （or） 或者条件 ||
	//var a = 10
	//switch {
	//case a == 10 && a == 18:
	//	fmt.Println(10)
	//case a == 9:
	//	fmt.Println(9)
	//default:
	//	fmt.Println("不知道")
	//}
	//5 fallthrough:无条件执行下一个case（穿透）
	//var a=10
	//switch {
	//case a == 10:
	//	fmt.Println(10)
	//	fallthrough //一旦碰到 fallthrough会无条件执行下面
	//case a == 9:
	//	fmt.Println(9)
	//	fallthrough
	//default:
	//	fmt.Println("不知道")
	//}

}

```

# 4 数组

```python

//数组
package main

func main() {
	//同一类元素的集合，不允许混合不通元素
	//1 定义数组(定义了一个大小为4的int类型数组)
	//var a [4]int
	//fmt.Println(a)
	//补充：int的默认值是0
	//var b int
	//fmt.Println(b)

	//2 使用数组(从0开始的)
	//a[0]=10
	//fmt.Println(a[0])
	//fmt.Println(a)

	//3 定义并赋初值（三种）
	//var a [4]int=[4]int{1,2,3,4}
	//var a =[4]int{1,2,3,4}
	//a :=[4]int{1,2,3,4}
	//fmt.Println(a)

	//4  定义并赋初值（其他方式）
	//var a [4]int=[4]int{1,2}  //只给前两个赋值
	//var a [4]int=[4]int{2:3}  //只给第三个赋值(记住)
	//var a [4]int=[4]int{2:3,3:4}  	//给最后连个赋值
	//var a [4]int=[4]int{3:4,2:3}  	//给最后连个赋值
	//a[3]=100
	//fmt.Println(a)

	//5 数组大小固定（大小一旦定义，不能改变）
	//var a [4]int=[5]int{1,2}   //错的
	//var a [4]int=[4]int{1,2,3,4,5}   //错的

	//var a [4]int=[4]int{1,2,3,4}
	//a[4]=10  //错的
	//fmt.Println(a[4])   //错的

	//6 数组的大小是类型的一部分
	//下面两个不是同一种类型(不能比较和加减)
	//var a [3]int
	//var b [4]int
	//if a>b{
	//
	//}

	//7 数组是值类型（区别于引用类型）
	//不准确（因为py中一切皆对象，不可变数据类型也是对象，对象就是地址）
	//py中不可变数据类型(值类型)当做参数传递到函数中，如果在函数中改变该值，原值不会改变
	//py中可变数据类型（引用类型：地址）当做参数传递到函数中，如果在函数中改变该值，原值会改变

	//go中值类型，当参数传递时，copy一份传过去，值被copy是相当于复制了一份传过去了
	//var a = [4]int{1, 2, 3, 4}
	//test(a)
	//fmt.Println(a)


	//8 数组的长度(len)
	//var a = [4]int{1, 2, 3, 4}
	//fmt.Println(len(a))

	//9 循环数组（两种方法）
	//第一种
	//var a = [4]int{1, 2, 3, 4}
	//for i:=0;i<len(a) ;i++  {
	//	fmt.Println(a[i])
	//}
	//第二种（range:关键字，不是内置函数）把数组放在range关键字后
	//var a = [4]int{1, 88, 3, 4}
	////for z:=range a{
	////	//fmt.Println(z)
	////	fmt.Println(a[z]) //这个不是正统用法
	////}
	////当用一个变量来接收，它就是索引
	////当用两个变量来结束，第一个是索引，第二个是真正的值
	////for i,v:=range a{
	//for _,v:=range a{
	//	//fmt.Println(i)
	//	//fmt.Println("---------")
	//	fmt.Println(v)
	//}

	//多维数组
	//定义和使用
	//var a [4][3]int
	//fmt.Println(a)
	//a[0][1]=100
	//fmt.Println(a)
	//定义并赋初值
	//var a [4][3]int=[4][3]int{{1,1,1},{2,2,2},{3,3,3},{4,4,4}}
	//把数组的第二个位置写成[2,2,2]
	//var a [4][3]int=[4][3]int{1:{2,2,2}}
	//fmt.Println(a)

	//var a [4][3]string=[4][3]string{1:{"2","2","2"}}
	//fmt.Println(a)
	//字符串的空值是空字符串  ""
	//int类型空值是  0
	//数组的空值,取决于类型的空值
	//var a [4][3]string
	//fmt.Println(a)



}
//func test(a [4]int)  {
//	a[0]=100
//	fmt.Println(a)
//}
```











