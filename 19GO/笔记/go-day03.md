# 1 切片

```go
package main

import "fmt"

//切片：切片不拥有任何数据，只是对现有数组的引用
//@@@@重点，切片是最最常用的数据结构，数组稍微少一些（py中的元组和列表的使用频率）

func main() {

	//1 定义切片之从基于一个数组
	//var a [9]int=[9]int{9,8,7,6,5,4,3,2,1}
	////b是切片[]int --- int类型的切片
	////var b []int=a[:]   //b包含了所有数组
	////var b []int=a[:3]   //加起始位置，加终止位置
	////var b []int=a[1:3]
	//var b []int=a[1:]
	//fmt.Println(b)

	//2 改变数组，会不会影响切片？数组改变或者切片改变，都会相互影响
	//var a [9]int=[9]int{9,8,7,6,5,4,3,2,1}
	//var b []int=a[1:]
	////a[1]=999
	//b[0]=888
	//fmt.Println(b)
	//fmt.Println(a)


	//3 切片的长度和容量len  cap 容量表示最多能装多少值
	//var a [9]int=[9]int{9,8,7,6,5,4,3,2,1}
	//前闭后开区间
	//var b []int=a[0:3]
	//fmt.Println(len(b))   //3
	//fmt.Println(cap(b))   //9

	//var b []int=a[1:3]
	//fmt.Println(len(b))   //2
	//fmt.Println(cap(b))   //8

	//var b []int=a[7:9]
	//fmt.Println(len(b))   //2
	//fmt.Println(cap(b))   //2
	//4 改变切片的其他操作，追加值:内置函数 append
	//var a [9]int=[9]int{9,8,7,6,5,4,3,2,1}
	//var b []int=a[1:3]
	//b=append(b,999)
	//b=append(b,888)
	////fmt.Println(a)
	////b的长度是多少：3，容量是多小：8
	////b的长度是多少：4，容量是多小：8
	//fmt.Println(len(b))
	//fmt.Println(cap(b))
	//fmt.Println(b)
	////数组变成什么样了
	//fmt.Println(a)
	//4 切片追到到了最后，超过了数组长度
	//var a [9]int=[9]int{9,8,7,6,5,4,3,2,1}
	//var b []int=a[6:8]
	//fmt.Println(b)
	//fmt.Println(len(b))
	//fmt.Println(cap(b))
	////追加一个
	//b=append(b,777)
	//fmt.Println(b)
	//fmt.Println(len(b))
	//fmt.Println(cap(b))
	////如果再追加一个会怎么样？它会重写定义一个数组，把原来的值copy到新的数组上，切片是对新数组的引用
	////新数组大小是多少？如何看新数组大小（切片的容量）：原切片容量的两倍
	//b=append(b,888)
	//fmt.Println(b)
	//fmt.Println(len(b))
	//fmt.Println(cap(b))
	//b=append(b,222)
	//b=append(b,333)
	////到头了
	//b=append(b,444)
	//fmt.Println(b)
	//fmt.Println(len(b))
	//fmt.Println(cap(b))
	//
	////原数组改变，会不会影响现在的切片
	//fmt.Println(a)
	//a[8]=111111
	//fmt.Println(a)
	////切片会变吗？不会变了，因为不引用原来的数组了
	//fmt.Println(b)
	////现在新的数组(切片引用的数组)，可不可以打印出来？不能赋值给一个变量，看不到了，在底层藏着



	//数组定义就必须指定长度，数组不能变长（一开始就定了，定了就不能改了）
	//这种定义方式，了解
	//var a [4]int=[...]int{1,2,3,4}
	//fmt.Println(a)
	//fmt.Println(len(a))
	//var b []int

	// 空切片，只定义。没有赋值
	//只定义，没有赋值，空值是什么？空切片：nil
	//var b []int
	//fmt.Println(b)
	//if b==nil{
	//	fmt.Println("我是空的")
	//}
	//b[0]=100   //空指针异常
	////nil[0]=100   //空指针异常
	//fmt.Println(b)
	//6 切片的另一种定义方式(中括号内加东西，都是数组,不加东西，是切片)
	//make(类型，长度，容量),底层也引用了数组，只是看不到
	//var b []int=make([]int,4,6)
	//传一个值表示，长度是3，容量也是3
	//var b []int=make([]int,3)
	//fmt.Println(b)
	//fmt.Println(len(b))
	//fmt.Println(cap(b))
	////第0个位置放1
	//b[0]=1
	////追加上1(会不会创建新数组？)
	//b=append(b,1)
	//fmt.Println(b)
	//fmt.Println(len(b))
	//fmt.Println(cap(b))

	//7 补充（了解，切片的数据结构）不论切片中存长度有多大，切片这个变量占得内存空间都是一样大的
	/*
	{
		point:地址
		length：切片长度
		cap：切片容量
	}
	 */
	//8 切片的函数传递(因为切片是引用类型，copy传递，改动原切片)

	//var b []int=make([]int,4,6)
	//fmt.Println(b)
	//test1(b)
	//fmt.Println(b)
	//9 切片定义的第三种方式(定义并初始化，类似于数组)
	//var b []int=[]int{1,2,3}
	//fmt.Println(b)
	//fmt.Println(len(b))
	//fmt.Println(cap(b)) 

	//10 多维切片（用的很少,了解）
	//var b=[][]string{{"a","b"},{"c","d"}}
	//var b [][]string=[][]string{{"a","b"},{"c","d"}}
	//fmt.Println(b)
	//fmt.Println(len(b))
	//fmt.Println(cap(b))
	////也是2
	//fmt.Println(cap(b[0]))

	//切片的copy
	//var a []int=make([]int,3,10000)
	////var b []int=make([]int,3,4)
	////var b []int=make([]int,2,4)  //如果b的长度是2 会怎么样
	//var b []int=make([]int,4,4)  //如果b的长度是4 会怎么样
	//a[0]=777
	//a[1]=888
	//a[2]=999
	//fmt.Println(a)
	//fmt.Println(b)
	////fmt.Println(a[3])  //错误的
	////把a的值copy到b中
	//copy(b,a)
	//fmt.Println(b)


	//切片一定依附于数组，可以无限扩容（py中的列表）



}
func test1(a []int)  {
	a[0]=999
	fmt.Println(a)
}
```



# 2 maps

```go

package main

import "fmt"

//maps：存储键值对（py：字典）
func main() {
	//1 maps的定义           字典/hash
	//map[key的类型]value的类型
	//map的空值是什么？nil
	//var a map[int]string
	//fmt.Println(a)
	//if a==nil{
	//	fmt.Println("我是空的")
	//}
	//a[1]="lqz"

	//2 定义并初始化方式一   make
	//make(类型)
	//var a map[int]string= make(map[int]string)
	//var a = make(map[int]string)
	//a := make(map[int]string)
	//fmt.Println(a)
	//if a==nil{
	//	fmt.Println("我是空的")
	//}else {
	//	fmt.Println("我不是空的")
	//}
	//a[1]="lqz"
	//fmt.Println(a[1])

	//3 定义并初始化方式二   直接赋值
	//var a map[int]string= map[int]string{1:"lqz",2:"egon"}
	//fmt.Println(a)
	////打印结果是这种形式：map[1:lqz 2:egon]

	//3 maps的取值
	//var a map[int]string= map[int]string{1:"lqz",2:"egon"}
	//fmt.Println(a[1])
	//fmt.Println(a[9])   //value值的空
	////value如果是个数字
	//var b map[int]int= map[int]int{1:1,2:2}
	//fmt.Println(b[1])
	//fmt.Println(b[9])   //value的空值  0

	//var b map[int]map[int][]int = map[int]map[int][]int{}   以后尽量不要这么玩

	//4 maps取值如果key不存在，结果是value的空值。问题来了，因为取出来的可能为0  ""  nil。。。，如何判断呢？
	//var a map[int]string= map[int]string{1:"lqz",2:"egon"}
	//var a map[int]int= map[int]int{1:1,2:2}
	//c:=a[9]
	//if c==""{
	//	fmt.Println("不存在")
	//}else c==0{
	//
	//}
	//神奇的使用,取值时，可以用两个变量来接收，第一个就是value值（可能为value值的空值），第二个是布尔，有就是true，没有就是false
	//c,ok:=a[9]
	//c,ok:=a[1]
	//fmt.Println(c)
	//fmt.Println(ok)

	//5 给map添加元素
	//var a map[int]string= map[int]string{1:"lqz",2:"egon"}
	////不存在的key（就是放进去）
	//a[9]="xxx"
	////存在的kye(替换)
	//a[9]="yyy"
	//fmt.Println(a)

	//6 删除map中元素
	//var a map[int]string= map[int]string{1:"lqz",2:"egon"}
	////内置函数delete
	//delete(a, 2)
	//fmt.Println(a)

	//7 map的长度 len()
	//var a map[int]string= map[int]string{1:"lqz",2:"egon"}
	//fmt.Println(len(a))

	//lqz的疑问
	//数字不需要写   写了没错，到底代表什么？底层,表示初始容量（不需要掌握，知道就可以了）
	//var a map[int]string= make(map[int]string,3)
	//fmt.Println(cap(a))   //没有
	////3 并不是map的长度
	//a[1]="xxx"
	//a[2]="yy"
	//a[3]="zz"
	//a[4]="ii"
	//fmt.Println(len(a))

	//8 maps的迭代，循环
	//var a map[int]string= map[int]string{0:"lqz",1:"egon"}
	////for i:=0;i<len(a);i++{  //这个不可以
	////	fmt.Println(a[i])
	////}
	////range 迭代
	//for k,v:=range a{
	//	fmt.Println(k)
	//	fmt.Println(v)
	//}
	// map中  go  map是无序的       py中从3.6以后字典有序，3.6之前无序，一旦做成有序，占得内存大了，



	//9 maps当参数传递,引用类型（地址），会改变原值
	//var a map[int]string= map[int]string{0:"lqz",1:"egon"}
	//a := map[int]string{0:"lqz",1:"egon"}
	//fmt.Println(a)
	//
	//test3(a)
	//fmt.Println(a)


	//10 想等性，不能直接比较
	//var a=map[int]string{1:"lqz"}
	//var b=map[int]string{1:"lqz"}
	////if a==b{
	////	//报错,不能比较
	////}
	//if a==nil{
	//
	//}
	//

}

func test3(a map[int]string)  {
	a[1]="oooo"
	fmt.Println(a)
}
```



# 指针

```go
// 存储变量内存地址的   变量
//什么类型的指针，就是什么类型前面加 *
//取一个变量的内存地址，用 &  取地址符号

package main

import "fmt"

//指针  ，c，c++有指针，go有指针，但是不完全一样（go的指针不支持运算） ，py和java，php都没有指针
//存储变量内存地址的   变量

// *  定义指针变量时，和解引用时使用
// &  只用在取地址的时候
func main() {
	//1 指针定义和初始化
	//a :=10
	////定义一个指针，存储a的内存地址
	////什么类型的指针，就是什么类型前面加 *
	////取一个变量的内存地址，用 &
	////指针类型的空值是  nil  -----》 引用类型的空值都是nil类型
	////var b *int =&a
	////var b  =&a
	//b  :=&a
	//fmt.Println(b)

	//2 解引用（反解）
	//a:=10
	//b:=&a
	//fmt.Println(b)
	////b这个指针对应的值打印出来
	//fmt.Println(*b)

	//3 骚操作(只要是变量就会有地址，只要有地址，就可以取地址 &)
	//a:=10
	////b:=&a
	////c:=&b
	////d:=&c
	//var b *int=&a
	//var c **int=&b
	//var d ***int=&c
	//fmt.Println(d)
	//fmt.Println(*d)
	//fmt.Println(**d)
	//fmt.Println(***d)

	//4 向函数传递指针参数
	// 通过函数传递，把a的值改为100
	//a:=10
	////b:=&a
	////test4(a)
	//test4(&a)
	//fmt.Println(a)

	//5 不要向函数传递数组的指针（取数组的地址），而要传递切片
	//通过函数传递，把原数组的值改变
	//var a =[5]int{9,8,7,6}
	////test5(&a)
	////fmt.Println(a)
	////test6(&a)  //这个不行
	//test6(a[:])
	//fmt.Println(a)

	//6 go 不允许指针运算(c中，经常内存溢出)
	//var a=[5]int{9,8,7,6}
	//b:=&a
	//fmt.Println(b)
	//fmt.Println((*b)[1])
	//c语言  b++指的是数组的第1个值（从0开始）
	//fmt.Println(b++)



	//a:= map[int]string{1:"lqz"}
	//var b *map[int]string=&a
	//fmt.Println((*b)[1])

	//7 数组指针和指针数组
	// 数组指针---》指向数组的指针
	//指针数组----》数组中放指针

	//var a=[5]int{9,8,7,6}
	//var b *[5]int=&a   //b是指向数组的指针
	//fmt.Println(b)
	//
	//x,y:=10,11
	//var c [4]*int=[4]*int{&x,&y}   //数组中放指针
	//fmt.Println(c)


}

//func test4(a int)  {
//	a=100
//	fmt.Println(a)
//}

//func test4(a *int)  {
//	(*a)=100
//	fmt.Println(*a)
//}

//func test5(a *[4]int)  {
//	(*a)[0]=999
//	fmt.Println(a)  //理应该是个地址但是人家给你打印成了 &[999 8 7 6] 表示指向这个数组的指针
//	fmt.Println(*a)
//}
func test6(a []int)  {
	a[0]=999
	fmt.Println(a)
}
```



# 结构体

```go
// 就是面向对象中的类（只能存放属性，不能存放方法），一系列属性的集合
//比如人结构体，有姓名，性别，年龄

package main

import "fmt"

//结构体（go版面向对象）,一系列属性的集合

//1 定义一个结构体（人类结构体，有姓名，性别，年龄）
//type关键字 结构体名字 struct{
//属性
//属性
//}
type Person struct {
	name string
	age int
	sex int
}
func main() {
	//2 结构体的使用
	//对比面向对象中的实例化
	//p:=Person{}
	//var p Person   //结构体对象是值类型
	//实例化并赋初值
	var p Person=Person{name:"lqz",age:18,sex:1}

	fmt.Println(p)

}

```





# go程序运行

```python
# 1 可以以xx.go文件的形式运行  go run xx.go
# 2 可以以包的形式执行（文件夹）go run 包名
```

# 编码规范

```go
// 1 go文件命名以下划线的形式（不建议用驼峰体）
// 2 变量的命名，建议用驼峰体
```



# 游戏

后端：java，python，c++，go

手机端：unity3d，cocos 2d  cocos 2dx
