## 一 什么是Dockerfile

Dockerfile是由一系列命令和参数构成的脚本，这些命令应用于基础镜像并最终创建一个新的镜像。

1、对于开发人员：可以为开发团队提供一个完全一致的开发环境；  
2、对于测试人员：可以直接拿开发时所构建的镜像或者通过Dockerfile文件构建一个新的镜像开始工作了；  
3、对于运维人员：在部署时，可以实现应用的无缝移植。

## 二常用命令

命令 | 作用  
---|---  
FROM image_name:tag | 定义了使用哪个基础镜像启动构建流程  
MAINTAINER user_name | 声明镜像的创建者  
ENV key value | 设置环境变量 (可以写多条)  
RUN command | 是Dockerfile的核心部分(可以写多条)  
ADD source_dir/file dest_dir/file | 将宿主机的文件复制到容器内，如果是一个压缩文件，将会在复制后自动解压  
COPY source_dir/file dest_dir/file | 和ADD相似，但是如果有压缩文件并不能解压  
WORKDIR path_dir | 设置工作目录  
  
## 三 使用脚本创建镜像

步骤：

（1）创建目录

    
    
    mkdir –p /usr/local/dockerdjango

（2）创建文件Dockerfile `vi Dockerfile`

    
    
    #依赖镜像名称和ID
    FROM python:3.6
    #指定镜像创建者信息
    MAINTAINER TEST
    #切换工作目录
    WORKDIR /usr
    RUN mkdir  /usr/local/mydocker
    RUN pip install==1.11.9
    ENV PATH $JAVA_HOME/bin:$PATH

（4）执行命令构建镜像

    
    
    docker build -t='django1.11.9' .

注意后边的空格和点，不要省略

（5）查看镜像是否建立完成

    
    
    docker images

