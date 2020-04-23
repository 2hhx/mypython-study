# Nginx实现负载均衡



# 1.准备工作。

结合本文场景，需要安装Nginx和Java环境（运行SpringBoot项目）。

**1.1 关于Linux系统安装Nginx可以参考我的文章---(传送门)。**

**1.2 由于只是测试，SpringBoot只是映射了根路径，端口分别是10001和10002，分别返回demo1和demo2字符串作为区分。**

# 2.Nginx负载均衡的集中方式介绍

## 2.1 轮询

轮询方式是Nginx负载默认的方式，顾名思义，所有请求都按照时间顺序分配到不同的服务上，如果服务Down掉，可以自动剔除，如下配置后轮训10001服务和10002服务。



```undefined
upstream  dalaoyang-server {
       server    localhost:10001;
       server    localhost:10002;
}
```

## 2.2 权重

指定每个服务的权重比例，weight和访问比率成正比，通常用于后端服务机器性能不统一，将性能好的分配权重高来发挥服务器最大性能，如下配置后10002服务的访问比率会是10001服务的二倍。



```undefined
upstream  dalaoyang-server {
       server    localhost:10001 weight=1;
       server    localhost:10002 weight=2;
}
```

## 2.3 iphash

每个请求都根据访问ip的hash结果分配，经过这样的处理，每个访客固定访问一个后端服务，如下配置（ip_hash可以和weight配合使用）。



```undefined
upstream  dalaoyang-server {
       ip_hash; 
       server    localhost:10001 weight=1;
       server    localhost:10002 weight=2;
}
```

## 2.4 最少连接

将请求分配到连接数最少的服务上。



```undefined
upstream  dalaoyang-server {
       least_conn;
       server    localhost:10001 weight=1;
       server    localhost:10002 weight=2;
}
```

## 2.5 fair

按后端服务器的响应时间来分配请求，响应时间短的优先分配。



```undefined
upstream  dalaoyang-server {
       server    localhost:10001 weight=1;
       server    localhost:10002 weight=2;
       fair;  
}
```

# 3.Nginx配置

以轮训为例，如下是nginx.conf完整代码。



```cpp
worker_processes  1;

events {
    worker_connections  1024;
}


http {
   upstream  dalaoyang-server {
       server    localhost:10001;
       server    localhost:10002;
   }

   server {
       listen       10000;
       server_name  localhost;

       location / {
        proxy_pass http://dalaoyang-server;
        proxy_redirect default;
      }

    }

}
```

# 4.测试

重启nginx，第一次访问[http://localhost:10000](https://links.jianshu.com/go?to=http%3A%2F%2Flocalhost%3A10000)如图所示，



![img](./)

image

在次访问如图所示



![img](./)

image

如果要修改负载均衡算法修改对应upstream模块即可。