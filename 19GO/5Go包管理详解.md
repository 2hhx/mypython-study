# Go包管理

## 一 包管理历史

Golang 的包管理一直被大众所诟病的一个点，但是我们可以看到现在确实是在往好的方向进行发展。下面是官方的包管理工具的发展历史：

  * 在 1.5 版本之前，所有的依赖包都是存放在 GOPATH 下，没有版本控制。这个类似 Google 使用单一仓库来管理代码的方式。这种方式的最大的弊端就是无法实现包的多版本控制，比如项目 A 和项目 B 依赖于不同版本的 package，如果 package 没有做到完全的向前兼容，往往会导致一些问题。
  * 1.5 版本推出了 vendor 机制。所谓 vendor 机制，就是每个项目的根目录下可以有一个 vendor 目录，里面存放了该项目的依赖的 package。`go build` 的时候会先去 vendor 目录查找依赖，如果没有找到会再去 GOPATH 目录下查找。
  * 1.9 版本推出了实验性质的包管理工具 dep，这里把 dep 归结为 Golang 官方的包管理方式可能有一些不太准确。关于 dep 的争议颇多，比如为什么官方后来没有直接使用 dep 而是弄了一个新的 modules，具体细节这里不太方便展开。
  * 1.11 版本推出 modules 机制，简称 mod。modules 的原型其实是 vgo，关于 vgo，可以自行搜索。

除此之外，社区也一直在有几个活跃的包管理工具，使用广泛且具有代表性的主要有下面几个：

  * godep
  * glide
  * govendor

## 二 modules 的使用

### 2.1 准备

Golang 版本：1.12.3。在 1.12 版本之前，使用 Go modules 之前需要环境变量 GO111MODULE:

  * GO111MODULE=off: 不使用 modules 功能，查找vendor和GOPATH目录
  * GO111MODULE=on: 使用 modules 功能，不会去 GOPATH 下面查找依赖包。
  * GO111MODULE=auto: Golang 自己检测是不是使用 modules 功能，如果当前目录不在$GOPATH **并且** 当前目录（或者父目录）下有go.mod文件，则使用 GO111MODULE， 否则仍旧使用 GOPATH mode

    
    
    set GO111MODULE=on    //windows
    export GO111MODULE=on //linux

在 GOPATH 之外创建一个项目 mod-demo，包含一个 main.go 文件，内容如下:

    
    
    package main
    
    import "fmt"
    import "github.com/astaxie/beego"
    func main() {
        fmt.Println("hello")
        beego.Run()
    }

### 2.2 初始化go modules

在项目根目录执行命令 `go mod init test-demo` ，然后会生成一个 go.mod

![image-20191120210941001](https://tva1.sinaimg.cn/large/006y8mN6gy1g94t8ve4vqj30pu05gmy8.jpg)

这里比较关键的就是这个 go.mod 文件，这个文件中标识了我们的项目的依赖的 package 的版本。执行 init
暂时还没有将所有的依赖管理起来。我们需要将程序 run 起来（比如执行 go run/test），或者 build（执行命令 go
build）的时候，才会触发依赖的解析。

比如使用 go run 即可触发 modules 工作。

    
    
    go run test.go

这个时候我们再查看 go.mod 文件：

    
    
    module test-demo
    
    go 1.13
    
    require (
        github.com/astaxie/beego v1.12.0 // indirect
        github.com/shiena/ansicolor v0.0.0-20151119151921-a422bbe96644 // indirect
    )

**注意：有indirect注释的代表间接依赖，没有的代表直接依赖，这里是版本号+时间戳+hash**

**注意：在goland中可以用alt+enter/option+enter来下载依赖包**

同时我们发现项目目录下多了一个 go.sum 用来记录每个 package 的版本和哈希值。go.mod 文件正常情况会包含 module 和
require 模块，除此之外还可以包含 replace 和 exclude 模块。

这些 package 并不是直接存储到 GOPATH/src，而是存储到 GOPATH/pkg/mod 下面，不同版本并存的方式。

![image-20191120211440359](https://tva1.sinaimg.cn/large/006y8mN6gy1g94te0hhiwj30qg0fs77y.jpg)

### 2.3 依赖的升级和降级

可以使用如下命令来查看当前项目依赖的所有的包

    
    
    go list -m -u all
    //如果我想要升级（降级）某个 package 则只需要 go get 即可，比如：
    go get package@version

需要注意的是，在 modules 模式开启和关闭的情况下，`go get` 的使用方式不是完全相同的。在 modules 模式开启的情况下，可以通过在
package 后面添加 _@version_ 来表明要升级（降级）到某个版本。如果没有指明 version 的情况下，则默认先下载打了 tag 的
release 版本，比如 v0.4.5 或者 v1.2.3；如果没有 release 版本，则下载最新的 pre release 版本，比如
v0.0.1-pre1。如果还没有则下载最新的 commit。这个地方给我们的一个启示是如果我们不按规范的方式来命名我们的 package 的 tag，则
modules 是无法管理的。version 的格式为 `v(major).(minor).(patch)`
，更多信息可以参考：https://semver.org/ 。

比如我们现在想将我们依赖中的 beego 项目的版本改为 v1.11.1，则可以像如下操作。我们发现执行完 `go get` 之后， go.mod
中的项目的版本也相应改变了。

    
    
    go get github.com/astaxie/beego@v1.11.1

在 modules 开启的模式下，go get 还支持 version 模糊查询，比如 > v1.0.0 表示大于 v1.0.0  
的可使用版本；< v1.12.0 表示小于 v1.12.0 版本下最近可用的版本。version 的比较规则按照 version  
的各个字段来展开。

除了指定版本，我们还可以使用如下命名使用最近的可行的版本：

  * go get -u 使用最新的 minor 或者 patch 版本
  * go get -u=patch 使用最新的 patch 版本

### 2.4 vender

我们知道 Go 1.5 推出了 vendor 机制，go mod 也可以支持 vendor 机制，将依赖包拷贝到 vendor 目录。但是像一些 test
case 里面的依赖包并不会拷贝的 vendor 目录中。

    
    
    go mod vendor

## 三 modules 高级特性

### 3.1 代理服务器GoProxy

proxy 顾名思义，代理服务器。众所周知，有些 Golang 的 package 在国内是无法直接 go get
的。在之前，我们解决这个问题，一般都是通过设置 http_proxy/https_proxy 来解决。GoProxy 相当于官方提供了一种 proxy
的方式让用户来进行包下载。要使用 GoProxy 只需要设置环境变量 `GOPROXY` 即可。目前公开的 GOPROXY 有：

  * goproxy.io **在goland-- >Go-->Go Modules(ego)中配置这个地址**
  * goproxy.cn: 由七牛云提供，参考 github repo

![image-20191120231215167](https://tva1.sinaimg.cn/large/006y8mN6gy1g94wsfd7ybj31io08yq4i.jpg)

当然你也可以实现自己的 GoProxy 服务，比如项目中的依赖包含外部依赖和内部依赖的时候，那么只需要实现 module proxy protocal
协议即可。

值得注意的是，在最新 release 的 Go 1.13 版本中默认将 GOPROXY 设置为
https://proxy.golang.org，这个对于国内的开发者是无法直接使用的。所以如果升级了 Go 1.13 版本一定要把 GOPROXY
手动改掉。

### 3.2 replace

replace 主要为了解决某些包发生改名的问题。

对于另外一种场景有的时候也是有用的，比如对于有些 golang.org/x/ 下面的包由于某些原因在国内是下载不了的，但是对应的包在 github
上面是有一份拷贝的，这个时候我们就可以将 go.mod 中的包进行 replace 操作。

下面是一个 Beego 项目的 go.mod 的 replace 的示例。

    
    
    replace golang.org/x/crypto v0.0.0-20181127143415-eb0de9b17e85 => github.com/golang/crypto v0.0.0-20181127143415-eb0de9b17e85
    replace gopkg.in/yaml.v2 v2.2.1 => github.com/go-yaml/yaml v0.0.0-20180328195020-5420a8b6744d

### 3.3 SubCommand

modules 支持的 subcommand 如下

即输入`go mod`后面可以跟以下命令

  * download: 下载 modules 到本地缓存
  * edit: 提供一种命令行交互修改 go.mod 的方式
  * graph: 将 module 的依赖图在命令行打印出来，其实并不是很直观
  * init: 初始化 modules，会生成一个 go.mod 文件
  * tidy: 清理 go.mod 中的依赖，会添加缺失的依赖，同时移除没有用到的依赖
  * vendor: 将依赖包打包拷贝到项目的 vendor 目录下，值得注意的是并不会将 test code 中的依赖包打包到 vendor 中。这种设计在社区也引起过几次争论，但是并没有达成一致。
  * verify: verify 用来检测依赖包自下载之后是否被改动过。
  * why: 解释为什么 package 或者 module 是需要，但是看上去解释的理由并不是非常的直观。

## 四 Go 1.13 对 modules 的改动

上面在讨论 GoProxy 的时候提到了 Go 1.13 默认设置环境变量 GOPROXY 的值，除此之外 Go 1.13 对 modules
还有哪些值得注意的改动呢？

### 4.1 默认开启

modules 在 Go 1.13 的版本下是默认开启的。

### 4.2 GOPRIVATE

前面也说到对于一些内部的 package，GoProxy 并不能很好的处理，Go 1.13 推出了 GOPRIVATE
机制。只需要设置这个环境变量，然后标识出哪些 package 是 private 的，那么对于这个 package 的处理将不会从 proxy
下载。GOPRIVATE 的值是一个以逗号分隔的列表，支持正则（正则语法遵守 Golang 的 包 path.Match）。下面是一个 GOPRIVATE
的示例：

    
    
    GOPRIVATE=*.corp.example.com,rsc.io/private

上面的 GOPRIVATE 表示以 *.corp.example.com 或者 rsc.io/private 开头的 package 都是私有的。

### 4.3 GOSUMDB

GOSUMDB 的全称为 Go CheckSum Database，用来下载的包的安全性校验问题。包的安全性在使用 GoProxy
之后更容易出现，比如我们引用了一个不安全的 GoProxy 之后然后下载了一个不安全的包，这个时候就出现了安全性问题。对于这种情况，可以通过 GOSUMDB
来对包的哈希值进行校验。当然如果想要关闭哈希校验，可以将 GOSUMDB 设置为 off；如果要对部分包关闭哈希校验，则可以将包的前缀设置到环境变量中
GONOSUMDB 中，设置规则类似 GOPRIVATE。

关于 GOSUMDB 的配置格式为：++。

    
    
    GOSUMDB="sum.golang.org"GOSUMDB="sum.golang.org+<publickey>"GOSUMDB="sum.golang.org+<publickey> https://sum.golang.org"

上面三种配置都是合理的，因为对于 sum.golang.org，Go 自己知道其对应的 publickey 和
url，所以我们只要配置一个名字即可，对于另外一个 sum.golang.google.cn 也是一样。除此之外的，都需要指明 publickey，url
默认是 https://。

关于 GOSUMDB 更多的详细信息可以参考：https://golang.org/cmd/go/#hdr-
Module_authentication_failures

## 五 自己写的包如何导入使用

直接在go.mod中写包名即可

    
    
    module test-demo
    
    go 1.13
    
    require (
        github.com/astaxie/beego v1.12.0
        github.com/gin-gonic/gin v1.4.0 // indirect
        github.com/go-redis/redis v6.15.6+incompatible
        github.com/shiena/ansicolor v0.0.0-20151119151921-a422bbe96644 // indirect
        redisPackage v0.0.1  //自己加的v0.1.1 必须为三个数(也可以 不加)
    )
    

## 五 总结

Golang 包管理历经多个版本，目前来看并没有一个完全让开发者满意的方案，比如类似 Java 的 maven 的包管理方式。很多开发者也表示
modules 的管理方式也不是很直观。其实 Golang 的 package 使用 git
的管理方式来管理其实是一个很好的管理方式，我们最需要的其实如何能让普通开发者获取到任何 package
的心智负担降到最低。值得欣慰的是我们确实有看到大家在这个方向上的努力，比如 modules 的 GoProxy。

放眼未来，希望会有一种令大部分普通开发者满意的包管理方式吧。

## 最后常用命令

    
    
    go mod init:初始化modules
    go mod download:下载modules到本地cache
    go mod edit:编辑go.mod文件，选项有-json、-require和-exclude，可以使用帮助go help mod edit
    go mod graph:以文本模式打印模块需求图
    go mod tidy:检查，删除错误或者不使用的modules，下载没download的package
    go mod vendor:生成vendor目录
    go mod verify:验证依赖是否正确
    go mod why：查找依赖
    
    go test    执行一下，自动导包
    
    go list -m  主模块的打印路径
    go list -m -f={{.Dir}}  print主模块的根目录
    go list -m all  查看当前的依赖和版本信息

