https://www.cnblogs.com/sandea/category/1245001.html

https://www.cnblogs.com/longren/p/11444126.html

# 命令实例

## docker的主要组成部分

docker是传统的CS架构分为docker client和docker server,向mysql一样

**docker的镜像管理**

```
查看镜像列表:
docker images
docker image ls

导出镜像:
docker image save centos > docker-centos6.9.tar.gz
导入镜像:
docker image load -i docker-centos6.9.tar.gz
删除镜像:
docker image rm centos:latest

docker image rm 578c3

搜索镜像 	docker search + 镜像名字

给源中镜像打标签:

docker tag nginx:latest 10.0.0.11:80/nginx:latest

推送指定镜像到docker镜像源服务器

docker push 10.0.0.11:80/nginx:latest

获取镜像 (下载)	docker pull image_name

官方pull	docker pull centos:6.8（没有指定版本，默认会下载最新版） 私有仓库pull	docker pull daocloud.io/huangzhichong/alpine-cn:latest

docker history image_name   显示一个镜像的历史

docker build -t <image-name> .   *(点一定不能去掉)#使用当前目录下的Dockerfile构建镜像
```

**docker的容器管理**

```
docker -v         #查看版本

docker info     #查看docker信息

运行容器

docker run --name 容器名 -d -p 3306:3306 mysql  docker 启动容器
docker run image_name
docker run -d -p 80:80 nginx:latest
run（创建并运行一个容器） 
-d 放在后台 
-p 端口映射 :docker的容器端口
-P 随机分配端口
-v 源地址(宿主机):目标地址(容器)
docker run -it --name centos6 centos:6.9 /bin/bash 
-it 分配交互式的终端 
--name 指定容器的名字 
/bin/sh覆盖容器的初始命令

docker run image_name  启动容器***

docker stop container_id  停止容器

docker kill container_name   杀死容器

docker ps (-a -l -q)    查看容器列表

docker container rm  'docker ps -a -q'   删除所有容器

docker rm -f  'docker ps -a -q`      #删除所有容器

docker ps -a  #查看容器列表

docker exec -it 77cd6bef4dc9 /bin/bash   #进容器

docker start/stop container-id||container-name 开启/停止 指定容器id或者容器名称的容器

docker run -d -p 80:80 -v /opt/xiaoniao:/usr/share/nginx/html nginx:latest

docker logs container-name/container-id     #查看容器日志

docker ps | grep ${CONTAINER_ID}    #查看容器状态

docker commit ID new_image_name     #镜像打包 (保存对容器的修改)
docker commit -m="提交的描述信息" -a="作者" 容器id  要创建的目标镜像名:[标签名]

docker inspect <id/container_name>   #查看容器内部详情细节

docker login #登录

Ctrl+P+Q     #退出而不关闭容器
```

# 1. 镜像仓库

1.1 `docker search [OPTIONS] TERM // 搜索镜像`

| 选项                  | 说明                   | 示例                                                         |
| --------------------- | ---------------------- | ------------------------------------------------------------ |
| `-f, --filter filter` | 根据条件筛选           | `--filter=is-automated=true // 只列出 automated build类型的镜像` `--filter=stars=10 // 列出收藏数不小于指定值的镜像` |
| `--limit int`         | 设置搜索结果的记录数量 |                                                              |
| `--no-trunc`          | 搜索结果完整显示       |                                                              |

1.2 `docker pull [OPTIONS] NAME[:TAG|@DIGEST] // 从镜像参数中拉取指定镜像`

| 选项                      | 说明                      | 示例 |
| ------------------------- | ------------------------- | ---- |
| `-a`                      | 拉取所有tagged镜像        |      |
| `--disable-content-trust` | 忽略镜像的校验,默认为true |      |

1.3 `docker push [OPTIONS] NAME[:TAG] // 上传镜像到仓库(要先登录仓库)`

| 选项                      | 说明         | 示例                                                         |
| ------------------------- | ------------ | ------------------------------------------------------------ |
| `--disable-content-trust` | 忽略镜像校验 | docker login docker tag local-image:tagname sandea/spark:tagname docker push sandea/spark:tagname |

1.4`docker login -u 用户名 -p 密码 //登录`

1.5 `docker logout // 登出`

# 2. 容器操作

2.1 `docker ps [OPTIONS] //列出容器`

| 选项         | 说明                              | 示例                      |
| ------------ | --------------------------------- | ------------------------- |
| `-a`         | 显示所有容器,默认只显示正在运行的 |                           |
| `-f`         | 过滤                              | `docker ps -f name=hello` |
| `-n 10`      | 显示最近创建的容器                |                           |
| `--no-trunc` | 显示全部描述                      |                           |
| `-q`         | 只显示简略ID                      |                           |
| `-s`         | 显示总的文件大小                  |                           |

2.2`docker inspect [OPTIONS] NAME|ID //获取容器或镜像的元数据`

| 选项                     | 说明                             | 示例 |
| ------------------------ | -------------------------------- | ---- |
| `-f filter`              | 筛选                             |      |
| `-s`                     | 如果是一个容器的话返回其文件大小 |      |
| `--type image/container` | 返回指定类型的JSON               |      |

2.3 `docker top CONTAINER // 查看指定容器中运行的进程`

2.4 `docker attach [OPTIONS] CONTAINER //进入正在运行的容器`

| 选项                  | 说明         | 示例                |
| --------------------- | ------------ | ------------------- |
| `-detach-keys string` |              |                     |
| `--no-stdin`          |              |                     |
| `--sig-proxy`         | `默认为true` | `--sig-proxy=false` |

2.5 `docker events [OPTIONS] //从服务器获取实时事件`

| 选项                | 说明                             | 示例                               |
| ------------------- | -------------------------------- | ---------------------------------- |
| `-f filter`         | 过滤                             |                                    |
| `--since timestamp` | `显示在指定时间之后发生的事件`   | `docker events --since=1467302400` |
| `--until timestamp` | `显示在指定时间之前所产生的事件` |                                    |

2.6 `docker logs [OPTIONS] CONTAINER //获取容器的日志`

| 选项                | 说明                 | 示例 |
| ------------------- | -------------------- | ---- |
| `--details`         | `显示详细日志`       |      |
| `-f`                | `日志实时输出`       |      |
| `--since timestamp` |                      |      |
| `--until timestamp` |                      |      |
| `--tail num`        | `输出最后多少行日志` |      |
| `-t`                | `显示日志时间`       |      |

2.7 `docker wait CONTAINER... //等待容器停止并输出其退出代码`

2.8 `docker export -o fileName.tar CONTAINER //将指定容器打包到tar文档中,可以指定文件路径`

2.9 `docker port CONTAINER [PORT] // 列出容器的端口映射(容器端口与主机端口对应关系)`

# 3.容器rootfs命令

3.1 `docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]] // 从一个容器创建一个新的镜像`

| 选项 | 说明                         | 示例 |
| ---- | ---------------------------- | ---- |
| `-a` | `镜像作者`                   |      |
| `-c` | `使用Dockerfile指令创建镜像` |      |
| `-m` | `提交时的说明文字`           |      |
| `-p` | `在提交时,暂停容器`          |      |

3.2 `docker cp // 容器与主机之间的文件复制`

|                                                   |                                                              |
| ------------------------------------------------- | ------------------------------------------------------------ |
| `docker cp [options] CONTAINER:SRC_PATH TAR_PATH` | `从容器中复制到主机`docker cp testtomcat：/usr/local/tomcat/webapps/test/js/test.js /opt |
| `docker cp [options] SRC_PATH CONTAINER:TAR_PATH` | `从主机复制到容器中`docker cp /opt/test.js testtomcat：/usr/local/tomcat/webapps/test/js |

| 选项 | 说明                                    | 示例 |
| ---- | --------------------------------------- | ---- |
| `-a` | `复制所有的gid/uid信息`                 |      |
| `-L` | `Always follow symbol link in SRC_PATH` | `??` |

3.3 `docker difff CONTAINER // 查看容器中被修改过的文件或目录`
`说明:C - Change, D - Delete, A - Add`

# 4. 容器生命周期管理

4.1 `docker start [options] container... //启动一个或多个容器`

| 选项                   | 说明                       | 示例 |
| ---------------------- | -------------------------- | ---- |
| `-a`                   | `启动后进入容器`           |      |
| `--detach-keys string` | ``                         |      |
| `-i`                   | `Attach container's STDIN` |      |

4.2 `docker stop [options] container... //停止一个或多个容器`

| 选项     | 说明               | 示例 |
| -------- | ------------------ | ---- |
| `-t int` | `多少秒后停止容器` |      |

4.3 `docker restart [options] container... //重启一个或多个容器`

| 选项     | 说明               | 示例 |
| -------- | ------------------ | ---- |
| `-t int` | `多少秒后重启容器` |      |

4.4 `docker kill [options] container... // 杀死一个或多个容器`

| 选项        | 说明                            | 示例 |
| ----------- | ------------------------------- | ---- |
| `-s string` | `给容器发送一个信号,默认为KILL` |      |

4.5 `docker rm [options] container... //删除一个或多个容器`

| 选项           | 说明                   | 示例 |
| -------------- | ---------------------- | ---- |
| `-f, -force`   | `强制移除容器`         |      |
| `-l, -link`    | `删除指定的连接`       |      |
| `-v, -volumes` | `删除容器及其挂载的卷` |      |

4.6 `docker pause container... // 暂停容器中所有的进程`

4.7 `docker unpause container... // 恢复容器中所有的进程`

4.8 `docker exec [options] container command [arg...] 在运行的容器中执行命令`

| 选项                | 说明                                      | 示例                                                     |
| ------------------- | ----------------------------------------- | -------------------------------------------------------- |
| `-d, --detach`      | `分离模式, 在后台运行`                    |                                                          |
| `--detach-keys str` | ``                                        |                                                          |
| `-e, --env list`    | `设置环境变量`                            |                                                          |
| `-i, --interactive` | `保持STDIN打开`                           | `与-t结合使用打开一个终端`                               |
| `-t, --tty`         | `分配一个伪终端`                          | `与-i结合使用打开一个终端`                               |
| `--privileged`      | `Give extended privileges to the command` |                                                          |
| `-u, --user str`    | `指定用户名或用户ID`                      |                                                          |
| `-w, --workdir str` | `在指定文件目录下执行相应的命令`          | `-w /home container ls`                                  |
| `-c`                | `执行docker容器命令`                      | `docker exec -it zeppelin bash -c 'echo $ZEPPELIN_HOME'` |

4.9 `docker run [optoins] image [command] [arg...] // 运行一个新容器中执行一个命令`

| 选项                             | 说明                                                         | 示例                                                 |
| -------------------------------- | ------------------------------------------------------------ | ---------------------------------------------------- |
| `--add-host list`                | `添加自定义的host-ip映射(host:ip)`                           |                                                      |
| `-a,` `--attach list`            | `将容器的stdin,stdout,stderr【标准输入，标准输出，错误输出】关联到本地shell中，在执行docker run时，将所有输入输出指定到本地shell中，若执行时携带此参数，可以指定将stdin,stdout,stderr的某一个或某几个关联到本地shell` |                                                      |
| `--blkio-weight uint16`          | `限制容器读写权重，当宿主机有1个以上容器时，可以设置容器的读写优先权，权重值在10～1000之间，0为关闭权重(默认)` |                                                      |
| `--blkio-weight-device list`     | `设置针对指定设备的权重，权重值在10～1000之间，且优先级高于blkio.weight` | `--blkio-weight-device "/dev/sda:100" ubuntu:latest` |
| `--cap-add list`                 | `增强linux能力，在docker容器内限制了大部分的linux能力，在之前，需要开启这些功能需要结合--privileged开启特权模式才能使用这些参数，考虑到安全性，可以通过该参数来开启指定的linux功能【默认开启的功能及全部定义详见docker runc】，若参数为all则默认开启所有linux能力` |                                                      |
| `--cap-drop list`                | `移除linux能力`                                              |                                                      |
| `--cgroup-parent str`            | `配置容器的控制组，继承该控制组的资源限制模式。`             |                                                      |
| `--cidfile str`                  | `创建一个容器，并将该容器的id输出到某一文件中，若该文件存在，则会返回一个错误` |                                                      |
| `--cpu-period int`               | `与参数--cpu-quota配合使用，用于设定cpu从新分配资源的时间周期,时间周期结束后，会对cpu进行重新分配` |                                                      |
| `--cpu-quota int`                | `与参数--cpu-period配合使用，用于设定该容器在资源分配周期内占用cpu的时间，若容器设定--cpu-quota=1000000 --cpu-period=500000，则该容器在这个时间周期内权重为50%，这两个参数主要是提升宿主机内某一容器的权重比，可以用来解决宿主机内若干容器的资源抢占导致重要容器cpu性能不足的场景。该模式应用于Linux 的CFS模式` |                                                      |
| `--cpu-rt-period int`            | `--cpu-period的微秒版`                                       |                                                      |
| `--cpu-rt-runtime int`           | `在一个cpu资源分配周期内，优先保证某容器的cpu使用的最大微秒数。例如，默认周期为 1000000 微秒（1秒），设置 --cpu-rt-runtime=950000 可确保使用实时调度程序的容器每 1000000 微秒可运行 950000 微秒，并保留至少 50000 微秒用于非实时任务` |                                                      |
| `-c, --cpu-shares int`           | `CPU份额(相对权重),默认为0`                                  |                                                      |
| `--cpus decimal`                 | `设置容器使用cpu的数量`                                      | `--cpus=".5" ubuntu:latest`                          |
| `--cpuset-cpus str`              | `设置容器允许在哪个cpu上执行该进程，譬如--cpuset-cpus="1,3"为指定在cpu 1 和cpu 3上执行，--cpuset-cpus="0-2"为指定在cpu0,cpu1,cpu2上执行` | `--cpuset-cpus="1,3"`                                |
| `--cpuset-mems str`              | `同参数--cpuset-cpus，但该参数是作用于NUMA 架构的 CPU`       |                                                      |
| `-d, --detach`                   | `后台运行容器并返回容器ID`                                   |                                                      |
| `--detach-keys str`              | `设置容器的键盘映射键位，在容器被链接到前台时，若宿主机的键盘键位与容器键位冲突，可以使用该指令对容器的键位进行重新映射` |                                                      |
| `--device list`                  | `向容器中添加主机设备`                                       |                                                      |
| `--device-cgroup-rule list`      | `将宿主机的设备添加到cgroup规则列表中`                       |                                                      |
| `--device-read-bps list`         | `限制设备的读取速率(每秒字节数)`                             |                                                      |
| `--device-read-iops list`        | `限制设备的读取速率(每秒IO操作次数)`                         |                                                      |
| `--device-write-bps list`        | `限制设备的写速率(每秒字节数)`                               |                                                      |
| `--device-write-iops list`       | `限制设备的写速率(每秒IO操作次数)`                           |                                                      |
| `--disable-content-trust`        | `忽略镜像的校验(默认为true)`                                 |                                                      |
| `--dns list`                     | `指定容器使用的DNS服务器,默认与主机一致`                     |                                                      |
| `--dns-option list`              | `设置DNS选项，同修改/etc/resolv.conf文件`                    |                                                      |
| `--dns-search list`              | `指定容器DNS搜索域名,默认与主机一致`                         |                                                      |
| `-entrypoint str`                | `覆盖映像默认的entrypoint`                                   |                                                      |
| `-e, --env list`                 | `给容器设置环境变量`                                         |                                                      |
| `--env-file list`                | `从指定文件读取环境变量`                                     |                                                      |
| `--expose list`                  | `开放一个或多个端口`                                         |                                                      |
| `--group-add list`               | `为容器添加用户组`                                           |                                                      |
| `--health-cmd str`               | `执行一个健康检查命令`                                       |                                                      |
| `--health-interval duration`     | `配合--health-cmd参数，设置健康检查的执行的间隔时间（ms /s / m / h）` |                                                      |
| `--health-retries int`           | `配合--health-cmd参数，设置健康检查命令失败重试的次数`       |                                                      |
| `--health-statr-period duration` | `配合--health-cmd参数，设置健康检查的启动时间（ms /s / m / h）` |                                                      |
| `--health-timout`                | `配合--health-cmd参数，设置健康检查命令超时时间（ms /s / m / h）` |                                                      |
| `-h, --hostname str`             | `指定容器的hostname`                                         |                                                      |
| `--init`                         | `在容器中新增一个守护进程，来预防该容器出现僵尸进程的可能性` |                                                      |
| `-i, --interactive`              | `以交互模式运行容器,常与-t同时使用`                          |                                                      |
| `--ip str`                       | `设置容器的IPv4地址`                                         |                                                      |
| `--ip6 str`                      | `设置容器的IPv6地址`                                         |                                                      |
| `--ipc str`                      | `使用IPC模式`                                                |                                                      |
| `--isolation str`                | `使用容器隔离, 该参数拥有三个值<br>(1)default 即与使用dockerd --exec-opt的参数默认效果相同<br>(2)process 使用linux内核命名空间进行隔离，该参数不支持windows环境。<br>（3）使用微软的Hyper-V虚拟技术进行隔离，该参数仅限windows环境` |                                                      |
| `--kernel-memory bytes`          | `限制该容器内核的内存使用`                                   |                                                      |
| `-l, --label list`               | `设置该容器的元数据`                                         |                                                      |
| `--label-file list`              | `通过本地文件导入元数据至该容器`                             |                                                      |
| `--link list`                    | `指定容器间的关联，使用其他容器的IP、env等信息`              |                                                      |
| `--link-local-ip list`           | `容器IPv4/IPv6链路本地地址`                                  |                                                      |
| `--log-driver str`               | `设置日志工具，用于动态收集日志`                             |                                                      |
| `--log-opt list`                 | `配合参数--log-driver使用，用于日志配置`                     |                                                      |
| `--mac-address str`              | `设置该容器mac地址`                                          |                                                      |
| `-m, --memory bytes`             | `设置容器使用的最大内存`                                     |                                                      |
| `--memory-reservation bytes`     | `软限制该容器的内存使用，当宿主机内存空闲时，该容器的内存使用可以一定比例超出限制，但当宿主机内存紧张时，会强制该容器内存使用限制在该参数之内` |                                                      |
| `--memory-swap bytes`            | `内存交换分区大小限制。配合参数--memory使用，且最小内存交换限制应该大于内存限制。该参数有4种情况:<br> (1)不设置--memory与该参数:则该容器默认可以用完宿舍机的所有内存和 宿主机 swap 分区。<br> (2)设置--memory 50MB 不设置--memory-swap（默认为0）:则--memory-swap值等于限制内存大小，即该容器能够申请的最大内存为100MB。<br> (3)设置--memory 50MB --memory-swap为-1:则该容器最大可以申请的内存为50MB+宿主机swap分区大小 <br> (4)设置--memory 50MB --memory-swap 100MB:则该容器可以申请的最大内存为100MB-50MB=50MB` |                                                      |
| `--memory-swappiness int`        | `用于调整虚拟内存的控制行为，为0～100之间的整数。在linux内存管理中，将内存中不活跃的页交换至硬盘中，以缓解内存紧张，该参数设置为0则认定该容器所有内存中的内容均不允许交换至硬盘，用以保障最大性能，若设置为100，则认为该容器所有内存中的数据均可以交换至硬盘。` |                                                      |
| `--mount mount`                  | `将文件系统挂载到容器`                                       |                                                      |
| `--name str`                     | `为容器指定一个名称`                                         |                                                      |
| `--network str`                  | `将容器连接到网络，支持bridge/host/none/container四种类型`   |                                                      |
| `--network-alias list`           | `设置该容器在网络上的别名`                                   |                                                      |
| `--no-healthcheck`               | `禁止一切健康检查行为`                                       |                                                      |
| `--oom-kill-disable`             | `设置是否禁止oom kill行为，若该容器因为需要大量请求内存，导致宿主机内存不足或触发到内存限制，导致杀死该容器进程，若设置该参数为true则会关闭这个检查` |                                                      |
| `--oom-score-adj int`            | `调整主机的OOM首选项（从-1000到1000）此处需要注意的是，非专业人士docker官方是不建议用户修改--oom-score-adj--oom-kill-disable这两个参数的` |                                                      |
| `--pid string`                   | `设置该容器的pid`                                            |                                                      |
| `--pids-limit int`               | `限制该容器所能创建的最大进程数。默认-1不限制`               |                                                      |
| `--privileged`                   | `在该容器上开启特权模式，让该容器拥有所有的linux能力`        |                                                      |
| `-p, --publish list`             | `将容器的端口映射到宿主机上`                                 | `docker run -p 8000:8000 ubuntu`                     |
| `--publish-all`                  | `将该容器的所有端口均随机映射至宿主机`                       |                                                      |
| `--read-only`                    | `设置该容器只读`                                             |                                                      |
| `--restart str`                  | `在退出该容器时重启该容器,默认为no`                          |                                                      |
| `--rm`                           | `当容器退出时自动删除它`                                     |                                                      |
| `--runtime str`                  | `指定该容器关联一个runtime的容器，在使用该参数时注意runtime specified必须在dockerd --add-runtime注册过` |                                                      |
| `--security-opt list`            | `设置安全属性，在windows上使用CredentialSpec模块来执行身份识别` |                                                      |
| `--shm-size bytes`               | `设置/dev/shm/目录的大小`                                    |                                                      |
| `--sig-proxy`                    | `代理进程所接收的所有字符,当指定--sig-proxy=false时，ctrl+c和ctrl+d 不会传递信号给docker进程而关闭容器,默认为true` |                                                      |
| `--stop-signal str`              | `停止带有信号的容器，在linux环境下输入kill -l,就可以看到所有信号名称，可以指定容器发出某种信号时停止该容器，譬如SIGKILL,默认为SIGTERM` |                                                      |
| `--stop-timeout int`             | `设置容器调用命令超时后自动退出。该参数可以设置容器在调用命令时导致超时后多少秒退出，0(默认)为永远不退出，该参数单位为秒` |                                                      |
| `--storage-opt list`             | `容器的存储设置,可以分别指定dm.basesize、dm.loopdatasize、dm.loopmetadatasize等项` | `--storage-opt dm.basesize=20G`                      |
| `--sysctl map`                   | `内核参数，对应修改容器中的/etc/sysctl.conf文件`             |                                                      |
| `--tmpfs list`                   | `指定挂载一个tmpfs目录，tmpfs是一种虚拟内存文件系统。`       |                                                      |
| `-t, -tty`                       | `打开一个伪终端,常与-i同时使用`                              |                                                      |
| `--ulimit ulimit`                | `设置容器的ulimit选项`                                       |                                                      |
| `-u, --user str`                 | `用户名或UID`                                                |                                                      |
| `--userns str`                   | ``                                                           |                                                      |
| `--uts str`                      | `使用uts命名空间`                                            |                                                      |
| `-v, --volume list`              | `在该容器下挂载卷`                                           |                                                      |
| `--volume-driver str`            | ``                                                           |                                                      |
| `--volumes-from list`            | ``                                                           |                                                      |
| `-w, --workdir str`              | `指定容器的工作目录`                                         |                                                      |

4.10 `docker create [optoins] image [command] [arg...] // 创建一个新容器`
**参数 同run**

# 5. 镜像管理

5.1 `docker images [options] [repository[:tag]] //列出镜像`

| 选项                      | 说明                   | 示例 |
| ------------------------- | ---------------------- | ---- |
| `-a <br>--all`            | `列出所有镜像`         |      |
| `--digests`               | `显示摘要信息(sha256)` |      |
| `-f <br> --filter filter` | `过滤`                 |      |
| `--format str`            | ``                     |      |
| `--no-trunc`              | ``                     |      |
| `-q <br>--quiet`          | `仅显示ID`             |      |

5.2 `docker rmi [options] image...//删除镜像`

| 选项              | 说明       | 示例 |
| ----------------- | ---------- | ---- |
| `-f <br> --force` | `强制删除` |      |
| `--no-prune`      | ``         |      |

5.4 `docker tag src_image[:tag] tar_image[:tag] // 创建某个镜像的副本`

5.5 `docker history [options] image //查看指定镜像的创建历史。`

| 选项              | 说明                         | 示例 |
| ----------------- | ---------------------------- | ---- |
| `--format str`    | ``                           |      |
| `-H <br> --human` | `以可读的形式打印日志和大小` |      |
| `--no-trunc`      | ``                           |      |
| `-q <br>--quiet`  | ``                           |      |

5.6 `docker save [options] image... //将指定镜像保存为tar归档文件`

| 选项                   | 说明               | 示例 |
| ---------------------- | ------------------ | ---- |
| `-o <br> --output str` | `输出至指定的文件` |      |

5.6 `docker import [options] file/url/- [repository[:tag]] //由tar文档生成镜像`

| 选项                     | 说明                       | 示例 |
| ------------------------ | -------------------------- | ---- |
| `-c ,<br> --change list` | `用dockerfile指令创建镜像` |      |
| `-m, <br> --message str` | `为创建的镜像设置描述信息` |      |

5.7 `docker build [options] path / url / - //使用dockerfile创建镜像`

| 选项                      | 说明                                            | 示例 |
| ------------------------- | ----------------------------------------------- | ---- |
| `--add-host list`         | ``                                              |      |
| `--build-arg list`        | `设置镜像创建时的变量`                          |      |
| `--cache-from str`        | ``                                              |      |
| `--cgroup-parent str`     | ``                                              |      |
| `--compress`              | `使用zip压缩构建上下文`                         |      |
| `--cpu-period int`        | `限制 CPU CFS周期`                              |      |
| `--cpu-quota int`         | `限制 CPU CFS配额`                              |      |
| `-c <br> --cpu-shars int` | `设置 cpu 使用权重`                             |      |
| `--cpuset-cpus str`       | `指定使用的CPU id`                              |      |
| `--cpuset-mems str`       | `指定使用的内存 id`                             |      |
| `--disable-content-trust` | `忽略校验，默认开启`                            |      |
| `-f <br> --file str`      | `指定要使用的Dockerfile路径`                    |      |
| `--force-rm`              | `设置镜像过程中删除中间容器`                    |      |
| `--iidfile str`           | `指定保存镜像id的文件`                          |      |
| `--isolation str`         | `使用容器隔离技术`                              |      |
| `--label list`            | `设置镜像使用的元数据`                          |      |
| `-m <br> --memory bytes`  | `设置内存最大值`                                |      |
| `--memory-swap bytes`     | `设置Swap的最大值为内存+swap，"-1"表示不限swap` |      |
| `--network str`           | `设置镜像网络模式`                              |      |
| `--no-cache`              | `创建镜像的过程不使用缓存`                      |      |
| `--pull`                  | `尝试去更新镜像的新版本`                        |      |
| `-q, <br> --quiet`        | `只输出镜像ID`                                  |      |
| `--rm`                    | `设置镜像成功后删除中间容器`                    |      |
| `--security-opt str`      | `安全设置`                                      |      |
| `--shm-size bytes`        | `设置/dev/shm的大小，默认值是64M`               |      |
| `-t, <br> --tag list`     | ``                                              |      |
| `--target str`            | ``                                              |      |
| `--ulimit ulimit`         | `Ulimit配置`                                    |      |

# 6. 其他命令

6.1 `docker info //显示docker系统信息`

6.2 `docker version // 显示docker相关的版本信息`