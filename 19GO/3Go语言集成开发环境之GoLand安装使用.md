## 一 GoLand集成开发环境下载

IDE 下载地址：<http://www.jetbrains.com/go/?fromMenu>

IDE安装就是平常的傻瓜式安装，这里就不多说了。

## 二 使用GoLand

1、 双击运行安装完的GoLand，选择创建项目，并关联GOROOT（会自动关联）

![image-20191023182905420](https://tva1.sinaimg.cn/large/006y8mN6gy1g88b9440slj316y0fgwgr.jpg)

2、在项目上点击右键，按图选择，如下，给文件命一个名字，如 test

![image-20191023183257598](https://tva1.sinaimg.cn/large/006y8mN6gy1g88bd6mrixj30za0aymzu.jpg)

3、在文件中写入如下代码：

![image-20191023183454863](https://tva1.sinaimg.cn/large/006y8mN6gy1g88bf5iapwj31d40bgjsu.jpg)

4、在文件上右键，选择 Run go build test.go，就可以看到控制台打印出hello world!啦

![image-20191023183528981](https://tva1.sinaimg.cn/large/006y8mN6gy1g88bfsxg3ij31kz0u0wm2.jpg)

![image-20191023183633187](https://tva1.sinaimg.cn/large/006y8mN6gy1g88bgv07xfj30yi0u076z.jpg)

5、也可以在Terminal窗口下输入go run test.go,就可以看到打印结果

![image-20191023183828420](https://tva1.sinaimg.cn/large/006y8mN6gy1g88bivhbn0j30u011qdjk.jpg)

6、或者输入go build test.go ,会在当前路径下生成一个可执行文件(test)，继续输入 ./test ，即可执行该可执行文件

