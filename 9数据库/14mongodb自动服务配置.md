1. mysql在windows安装好以后，往后都能自动启动服务，非常方便，以后直接连接就好。

   而mongodb需要手动创建服务。

   先在mongodb_data文件夹创建一个log文件夹

   [![Windows 如何运行MongoDB 如何配置MongoDB服务](https://imgsa.baidu.com/exp/w=500/sign=dfbe0c88fed3572c66e29cdcba126352/1b4c510fd9f9d72ab5364b7cd82a2834349bbb05.jpg)](http://jingyan.baidu.com/album/d5c4b52b906bafda560dc591.html?picindex=9)

2. 

   在安装目录中创建一个文件mongod.cfg

   内容如下

   

   systemLog:

   ​    destination: file

   ​    path: D:\mongodb_data\log\mongod.log

   storage:

   ​    dbPath: D:\mongodb_data\db

   [![Windows 如何运行MongoDB 如何配置MongoDB服务](https://imgsa.baidu.com/exp/w=500/sign=7d8343a4ecfe9925cb0c695004a95ee4/c83d70cf3bc79f3dbf76bf51b6a1cd11728b293f.jpg)](http://jingyan.baidu.com/album/d5c4b52b906bafda560dc591.html?picindex=10)

3. 

   **使用管理员权限**打开一个cmd，输入

   mongod --config "D:\MongoDB\mongod.cfg" --install

   执行

   [![Windows 如何运行MongoDB 如何配置MongoDB服务](https://imgsa.baidu.com/exp/w=500/sign=a310e142bc7eca80120539e7a1239712/a6efce1b9d16fdfabe50818fb88f8c5494ee7b96.jpg)](http://jingyan.baidu.com/album/d5c4b52b906bafda560dc591.html?picindex=11)

4. 

   打开系统的服务

   [![Windows 如何运行MongoDB 如何配置MongoDB服务](https://imgsa.baidu.com/exp/w=500/sign=d3066e89b03eb13544c7b7bb961ea8cb/d31b0ef41bd5ad6e8ec8268b8dcb39dbb6fd3c99.jpg)](http://jingyan.baidu.com/album/d5c4b52b906bafda560dc591.html?picindex=12)

5. 

   先关闭正在运行的mongodb服务端命令提示符，再开启mongodb服务

   [![Windows 如何运行MongoDB 如何配置MongoDB服务](https://imgsa.baidu.com/exp/w=500/sign=9c1cc48f9082d158bb8259b1b00a19d5/9345d688d43f8794e9d70a4fde1b0ef41bd53aa2.jpg)](http://jingyan.baidu.com/album/d5c4b52b906bafda560dc591.html?picindex=13)

6. 

   如果100错误，关闭正在运行的mongodb服务端命令提示符，并删除mongod.lock文件

   [![Windows 如何运行MongoDB 如何配置MongoDB服务](https://imgsa.baidu.com/exp/w=500/sign=e78e44ab8e8ba61edfeec82f713597cc/ac6eddc451da81cb6f60c1ac5e66d016082431c8.jpg)](http://jingyan.baidu.com/album/d5c4b52b906bafda560dc591.html?picindex=14)

   [![Windows 如何运行MongoDB 如何配置MongoDB服务](https://imgsa.baidu.com/exp/w=500/sign=b5e04da23687e9504217f36c2039531b/b8389b504fc2d56286d5928ceb1190ef77c66cf5.jpg)](http://jingyan.baidu.com/album/d5c4b52b906bafda560dc591.html?picindex=15)

7. 

   启动成功

   [![Windows 如何运行MongoDB 如何配置MongoDB服务](https://imgsa.baidu.com/exp/w=500/sign=106733c1fa03738dde4a0c22831ab073/622762d0f703918f098a71ab5d3d269759eec47c.jpg)](http://jingyan.baidu.com/album/d5c4b52b906bafda560dc591.html?picindex=16)

8. 

   在cmd中输入mongo，如下结果表明服务启动成功。

   服务设置为自动的话（默认），以后开机后就可以直接使用mongodb了。

   [![Windows 如何运行MongoDB 如何配置MongoDB服务](https://imgsa.baidu.com/exp/w=500/sign=7338e1737e0e0cf3a0f74efb3a47f23d/9213b07eca806538efbde8729bdda144ad348278.jpg)](http://jingyan.baidu.com/album/d5c4b52b906bafda560dc591.html?picindex=17)