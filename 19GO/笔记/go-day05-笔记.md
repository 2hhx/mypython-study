# 昨日回顾

## 1 可变长参数

```go
// 1 在函数定义时  func 函数名(形参名 ...类型)，可以传可变长的参数
		-在函数内部，被转成了切片，但是如果调用函数不能直接传切片
// 2 如果真的想传切片  函数名(切片变量...),切片的类型要跟函数要求的类型一致
```

## 2 结构体

```go
//1 定义  type关键字 结构体名字 struct{一系列属性的集合}，类似于面向对象的类，但是只有属性，没有方法
//2 使用 var 变量 结构体名字=结构体名字{初始化}，三种：不加参数，按关键字传参数（可以少传，可以打乱顺序），按位置传参数
//3 调用属性（取属性，赋值属性） 通过 .  来使用  p.name
//4 结构体中字段，公有字段，和私有字段（大小写）
//5 匿名结构体，结构体没有名字和type关键字（定义再函数内部，只使用一次时）
//6 匿名字段，字段没有名字（字段的名字就是类型名），如果全用匿名字段，一个结构体中，只能有一个相同的类型（字段提升）
//7 结构体嵌套（一个结构体中套一个结构体，类比：类的继承）
//8 如果嵌套的结构体中有匿名字段，字段会提升（类比：类的继承）
//9 结构体的0值，不是nil，他是一个值类型，在函数传递时，在函数内部，不会改变原结构体，要想改变，要传递指针
// 10 结构体指针  指向结构体的指针  var 变量名 *结构体名字 = &结构体名字{}
//11 结构体想等性：所有字段都可以比较，结构体就可以比较；有不能比较的字段，结构体就不能比较
```



## 3 方法

```go
//1 定义 绑定给结构体的func (p Person)speak(){},speak方法绑定给了Person结构体
//2 使用 p.speak()
//3 有了函数，为什么要有方法？它跟结构体绑定在一起，使用起来更方便
//4 值类型接收器和指针类型接收器func (p Person)speak(){}     func (p *Person)speak(){}  指针会改变原来的结构体，值的话不会改变原来的结构体
//5 匿名字段的方法（结构体嵌套了另一个结构体，另一个结构体如果是匿名的，并且绑定了方法），方法会提升（类比：面向对象的继承）p.另一个结构体的方法()     p.另一个结构体.另一个结构体的方法() 
//6 非结构体的方法  1 给类型重命名 type 自己的名字 类型   2 可以给自己命名的类型绑定方法
//7 在方法中使用值接收器和在函数中使用值参数
//8 在方法中使用指针接收器和在函数中使用值参数



```

### 补充

```go
package main

import "fmt"

//7 在方法中使用值接收器和在函数中使用值参数
//8 在方法中使用指针接收器和在函数中使用值参数

type Person struct {
	Name string
}

//func (p Person)speakName()  {
//	fmt.Println(p.Name)
//}

func (p *Person)speakName()  {
	fmt.Println(p.Name)
}

//func speakNmae(p Person)  {
//	fmt.Println(p.Name)
//}
func speakNmae(p *Person)  {
	fmt.Println(p.Name)
}

func main() {
	//p:=Person{"lqz"}
	//在方法中使用值接收器和在函数中使用值参数
	//p.speakName()
	//speakNmae(p)
	
	//在方法中使用指针接收器和在函数中使用指针参数
	//p:=Person{"lqz"}
	//p.speakName()     //直接支持用p结构体调用
	//(&p).speakName()   //正统的想法

	p:=&Person{"lqz"}
	//p.speakName()     //正统的用法
	//(*p).speakName()   //解引用以后再用
	
	speakNmae(p)   //只能用指针
	//speakNmae((*p)) //只能用指针,不能用值，类型不匹配
	
	
	//如果是方法，不管是指针接收器还是值接收器，都支持值来掉和指针来掉
	
}
```



# 今日内容

## 1 接口

```go
package main

import "fmt"

// 接口  规范结构体的行为（只要结构体实现了该接口，）
type DuckInterface interface {
	speak()
	run()
}
//定义一个TDuck（唐老鸭）结构体来实现该接口
type TDuck struct {
	name string
	age int
	wife string
}
//实现接口就是实现接口中所有方法
func (t TDuck)speak()  {
	fmt.Println(t.name,"在说话")
}
func (t TDuck)run()  {
	fmt.Println(t.name,"在走路")
}


//定义一个PDuck（普通鸭子）结构体来实现该接口
type PDuck struct {
	name string
	age int
}
//实现接口就是实现接口中所有方法
func (t PDuck)speak()  {
	fmt.Println(t.name,"在说话")
}
func (t PDuck)run()  {
	fmt.Println(t.name,"在走路")
}


//3 空接口
type Empty interface {

}

func main() {
	//1 实例化得到两个鸭子对象
	//var t TDuck=TDuck{name:"唐老鸭",age:2,wife:"刘亦菲"}
	//var p PDuck=PDuck{name:"普通鸭子",age:1}
	//
	////2 这两个鸭子的对象(因为它实现了该接口)，都可以赋值给接口类型
	//var i DuckInterface
	////i=t
	////fmt.Println(i)
	//i=p
	//fmt.Println(i)

	//3 空接口（没有方法，所有类型其实都实现了空接口，于是：任意的类型都可以赋值给空接口类型）
	//var a Empty=10
	//a="ssddd"
	//a=p
	//fmt.Println(a)

	//4 匿名空接口（空接口没有名字）
	//var a interface {}="dddd"
	//fmt.Println()

	//5 类型断言（接口类型传到函数中，它到底是PDuck还是TDuck，我需要断言回来）
	//var t TDuck=TDuck{name:"唐老鸭",age:2,wife:"刘亦菲"}
	//var p PDuck=PDuck{name:"普通鸭子",age:1}
	////test(t)
	//test(p)

	//6 类型选择
	var t TDuck=TDuck{name:"唐老鸭",age:2,wife:"刘亦菲"}
	var p PDuck=PDuck{name:"普通鸭子",age:1}
	var a=10
	var b="xxx"
	//test2(t)
	//test2(p)
	//test2(a)
	//test2(b)
	test3(t)
	test3(p)
	test3(a)
	test3(b)



}
//5 类型断言
func test(d DuckInterface)  {
	//暂时不知道他们的真正类型,接口类型只能调用接口的方法，不能调用原来结构体的属性和方法
	//fmt.Println(d.speak)
	//我可以断言你是什么类型，我断言你是TDuck类型   d.(TDuck) 返回两个值，一个是断言成功的TDuck类型的对象，一个是true和false表示是否断言成功
	//v,ok:=d.(TDuck)
	//if ok{
	//}
	//if v,ok:=d.(TDuck);ok{
	//	fmt.Println(v.wife)
	//	fmt.Println(v.age)
	//	v.speak()
	//}
}

//6 类型选择
func test2(a interface{})  {
	switch a.(type) {
	case int:
		fmt.Println(a)
	case string:
		fmt.Println(a)
	case TDuck:
		fmt.Println(a)
	case PDuck:
		fmt.Println(a)
	}
}
//6 类型选择其他
func test3(a interface{})  {
	switch v:=a.(type) {
	//注意区分类型断言,这种情况只能用一个值来接收
	//因为a是空接口类型，它不能调用任何属性和方法
	case int:
		fmt.Println(a)
		fmt.Println(v)
	case string:
		fmt.Println(a)
		fmt.Println(v)
	case TDuck:
		fmt.Println(a)
		fmt.Println(v.wife)
	case PDuck:
		fmt.Println(a)
		fmt.Println(v.name)
	}
}



//py中，是这样的
//len(列表)：列表.__len__()
//len(字典):字典.__len__()
```



```go
package main

import "fmt"

//接口使用其他

//1 接口的值接收器和指针接收器，等同于方法的值接收器和指针接收器（不论指针接收器还是值接收器，都可以用指针或者值来调用）


//2 实现多个接口
//写了俩接口
//type SpeakInterface interface {
//	speak()
//}
//type RunInterface interface {
//	run()
//}
////定义了一个狗结构体
//type Dog struct {
//	name string
//	age int
//}
////让狗实现run接口，实现speak接口
//func (d Dog)speak()  {
//	fmt.Println(d.name,"在说话")
//}
//func (d Dog)run()  {
//	fmt.Println(d.name,"在走路")
//}


//3 接口嵌套

//定义一个hobby接口，有打印id和打印name方法
type Hobby interface {
	printId()
	printName()
}
//再定义一个接口，嵌套Hobby接口
type TestInterface interface {
	Hobby  //只写接口名字（Hobby接口内的所有方法，就相当于在这写了一遍）
	printWife()  //这个接口相当于有三个方法
}
//定义一个结构体来实现上面的方法
type Person3 struct {
	name string
	age int
}
//实现接口
func (p Person3)printId()  {

}
func (p Person3)printName()  {

}
func (p Person3)printWife()  {

}

func main() {
	//2 实现读个接口
	//var d Dog=Dog{"egon",18}
	//var s SpeakInterface=d
	//var r RunInterface=d
	//s.speak()
	//r.run()

	//3 接口嵌套
	//var p Person3
	//var h Hobby=p
	//var t TestInterface=p
	//h.printName()
	//t.printWife()


	//4 接口零值,  是nil，接口是引用类型
	var p Person3
	//var t TestInterface  =&p   //本质  为了函数传递时，节省空间
	var t TestInterface  =p   //以后用的多的
	fmt.Println(t)

}

```



## 2 并发

```go
//什么是并发，什么是并行？
	-面试会被问（跑步的例子）

// 大家学go 都是奔着它大肆炫耀的高并发来的，go自动处理了这些事，开发者只需要简单的使用就可以

package main
//go协程   goroutine
import (
	"fmt"
	"time"
)


//这是一个普通函数
func testgo()  {
	time.Sleep(1*time.Second)
	fmt.Println("hello world")

}
func main() {
	//1 让普通函数testgo并发起来
	//在函数前加一个go关键字，不需要关注开的是线程还是协程，因为它内部自己处理，你只管用就行了
	go testgo()
	go testgo()
	go testgo()
	go testgo()
	go testgo()
	go testgo()
	go testgo()

	//跟py稍微有点区别
	time.Sleep(3*time.Second)


	//本质上：其他语言一般很少很少很少开进程，开线程就够了，其他语言的多线程就是多线程，可以利用多核优势，因为py有gil的限制，作者没办法了，给你搞了一个开进程
}

//1s支持10个请求，  10个人进来了查10个表，每个人占用10s钟，在接下来10s钟，只有这10个人在访问网站
//10个人进来了查10个表，每个人占用0.001s钟,接下来10s中，可以允许多少人访问，读写分离

//celery异步  请求来了----10s--处理完
//			请求来了---》直接返回了     起了一个异步任务去处理10s该做做
```



## 3 信道

```go

package main

import (
	"fmt"
	"time"
)

//不通goroutine之间通信，怎么做？

func main() {
	//1 信道的定义，就是一个变量（特殊）,两个协程之间通信，传递什么类型（int，bool）
	//定义了一个可以传递int类型的信道
	//var a chan int

	//2 信道的0值  <nil>  引用类型，当做参数传递，直接就是操作该信道
	//var a chan int
	//fmt.Println(a)

	//3 信道的初始化
	//var a chan int=make(chan int)
	//fmt.Println(a)  // 打印出来是个地址：0xc000016120

	//4 使用，不通协程之间传递数据
	var a chan int=make(chan int)

	go test5(a)   //test5执行多长时间，不清楚，睡多久不知道，这种方式有问题

	//把值从信道中取出
	//<-a  //取出来，没有赋值给一个变量
	//var i int = <-a  //取出来，赋值给一个变量i

	time.Sleep(3*time.Second)
	i := <-a  //取出来，赋值给一个变量i  i是int类型   重点：如果信道中没有值，会阻塞，等另一个goroutine放进值取，再继续往下走
	fmt.Println(i)


	//5 信道，默认都是阻塞的（重点）
	//本质，默认的信道，是放不进东西去，不管是放还是取，都阻塞

}

func test5(a chan int)  {
	fmt.Println("go go go ")
	//当它执行完成，往信道放一个值
	a<-1    //往信道中放值，因为它是管子，不能直接赋值方式放值,在这个位置，往里放值，也会阻塞
	fmt.Println("xxxx")


}

//6 例子，234   2的平方+3的平方+4的平方  +   2的立方+3的立方+4的立方
//计算平方和与立方和的和

```

```go
package main
//
//
////6 例子，234   2的平方+3的平方+4的平方  +   2的立方+3的立方+4的立方
////计算平方和与立方和的和
//import (
//	"fmt"
//)
//
//func calcSquares(number int, squareop chan int) {
//	sum := 0
//	for number != 0 {   //589   9的平方+8的平方+5的平方
//		digit := number % 10   //取余数 --9    8  5
//		sum += digit * digit
//		number /= 10     //除以10等于  58        5
//	}
//	squareop <- sum
//}
//
//func calcCubes(number int, cubeop chan int) {
//	sum := 0
//	for number != 0 {
//		digit := number % 10
//		sum += digit * digit * digit
//		number /= 10
//	}
//	cubeop <- sum
//}
//
//func main() {
//	number := 123
//	//定义了两个信道
//	sqrch := make(chan int)
//	cubech := make(chan int)
//	//起了两个协程，分别计算平方和和立方和
//	go calcSquares(number, sqrch)
//	go calcCubes(number, cubech)
//	//squares, cubes := <-sqrch, <-cubech
//	//从信道中取出值，赋给两个变量  cubes是什么类型？  int
//	cubes := <-sqrch
//	squares:=<-cubech
//
//
//	fmt.Println("Final output", squares + cubes)
//}

```

```go
package main

//有缓冲信道(无缓冲，取值，赋值都是阻塞)
func main() {

	//0 无缓冲信道演示

	//var a chan int=make(chan int)
	//a<-1   //一直阻塞在这，所以报死锁错误


	//1 定义有缓冲信道（信道里可以真正的放值，没放满之前，不阻塞）
	//定义了一个缓冲大小为3的信道
	//var a chan int=make(chan int,3)
	//a<-1   //放一个值
	//a<-2   //在放一个
	//a<-3   //在放一个
	////a<-4   //会怎么样？死锁
	//fmt.Println(<-a) //取出一个
	//fmt.Println(<-a) //取出一个
	////fmt.Println(<-a) //再取出一个，会死锁（会一直等待，等待死锁）
	//
	//2 无缓冲信道的本质是，缓冲大小为0 ，不是1：记住了
	//var a chan int=make(chan int,0)
	//a<-1

	//3 长度和容量
	//var a chan int=make(chan int,3)
	//a<-1
	//fmt.Println(len(a))  //1
	//fmt.Println(cap(a))  //3

	//4 单向信道
	//var a chan int=make(chan int)

}
```

```go
package main
//4 单向信道（只能读或只能写）
// 使用场景，传到另外的协程函数中，转成唯送信道（只能往里写），不允许在函数中读出值

import "fmt"

func sendData(sendch chan<- int) {   //接收一个唯送信道，只允许往里写
	sendch <- 10
	//<-sendch
}

func main() {
	sendch := make(chan int)   //双向信道，可以放值，也可以取值
	go sendData(sendch)
	fmt.Println(<-sendch)  //取值没问题
}

```

```go

//
//package main
//
////5 信道关闭 close(信道)
//
//import (
//"fmt"
//)
//
//func producer(chnl chan int) {
//	for i := 0; i < 10; i++ {
//		chnl <- i  //一写就卡主
//	}
//	close(chnl)
//}
//func main() {
//	ch := make(chan int)
//	go producer(ch)
//	for {  //死循环
//		v, ok := <-ch
//		if ok == false {
//			break
//		}
//		fmt.Println("Received ", v, ok)
//	}
//}

//6 通过for range循环信道,如果信道不关闭，可以一直取，一旦关闭，for循环结束
//
//package main
//
//import (
//	"fmt"
//)
//
//func producer(chnl chan int) {
//	for i := 0; i < 10; i++ {
//		chnl <- i
//	}
//	close(chnl)
//}
//func main() {
//	ch := make(chan int)
//	go producer(ch)
//	//通过for range 就不用判断false了，当信道关闭，循环自动结束（代码量少）
//	for v := range ch {
//		fmt.Println("Received ",v)
//	}
//}

//7 重写那个例子
package main

import (
	"fmt"
)

func digits(number int, dchnl chan int) {
	for number != 0 {
		digit := number % 10
		dchnl <- digit
		number /= 10
	}
	close(dchnl)
}
func calcSquares(number int, squareop chan int) {
	sum := 0
	dch := make(chan int)
	go digits(number, dch)
	for digit := range dch {
		sum += digit * digit
	}
	squareop <- sum
}

func calcCubes(number int, cubeop chan int) {
	sum := 0
	dch := make(chan int)
	go digits(number, dch)
	for digit := range dch {
		sum += digit * digit * digit
	}
	cubeop <- sum
}

func main() {
	number := 589
	sqrch := make(chan int)
	cubech := make(chan int)
	go calcSquares(number, sqrch)
	go calcCubes(number, cubech)
	squares, cubes := <-sqrch, <-cubech
	fmt.Println("Final output", squares+cubes)
}
```



## 4 异常处理



```go
package main

import "fmt"

//  异常处理 不是try except
//特殊  defer panic  recover   结合起来用
// defer 延迟调用，即便程序出了严重错误，也会执行
// panic 主动抛出异常 等同于py中的raise
//recover 恢复程序，继续执行（程序出了异常，它给我恢复）

//func main() {
//	defer fmt.Println("xxx")  //延迟调用,先注册，后调用
//
//	defer fmt.Println("ddd")  //延迟调用
//
//	fmt.Println("yyy")
//}

//func main() {
//	defer fmt.Println("lqz lqz lqz ")  //加了defer 表示程序执行完成后再调用defer注册的内容执行
//
//	fmt.Println("xxx")
//	fmt.Println("yyy")
//	panic("我错了") //抛出异常
//	fmt.Println("zzz")
//}

func main() {
	defer func() {
		if error := recover(); error != nil {
			fmt.Println(error)
		}
		fmt.Println("这是finally的内容，不管程序是否出错，都会执行")
	}()

	f1()
	f2()
	f3()

}

func f1() {
	fmt.Println("f1  f1")
}
func f2() {
	//f2有异常，捕获异常
	//py 这么写
	//print(f1 f1)
	//try：
	//	fmt.Println("f2  f2")
	//	raise("xxx")
	//	print(pppp)
	//excepet Exception as e：
	//	print(e)
	//
	//finally:
	//	执行代码
	//
	//print(f3 f3)

	//defer func() {
	//	if error := recover(); error != nil {
	//		fmt.Println(error)
	//	}
	//	fmt.Println("这是finally的内容，不管程序是否出错，都会执行")
	//}()
	fmt.Println("f2  f2")
	panic("xxx")
	fmt.Println("f2asdfasdfadsfdsa  f2")  //会不会打印

}
func f3() {
	fmt.Println("f3  f3")
}

//以后的异常处理就是 拿到你要捕获异常的前面
//defer func() {
//	if error := recover(); error != nil {
//		fmt.Println(error)  //如果有异常就执行
//	}
//	fmt.Println("这是finally的内容，不管程序是否出错，都会执行") //finally执行的东西
//}()

```



# 补充

## py和go函数传参

```python

#py 中，小整数池

x="sdafasdfasdfe44**3^^%$223wqwasdfasfdsafas"
y="sdafasdfasdfe44**3^^%$223wqwasdfasfdsafas"

ll=[1,2,3,4,5]
# print(x,'------',id(x))
# print(id(y))
print(ll,'------',id(ll))
ll.append(4)
print(ll,'------',id(ll))


def test(ll):
    print(id(ll))
    ll.append(4)
    print(ll, '----dddd--', id(ll))
# test(x,ll)
# print(x,'------',id(x))
test(ll)
print(ll,'------',id(ll))


# py中所有的都是引用，所有的都是地址，在函数传递时，都是copy地址传递，就不涉及特别耗费空间
# go中，分值和引用，如果传值，copy一份传过来，值可能很大10m，允许传地址或copy一份传递
```



