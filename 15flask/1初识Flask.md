## 1 什么是flask?



Flask 本是作者 Armin Ronacher在2010年4月1日的一个愚人节玩笑 ，不过后来大受欢迎，进而成为一个正式的python编写的web框架

Flask是一个Python编写的Web
微框架，让我们可以使用Python语言快速实现一个网站或Web服务，在介绍Flask之前首先来聊下它和Django的联系以及区别，django个大而全的web框架，它内置许多模块，flask是一个小而精的轻量级框架，Django功能大而全，Flask只包含基本的配置,
Django的一站式解决的思路，能让开发者不用在开发之前就在选择应用的基础设施上花费大量时间。Django有模板，表单，路由，基本的数据库管理等等内建功能。与之相反，Flask只是一个内核，默认依赖于2个外部库：
Jinja2 模板引擎和 WSGI工具集--Werkzeug ,
flask的使用特点是基本所有的工具使用都依赖于导入的形式去扩展，flask只保留了web开发的核心功能。

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009213820982-2905688.jpg)  

_WSGI（web服务器网关接口）是python中用来规定web服务器如何与python Web服务器如何与Python
Web程序进行沟通的标准，本质上就是一个socket服务端。而 Werkzeug模块 就是WSGI一个具体的实现_

**关键词** ：一个Python编写微web框架 一个核心两个库( Jinja2 模板引擎 和 WSGI工具集)

## 2 为什么要有flask?

flask性能上基本满足一般web开发的需求, 并且灵活性以及可扩展性上要优于其他web框架, 对各种数据库的契合度都非常高

**关键词** ：1. 性能基本满足需求 2 .灵活性可拓展性强 3. 对各种数据库的契合度都比较高。

 4.在真实的生产环境下，小项目开发快，大项目设计灵活



