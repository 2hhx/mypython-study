# MySQL部署

## 一 拉取mysql镜像

    
    
    docker pull centos/mysql-57-centos7

## 二 创建容器

    
    
    docker run -di --name=mysql -p 33306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql

-p 代表端口映射，格式为 宿主机映射端口:容器运行端口

-e 代表添加环境变量 MYSQL_ROOT_PASSWORD 是root用户的登陆密码

（3）远程登录mysql

连接宿主机的IP ,指定端口为33306

