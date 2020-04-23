[TOC]



# Web API接口



# 什么是Web API接口

通过网络，规定了前后台信息交互规则的url链接，也就是前后台信息交互的**媒介**

Web API接口和一般的url链接还是有区别的，Web API接口简单概括有下面四大特点

- url：长得像返回数据的url链接

  - https://api.map.baidu.com/place/v2/search

- 请求方式：get、post、put、patch、delete

  - 采用get方式请求上方接口

- 请求参数：json或xml格式的key-value类型数据

  - ak：6E823f587c95f0148c19993539b99295
  - region：上海
  - query：肯德基
  - output：json

- 响应结果：json或xml格式的数据

  - 上方请求参数的output参数值决定了响应数据的格式

  - ```
    {
        "status":0,
        "message":"ok",
        "results":[
            {
                "name":"肯德基(罗餐厅)",
                "location":{
                    "lat":31.415354,
                    "lng":121.357339
                },
                "address":"月罗路2380号",
                "province":"上海市",
                "city":"上海市",
                "area":"宝山区",
                "street_id":"339ed41ae1d6dc320a5cb37c",
                "telephone":"(021)56761006",
                "detail":1,
                "uid":"339ed41ae1d6dc320a5cb37c"
            }
            ...
            ]
    }
    ```

# 接口文档的编写：YApi

YApi是去哪网大前端技术中心的一个开源可视化**接口管理平台**。

YApi项目可以搭建在任何本地或云服务器上，完成后台项目开发时的接口编写。为开发、测试等人员提供可视化的接口预览。

去哪同时在网上提供了YApi的[测试网站：http://yapi.demo.qunar.com/](http://yapi.demo.qunar.com/)，我们可以通过测试网站了解YApi是如何进行接口的编写的

- 访问测试网站

![img](https://img2018.cnblogs.com/blog/1825659/201911/1825659-20191115214207331-2052862208..png)

- 创建接口项目

![img](https://img2018.cnblogs.com/blog/1825659/201911/1825659-20191115214211361-41192532..png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118191138793-330833139.png)

- 创建接口

![img](https://img2018.cnblogs.com/blog/1825659/201911/1825659-20191115214214351-612262641..png)

- 编写接口

![img](https://img2018.cnblogs.com/blog/1825659/201911/1825659-20191115214215398-527664870..png)

![img](https://img2018.cnblogs.com/blog/1825659/201911/1825659-20191115214216761-1005429308..png)

![img](https://img2018.cnblogs.com/blog/1825659/201911/1825659-20191115214219053-716175199..png)

## 结果显示

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118191509592-614110220.png)
![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118191512061-1745545352.png)

## 测试

在网址上输入：https://api.map.baidu.com/place/v2/search

后面携带的参数一定要齐全

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118191055751-1123963156.png)
![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118191058316-70118444.png)



## 删除

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118191227087-1528119415.png)

# 接口测试工具：Postman

Postman是一款接口调试工具，是一款免费的可视化软件，同时支持各种操作系统平台，是测试接口的首选工具。

Postman可以直接从[官网：https://www.getpostman.com/downloads/](https://www.getpostman.com/downloads/)下载获得，然后进行傻瓜式安装。

一键安装，不能更改位置

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118195849223-271688847.png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118195852452-1100939677.png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118195854686-349219122.png)

- 工作面板

![img](https://img2018.cnblogs.com/blog/1825659/201911/1825659-20191115214221927-317456262..png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118195857137-1487294935.png)

## 简易的get请求

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118195858991-914419355.png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118195843231-2146226134.png)



## 简易的post请求

![img](https://img2018.cnblogs.com/blog/1825659/201911/1825659-20191115214229410-866523703..png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118195901315-1261096289.png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118195904555-2011588527.png)



- 案例：请求百度地图接口

![img](https://img2018.cnblogs.com/blog/1825659/201911/1825659-20191115214232002-1319748252..png)





![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118195906282-1714522774.png)