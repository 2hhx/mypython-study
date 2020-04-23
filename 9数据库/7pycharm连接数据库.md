# pycharm连接数据库MySQL（pycharm充当数据库的客户端）

# pycharm中找数据库的三种方式

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021133255022-1346257289.png)

<h3>下载database插件</h3>

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021133540512-1725415823.png)

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021133632140-1930851986.png)



# 时区错误

Server returns invalid timezone. Go to 'Advanced' tab and set 'serverTimezone' property manually.

如果出现时区错误的话，有俩种不过方法

<h1>方法1</h1>

jdbc:mysql://localhost:3306?serverTimezone=GMT



在URL的后面添加?serverTimezone=GMT，但是比较麻烦，每次都要进行配置时区，建议采用方法二

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021134239129-1254907403.png)

------

<h1>方法2配置时区</h1>

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021141525376-1739718731.png)

# 连接数据库成功和会出现的问题

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021143025499-1763421936.png)



![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021143136316-1025027368.png)

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021140017798-1220565488.png)



# pycharm连接数据库sqlite

一共两步

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191028161417871-621638020.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191028161503246-1791382452.png)

