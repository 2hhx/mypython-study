# 0 可变长参数

```go
// 可以传切片，只不过需要这样传：     切片...

package main

import "fmt"



//可变长参数
func main() {


	//a:=find(23,12,34,45,56,67)
	//fmt.Println(a)
	//test3(1,2,3,4)
	a:=[]int{1,2,3,4}
	//test4(a)  //不可以
	test4(a...)  //相当于打散了,对数据类型有要求，也就是a必须是int类型切片才能打散了传过去


	//type Empty interface {
	//
	//}
	//var a []interface{}=[]interface{}{1,2,3}
	////var a []Empty=[]Empty{1,2,3}  //报错，不是同一种类型
	//fmt.Println(a...)


	//a只能是空接口类型的切片
	//fmt.Println(a...)
}

//func find(num int,nums ...int) bool {
//	b:=false
//	for _,v:=range nums{
//		if num==v{
//			fmt.Println("在里面")
//			b=true
//			break
//		}else {
//			b=false
//		}
//	}
//	return b
//}

//func test3(a ...int)  {
//	//a 是个切片类型
//	fmt.Println(a)
//	fmt.Println(len(a))
//	fmt.Println(cap(a))
//	//a是个数组？是个切片
//	//查看变量a的类型（切片类型）
//	fmt.Printf("%T",a)
//	//fmt.Print()   //不换行
//	//fmt.Println()  // 换行
//	//fmt.Printf("%T",1)


//直接传切片
func test4(a ... int)  {
	//a是切片，但是不能直接传切片
	//如果想直接传切片，可以吗？
	fmt.Println(a)

}
```









# 1 结构体

```go
package main

//结构体（面向对象的类）

//1 定义一个结构体,Person  有姓名，性别，年龄

//public
//private

//public
//private
//proctet
//属性大写和小写有什么区别？
//大写表示外部包可以使用
//type Person struct {
//	//name string
//	Name string
//	age  int
//	sex  int
//}

//8 匿名字段（字段没有名字，一种类型只能写一次）
//type Person struct {
//	 string
//	 int
//	 //int  报错
//}

//9 结构体嵌套
//写一个爱好结构体
//type Hobby struct {
//	id int
//	name string
//}
//type Person struct {
//	Name string
//	age  int
//	sex  int
//	hobby Hobby  //结构体嵌套，嵌套了一个Hobby结构体
//}

//10 提升字段（匿名字段）
//写一个爱好结构体
//type Hobby struct {
//	id int
//	hobbyName string
//}
type Hobby struct {
	id int
	Name string
}
type Person struct {
	Name string
	age  int
	sex  int
	Hobby  //字段没有名字
}
func main() {
	//2 实例化得到人对象  变量类型就是结构体的名字
	//var p Person=Person{}   //相当于调用__init__方法完成初始化
	//var p Person=Person{Name:"lqz",age:18,sex:1}   //按关键字初始化
	//var p Person=Person{"lqz",18,1}   //按位置初始化
	//有什么区别？按关键字，可以忽略位置，可以传一部分
	//var p Person=Person{sex:1,age:18,Name:"lqz"}
	//var p Person=Person{sex:1,age:18}
	//fmt.Println(p)

	//3 结构体类型的零值是：属性的空值   ，不是nil，它不是引用类型，当做参数传递，不会修改原来的
	//var p Person  //只定义，没有初始化
	//var p Person=Person{Name:"lqz",age:18,sex:1}
	//fmt.Println(p)
	//test5(p)
	//fmt.Println(p)

	//4 当参数传递，要修改原来的结构体
	//var p Person=Person{Name:"lqz",age:18,sex:1}
	//fmt.Println(p)
	//test6(&p)
	//fmt.Println(p)

	//5 结构体属性获取和修改
	//var p Person=Person{Name:"lqz",age:18,sex:1}
	//fmt.Println(p.Name)
	//p.age=20
	//fmt.Println(p)

	//6 匿名结构体(定义再函数内部，没有名字和type关键字)
	//一堆属性的集合  (其实就是面向对象的封装)
	//a := struct {
	//	Name string
	//	age  int
	//}{Name:"lqz",age:19}
	////var a int
	////var b  string
	////var c [3]int
	//fmt.Println(a.Name)

	//7 结构体指针
	//var p Person=Person{Name:"lqz",age:18,sex:1}
	//fmt.Println(p)
	//test6(&p)
	//var p *Person=&Person{Name:"lqz",age:18,sex:1}
	//p:=&Person{Name:"lqz",age:18,sex:1}
	//(*p).Name="xxx"  //正统用法
	//p.Name="yyy"   //推荐用法（别人的代码，这种多）
	//p:=&Person{Name:"lqz",age:18,sex:1}
	//test6(p)
	//var p Person=Person{Name:"lqz",age:18,sex:1}
	//fmt.Printf("%T",p)


	//8  匿名字段（字段没有名字）（暂时先不讲它有什么用，做变量提升，面向对象中的继承）
	//实例化，匿名字段名字就是类型名
	//var p Person=Person{string:"lqz",int:18} //按关键字,有点奇特（这就是为什么一种类型只能写一次）
	////var p Person=Person{"lqz",19} //按位置
	//fmt.Println(p)

	//9 结构体嵌套（结构体内套结构体）
	//p:=Person{Name:"lqz",age:19,sex:1,hobby:Hobby{id:1,name:"篮球"}}   //按关键字
	//p:=Person{"lqz",19,1,Hobby{id:1,name:"篮球"}}   //按位置
	//p:=Person{}
	//var p Person  //注意区分引用类型和值类型（数组，字符串，数字，只需要定义，不需要初始化就可以使用）
	////修改hobby的id
	//p.hobby.id=9
	//fmt.Println(p)

	//10 提升字段（匿名字段一起用）
	//p:=Person{Name:"lqz",age:19,sex:1,Hobby:Hobby{id:1,hobbyName:"篮球"}}  //Hobby是类型名
	////神奇的一模发生了
	//fmt.Println(p.hobbyName)  //在外层可以调用内层的属性，提升了，匿名的时候提升
	//p.hobbyName="xxx"
	//p.Hobby.hobbyName="yyy"  //以后这两种方式都可以了
	//
	//你去想面向对象的继承  Person继承了Hobby  self.id可以直接拿到Hobby的id
	//Hobby中字段跟Person中字段重名了怎么办
	//p:=&Person{Name:"lqz",age:19,sex:1,Hobby:Hobby{id:1,Name:"篮球"}}
	//fmt.Printf(p.Name)
	//fmt.Println(p.Hobby.Name)
	////p.Hobby=&Hobby{id:1,Name:"足球"}
	//var h Hobby
	//h=Hobby{id:1,Name:"足球"}
	//go的结构体嵌套就是面向对象的继承

	//11 结构体相等性
	//结构体内部所有字段都可以比较，结构体才可以比较，如果内部有不可比较的字段，结构体就不能比较
	//结构体内部所有字段都可以比较，结构体才可以比较
	//a:=Name{"liu","qz"}
	//b:=Name{"liu","qzddd"}
	//if a==b{
	//	fmt.Printf("xxx")
	//}


	//如果内部有不可比较的字段，结构体就不能比较
	//c:=AA{}
	//d:=AA{}
	//if c==d {
	//
	//}


	//int 和int比较  string和string   数组和数组比      引用类型不能比较：切片不能和切片比  map不能和map比
	//	//int和string能比么？不是同一个类型就不能比较
	//	//Myint 和int


}

type Name struct {
	firstName string
	lastName string
}
type AA struct {
	XX map[int]string
}

//func test5(p Person) {
//	p.Name = "egon"
//	fmt.Println(p)
//}
//func test6(p *Person) {
//	//p是个指针
//	//(*p).Name="egon"  //正统的用法，需要解引用
//	p.Name = "egon" //这种用法也可以，推荐这种用法，没有解引用，go自动处理
//	fmt.Println(p)
//}

```



# 2 方法

```go
package main

//方法（面向对象中的概念） ：函数和方法
//方法有特殊之处，绑定给对象的，在方法内部，可以修改对象


//1 定义一个方法（需要跟结构体搭配）
//type Person1 struct {
//	Name string
//	Age int
//}
////给结构体绑定方法Run方法
//
////def run(self)
////	print(self.name ,"在跑步")
////func (p Person1) Run()  {
////	fmt.Println(p.Name,"在跑步")
////}
////
////func (p Person1) speak()  {
////	fmt.Println(p.Name,"在说话")
////}


//4 值接收器和指针接收器
//type Person1 struct {
//	Name string
//	Age int
//}
//func (p Person1) changeNmae(name string)  {
//	p.Name=name
//	fmt.Println(p)
//}
//指针类型接收器
//func (p *Person1) changeNmae(name string)  {
////	//(*p).Name=name
////	p.Name=name
////	fmt.Println(p)
////}

//5 匿名字段方法

//type Hobby1 struct {
//	id int
//	hobbyname string
//}
//
//type Person1 struct {
//	name string
//	age int
//	Hobby1   //匿名字段
//}
//
//func (h Hobby1)printName()  {
//	fmt.Println(h.hobbyname)
//}
//
//
//func (p Person1)Speak()  {
//	fmt.Println(p.name,"在说话")
//}



//6 非结构体上的方法
//想给int 绑定一个add方法，以后每add一次，自加1(int,string,数组，都不能直接绑定方法)
//把int8重命名
type Myint int8
func (i *Myint)add()  {
	(*i)++  //自增1
}

func main() {
	//2 使用方法
	//p:=Person1{"lqz",19}
	//p.Run()
	//p.speak()
	////p.sex=1  //上节课同学问的

	//3  为什么有了函数，还要方法？用函数完全能实现方法的功能（包括py中，函数完全能实现方法的功能）
	//p:=Person1{"lqz",19}
	//p.Run()
	//Run(p)

	//4 值接收器和指针接收器
	//值类型接收器，不会修改原来的
	//p:=Person1{"lqz",19}
	//p.changeNmae("egon")
	//fmt.Println(p)  //这个打印出 {lqz,19} 名字并没有改

	//指针类型解收器,会修改原来的
	//p:=Person1{"lqz",19}
	//p.changeNmae("egon")
	//fmt.Println(p)  //这个打印出 {lqz,19} 名字并没有改

	//5 匿名字段方法（方法提升）
	//p :=Person1{"lqz",18,Hobby1{1,"篮球"}}

	//调用打印hobby名字的方法printName
	//p.Hobby1.printName()  //大家都想到
	//p.printName()   //直接点出来

	//面向对的继承Person1继承了Hobby1，对象.方法 可以直接用父类的方法


	//6 非结构体上的方法(需要重命名)
	//想给int 绑定一个add方法，以后每add一次，自加1
	//var a Myint=10
	//a.add()
	//a.add()
	//a.add()
	//fmt.Println(a)

	
}



//func Run(p Person1)  {
//	fmt.Println(p.Name,"在跑步")
//}
```



# 3 接口

```go
package main

import "fmt"

//接口  ：定义对象的行为
//结构体是一系列属性的结合
//接口是一系列方法的集合

//1 定义接口
//定义了一个鸭子接口
type DuckInterface interface {
	speak()
	run()
}
//定义一个糖老鸭结构体

type TLDuck struct {
	name string
	age int
	wife string
}
//让糖老鸭结构体实现DuckInterface（实现接口中所有的方法）
func (d TLDuck)speak()  {
	fmt.Println("说话")
}
func (d TLDuck)run()  {
fmt.Println("走路")
}

//java 侵入式接口
//go 非侵入式接口
```



# 补充

## 1 集群和微服务



## 2 提前补充

```go

//给类型重命名
//把int类型重命名为Myint
type Myint int

func main() {	
	var a Myint=10
	fmt.Println(a)
	var b int=100
	b=int(a)  //强制类型转换
	fmt.Println(b)
}


	type Empty interface {

	}
	var a []interface{}=[]interface{}{1,2,3}
	//var a []Empty=[]Empty{1,2,3}  //报错，不是同一种类型
	fmt.Println(a...)
```













