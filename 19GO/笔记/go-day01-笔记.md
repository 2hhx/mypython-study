

# 1 go语言介绍

```python
1 2009年11月谷歌公司对外公开的一门语言（现在是2020年，10年多的时间），python：89年  java：90年，新语言，可以能会有个一些缺点，生态没有流行语言好（2013年，我第一听到）
2 web框架：beego（中国人写的，跟django比较像，orm框架），gin框架（没有orm框架，需要用第三方老外，几个学生写的），java：spring全家桶
3 Go是静态（动态/静态）强类型（不允许不同类型相加）语言，是区别于解析型语言的编译型语言
# 强类型
int a=10
a='dasdfa'  #不允许
# 弱类型
int a=10
a='3333'  #弱类型
# java、python、go
# java有个编译过程：把java代码编译成.class 字节码（jar包，war包） 中间态，这种文件可以直接运行在java虚拟机上（一处编码，到处运行，跨平台），要运行java程序，必须有java虚拟机（不通平台有不通平台的java虚拟机，至少几百兆内存）
jdk：java开发环境（包括jre和jvm）
jre：java运行环境（包括jvm）
jvm：java虚拟机（java程序运行在虚拟机上）
# go语言：编译型（跨平台编译，在windows平台可以编译出linux平台的可执行文件），运行go编译好的程序，机器什么都不需要安装，开发go的程序需要安装开发环境
# pyhton：解释型，不通平台安装解释器，不管是开发还是运行都需要装python解释器
4 跨平台的编译型语言
5 有垃圾回收的机制，支持面向对象和面向过程的编程模式（go不是纯粹面向对象语言，但是它可以完成面向对象能做的）
6 Go语言发展 最新的是 1.14==go mode
7 谁再用：谷歌，k8s    百度的消息系统，主流的互联网公司都在用，  饿了么，知乎  bilibi（php）
8 服务端开发，并发量高，微服务，自动化运维（部分）
9 go开发的项目：docker，k8s
10 go为什么火：1 docker和k8s的火  2 性能高，特别适合服务端开发  3 区块链（第一个开源的区块链项目，go写的，国家推区块链）
11 go在国内感觉更火一些，互联网公司在用，人才比较缺，起始薪资，稍微高一些
12 GOPATH的工作区包含bin，src，和pkg这三个：
	src——源码（包含第三方的和自己项目的）
	bin——编译生成的可执行程序
	pkg——编译时生成的对象文件
```



# 2 环境搭建（开发工具介绍）

```python
#下载：https://golang.google.cn/dl/，一路下一步
# 查看（window和mac不需要配置环境变量（已经配置好了））
# 打开命令窗口：go version
	go version
	go version go1.14 windows/amd64
  
# ide(golang,vscode),个人推荐用goland，因为你pycharm已经用的很6了，换成golang，无缝切换
	-vscode免费
  -goland安装，一路下一步，跟pycharm一毛一样
  -创建项目，新建go文件（傻子都会）
# goroot和gopath(正常安装完就有了，一般不用改)
在命令行下敲：go env  查看go的环境，配置相关
GOPATH="/Users/liuqingzheng/go" :代码存放路径（代码不能放在其它位置，必须放在gopath下的src文件夹,否则不能执行）gopath一开始是没有的，go env能见到，但是硬盘上没有这个路径，手动创建出来  
GOROOT="/usr/local/go"   :go开发环境的安装路径，就可以使用go内置的模块，包
windows下修改：
set GOPATH=C:\Users\liuqingzheng\go
set GOROOT=路径
  
  ##### 重点：代码一定要放在gopath的src路径下
```

# 3 hello world

```python
#1 代码执行（正常流程）执行go代码，先编译，再执
	# go build  xx.go  编译成可执行文件(windows 下是exe   )
	# ./xx 
#2 编码并执行
	#go run xx.go（没有可执行文件，开发阶段使用）
#3 代码执行（goland中） 
	# goland中右键--run
```

```go

//是单行注释（以后基本上所有的编程语言，// 表示单行注释）
//多行注释 java，js，go
/*
注释
注释
注释
*/
//0 go程序执行的入口，main包下的main函数  入口函数

//1  改成main  表示当前文件的包是main包，每一个go文件都必须写（第一行都写package ）
package main
//2 导入fmt模块，类比python中 包导入：import os   os.path
//goland 可以自动导入包
import "fmt"

//写个main 敲回车
//3  main函数  大括号表示函数体，类比python中缩进
func main() {
	//4 输出函数不是内置函数，所以需要导入fmt，使用
	//双引号
	fmt.Println("hello world")
	fmt.Println("my name is lqz")

}

```



# 4 变量定义和类型

## 定义

```go

//变量
package main

import "fmt"

func main() {
	//三种
	//第一种（全定义）
	//var关键字 变量名 变量类型 = 变量值
	//var a int=10
	//fmt.Println(a)
	//第二种（类型推导）类型可以省略
	//var a =10
	//fmt.Println(a)
	//第三（简略声明）冒号和等号是一起的
	//a    :=   10
	//fmt.Println(a)
	// 演变
	//变量要先定义再赋值
	//var a int   //定义（第一种方式）
	//	//a=10        //赋值
	//	//fmt.Println(a) //使用

	//var a  //这种定义变量，必须用第一种方式
	//a=10
	//fmt.Println(a)

	//声明多个变量
	//var a,b,c int=1,2,3
	//var a,b,c =1,2,3
	var a,b,c =1,"2",3
	//a,b,c :=1,2,3
	fmt.Println(a)
	fmt.Println(b)
	fmt.Println(c)


	/*注意：
		1 变量定义了，必须使用，否则报错
		2 变量不能重复定义
		3 变量要先定义再赋值
		4 可以声明多个变量(三种方式都可以用)
	 */

}

```

## 类型

```python

//变量类型
package main

import "fmt"

func main() {
	/*
	1 数字
		-int（整数，含正负）
			-int，int8，int16，int32，int64：占得大小不一样，
				-int在32位机器上是int32，64位机器上是int64
				-int8表示占一个字节（8个比特位）表示的范围是：正负2的7次方-1
				-int16表示占两个字节（16个比特位）表示的范围是：正负2的15次方-1
		-uint（正整数）
			-uint，uint8 uint16 uint32 uint64
				-uint在32位机器上是uint32，64位机器上是uint64
				-表示范围不一样uint8：2的8次方-1
		-float
			-float64和float32 ：表示小数点后范围不一样（，没有float）
		-complex64(复数：科学运算用得到，开发中，基本不用)了解
			-有实部和虚部
		-byte
			-uint8的别名
		-rune
			-int32的别名
	2 布尔(bool)
		-真：true
		-假：false
	3 字符串
		-双引号包裹的："xxx"
		-反引号包裹，可以换行，   没有单引号，三引号
	*/

	//var a int=10
	//var d float32=9.8
	//var b bool=false
	//var c bool=true
	//var s string="lqz"
	//`` 是python中的三引号，可以换行
//	var s string=`
//你好
//我好
//大家好
//`
	var s string="你好\n" +
		"我好" +
		"大家好"
	fmt.Println(s)

}
```



# 5 常量

```go
//常量
package main

import "fmt"

//程序运行期间不允许修改
//数据库，redis连接
//const关键字 常量名 =常量值   赋值一次，不允许修改
//const NAME  = "lqz"
//const NAME  string = "lqz"
const sex  = true 
func main() {
	//fmt.Println(NAME)
	//fmt.Println(sex)
	//sex=false
	
	fmt.Println(a)
}
```



# 6 函数

```go
//函数
package main

func main() {

	//test()
	//test(3,4,"xxx") //没有关键字参数一说,都按位置传，也没有默认参数
	//test(3,4,"xxx")

	//var a int =test(3,4)
	//var a  =test(3,4)
	//a  :=test(3,4)
	//fmt.Println(a)
	//返回两个值，就要用两个值来接受
	//a,b:=test(3,4)  //这种用的多
	//var a,b int=test(3,4)
	//var a,b =test(3,4)
	//fmt.Println(a,b)
	//如果有个值不想接收
	// _,a,_:=test(3,4)
	//fmt.Println(a)
	// py中 _ 是一个真正的变量，go中，就是没有
	//fmt.Println(_)

	test(1,2,3,4,5,5,6,7)
}
//func关键字  函数名(参数1 参数类型，参数2 参数类型)(){
//函数体的内容
//}
//函数使用1(没有先定义再使用这一说)
//func test()  {
//	fmt.Println("我是test")
//}
//函数使用2 带参数
//func test(a int,b int,c string) {
//	fmt.Println(a+b)
//	fmt.Println(c)
//}

//函数使用3
//func test(a ,b int,c string) {
//	fmt.Println(a+b)
//	fmt.Println(c)
//}

////函数使用 4 规定返回值类型
//func test(a,b int) int {
//	//返回a+b的结果
//	return a+b
//}
//函数使用 5 规定返回值类型,返回多个值
//func test(a,b int) (int,int,string) {
//	//返回a+b的结果
//	return a+b,a*b,"ok"
//}

//函数使用6 不定长参数
//func test(z ...int)  {
//	fmt.Println(z)
//
//}
```



# 7 包

```go

//包的使用
package main

import (
	"fmt"
	"go_day01/package_test"
)

func main() {
	fmt.Println("xxx")
	//go_package.Test()
	package_test.Test()


}

/*1 你写的所有包必须在gopath的src路径下
	包名一般是文件夹的名字
	在外部包可以直接调用， 包名.方法名（）
  2 包下的函数，如果大写字母开头，表示导出（可以给外部包使用），如果小写表示，只能给包内部使用
 */

//深入了解：你为什么配置goroot和gopath：如果不配goroot，内置包找不到，如果不配gopath，自己写的包找不到
```









