# Elasticsearch高级之-集群搭建，数据分片

## 一 广播方式

当es实例启动的时候，它发送了广播的ping请求到地址`224.2.2.4:54328`。而其他的es实例使用同样的集群名称响应了这个请求。

![img](https://img2018.cnblogs.com/blog/1168165/201903/1168165-20190320204049239-468915046.png)

一般这个默认的集群名称就是上面的`cluster_name`对应的`elasticsearch`。通常而言，广播是个很好地方式。想象一下，广播发现就像你大吼一声：别说话了，再说话我就发红包了！然后所有听见的纷纷响应你。
 但是，广播也有不好之处，过程不可控。

```python
#1 在本地单独的目录中，再复制一份elasticsearch文件
# 2 分别启动bin目录中的启动文件

```

## 二 单播方式

当节点的ip(想象一下我们的ip地址是不是一直在变)不经常变化的时候，或者es只连接特定的节点。单播发现是个很理想的模式。使用单播时，我们告诉es集群其他节点的ip及（可选的）端口及端口范围。我们在`elasticsearch.yml`配置文件中设置：

```
discovery.zen.ping.unicast.hosts: ["10.0.0.1", "10.0.0.3:9300", "10.0.0.6[9300-9400]"]
```

大家就像交换微信名片一样，相互传传就加群了.....

![img](https://img2018.cnblogs.com/blog/1168165/201903/1168165-20190320204528233-290861328.png)

一般的，我们没必要关闭单播发现，如果你需要广播发现的话，配置文件中的列表保持空白即可。

```python
#现在，我们为这个集群增加一些单播配置，打开各节点内的\config\elasticsearch.yml文件。每个节点的配置如下（原配置文件都被注释了，可以理解为空，我写好各节点的配置，直接粘贴进去，没有动注释的，出现问题了好恢复）：

#1 elasticsearch1节点，,集群名称是my_es1,集群端口是9300；节点名称是node1，监听本地9200端口，可以有权限成为主节点和读写磁盘（不写就是默认的）。

cluster.name: my_es1
node.name: node1
network.host: 127.0.0.1
http.port: 9200
transport.tcp.port: 9300
discovery.zen.ping.unicast.hosts: ["127.0.0.1:9300", "127.0.0.1:9302", "127.0.0.1:9303", "127.0.0.1:9304"]

# 2 elasticsearch2节点,集群名称是my_es1,集群端口是9302；节点名称是node2，监听本地9202端口，可以有权限成为主节点和读写磁盘。

cluster.name: my_es1
node.name: node2
network.host: 127.0.0.1
http.port: 9202
transport.tcp.port: 9302
node.master: true
node.data: true
discovery.zen.ping.unicast.hosts: ["127.0.0.1:9300", "127.0.0.1:9302", "127.0.0.1:9303", "127.0.0.1:9304"]

# 3 elasticsearch3节点，集群名称是my_es1,集群端口是9303；节点名称是node3，监听本地9203端口，可以有权限成为主节点和读写磁盘。

cluster.name: my_es1
node.name: node3
network.host: 127.0.0.1
http.port: 9203
transport.tcp.port: 9303
discovery.zen.ping.unicast.hosts: ["127.0.0.1:9300", "127.0.0.1:9302", "127.0.0.1:9303", "127.0.0.1:9304"]

# 4 elasticsearch4节点，集群名称是my_es1,集群端口是9304；节点名称是node4，监听本地9204端口，仅能读写磁盘而不能被选举为主节点。

cluster.name: my_es1
node.name: node4
network.host: 127.0.0.1
http.port: 9204
transport.tcp.port: 9304
node.master: false
node.data: true
discovery.zen.ping.unicast.hosts: ["127.0.0.1:9300", "127.0.0.1:9302", "127.0.0.1:9303", "127.0.0.1:9304"]

由上例的配置可以看到，各节点有一个共同的名字my_es1,但由于是本地环境，所以各节点的名字不能一致，我们分别启动它们，它们通过单播列表相互介绍，发现彼此，然后组成一个my_es1集群。谁是老大则是要看谁先启动了！

```

