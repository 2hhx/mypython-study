[TOC]





[我的码云：gitee](https://gitee.com/SkyOceanchen)

[我的github](https://github.com/SkyOceanchen/)

比较好的github上的项目

[知乎1https://www.zhihu.com/topic/19566035/hot](https://www.zhihu.com/topic/19566035/hot)

[知乎2https://www.zhihu.com/topic/19607465/hot](https://www.zhihu.com/topic/19607465/hot)

[经典的爬虫项目https://www.zhihu.com/question/58151047/answer/859783454](https://www.zhihu.com/question/58151047/answer/859783454)




![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191129164826447-1206597352.jpg)
![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191129164831642-40357993.png)
![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191129164835788-1425301669.jpg)



[更多github项目1](https://www.zhihu.com/topic/19566035/hot)

[更多git项目2](https://www.zhihu.com/topic/19607465/hot)

[经典的爬虫项目](https://www.zhihu.com/question/58151047/answer/859783454)

# 版本控制器

```python
"""
完成 协同开发 项目，帮助程序员整合代码

软件：SVN 、 GIT

git：集群化、多分支
"""
```



# git

## 简介

```python
"""
什么是git：版本控制器 - 控制的对象是开发的项目代码
代码开发时间轴：需求1 > 版本库1 > 需求2 > 版本库2 > 版本库1 > 版本库2 
"""
```

## git与svn比较

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128182619587-2092208997.png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128182540566-1878342796.png)

## git的工作流程

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128182636922-1919294134.png)

## git分支管理

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128182650736-1872684947.png)



# git使用

## 安装

```python
# 1.下载对应版本：https://git-scm.com/download
# 2.安装git：在选取安装路径的下一步选取 Use a TrueType font in all console windows 选项
```



# 基础命令

## 将已有的文件夹 - 初始化为git仓库

```python
"""
>: cd 目标文件夹内部
>: git init
"""
```

## 在指定目录下 - 初始化git仓库

```python
"""
>: cd 目标目录
>: git init 仓库名

"""
```

## 在仓库目录终端下 - 设置全局用户

```python
"""
>: git config --global user.name '用户名'
>: git config --global user.email '用户邮箱'

注：在全局文件 C:\Users\用户文件夹\.gitconfig新建用户信息，在所有仓库下都可以使用
"""
```

## 在仓库目录终端下 - 设置局部用户

```python
"""
>: git config user.name '用户名'
	-- 用户名
>: git config user.email '用户邮箱'
	-- 用户邮箱
	
注：在当前仓库下的config新建用户信息，只能在当前仓库下使用
注：一个仓库有局部用户，优先使用局部用户，没有配置再找全局用户
"""
```

## 查看仓库状态

```python
"""
# 当仓库中有文件增加、删除、修改，都可以在仓库状态中查看
>: git status  
	-- 查看仓库状态
>: git status -s  
	-- 查看仓库状态的简约显示
"""
```

## 工作区操作

```python
# 通过任何方式完成的文件删与改
# 空文件夹不会被git记录
```

## 撤销工作区操作：改、删

```python
"""
>: git checkout .
	-- 撤销所有暂存区的提交
>: git checkout 文件名
	-- 撤销某一文件的暂存区提交
"""
```

## 工作区内容提交到暂存区

```python
"""
>: git add .  
	-- 添加项目中所有文件
>: git add 文件名  
	-- 添加指定文件
"""
```

## 撤销暂存区提交：add的逆运算

```python
"""
>: git reset HEAD .
	-- 撤销所有暂存区的提交
>: git reset 文件名
	-- 撤销某一文件的暂存区提交
"""
```

## 提交暂存区内容到版本库

```python
# git commit -m "版本描述信息"
```

## 撤销版本库提交：commit的逆运算

```python
"""
回滚暂存区已经提交到版本库的操作：
    查看历史版本：
        >: git log
        >: git reflog
    查看时间点之前|之后的日志：
        >: git log --after 2018-6-1
        >: git log --before 2018-6-1
        >: git reflog --after 2018-6-1
        >: git reflog --before 2018-6-1
    查看指定开发者日志
        >: git log --author author_name
        >: git reflog --author author_name
    回滚到指定版本：
        回滚到上一个版本：
            >: git reset --hard HEAD^
            >: git reset --hard HEAD~
        回滚到上三个版本：
            >: git reset --hard HEAD^^^
            >: git reset --hard HEAD~3
        回滚到指定版本号的版本：
            >: git reset --hard 版本号
            >: eg: git reset --hard 35cb292
"""
```

# 过滤文件

```python
# .gitignore 文件
# 1）在仓库根目录下创建该文件
# 2）文件与文件夹均可以被过滤
# 3）文件过滤语法

""" 过滤文件内容
文件或文件夹名：代表所有目录下的同名文件或文件夹都被过滤
/文件或文件夹名：代表仓库根目录下的文件或文件夹被过滤

eg：
a.txt：项目中所有a.txt文件和文件夹都会被过滤
/a.txt：项目中只有根目录下a.txt文件和文件夹会被过滤
/b/a.txt：项目中只有根目录下的b文件夹下的a.txt文件和文件夹会被过滤
*x*：名字中有一个x的都会被过滤（*代表0~n个任意字符）
空文件夹不会被提交，空包会被提交
"""

```

# 提交后如何删除自己的文件

直接在自己的文件夹中进行删除，然后执行下面的命令

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128202733100-1178919151.png)

# 创建远程gitee仓库

## 选择线上仓库

```python
"""
1.注册码云账号并登录：https://gitee.com/
2.创建仓库
3.本地与服务器仓库建立连接
"""
"""
1）本地配置线上的账号与邮箱
>: git config --global user.name "doctor_owen"
>: git config --global user.email "doctor_owen@163.com"


这里是局部配置
>: git config user.name "doctor_owen"
>: git config user.email 

2）在本地初始化仓库(git init)，并完成项目的初步搭建(项目架构)(一般都是项目负责人完成项目启动)
# 这个过程就是git的基础部分的本地操作

3）采用 https协议 或 ssh协议 与远程git仓库通信提交提交代码(一般都是项目负责人完成)
	i) https协议方式，无需配置，但是每次提交都有验证管理员账号密码
	>: git remote add origin https://gitee.com/doctor_owen/luffy.git  # 配置远程源
	>: git push -u origin master  # 提交本地仓库到远程源
	
	ii) ssh协议，需要配置，配置完成之后就可以正常提交代码
	>: git remote add origin git@gitee.com:doctor_owen/luffy.git  # 配置远程源
	>: git push -u origin master  # 提交本地仓库到远程源
	
	iii）查看源及源链接信息
	>: git remote
	>: git remote -v
	
	iv）删除源链接
	>: git remote remove 源名字 
	
注：origin远程源的源名，可以自定义；master是分支名，是默认的主分支
"""
```

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128182709426-1341296052.png)





# 用本地仓库首次初始化远程仓库

## 本地仓库与远程仓库建立源连接

```
前提：本地仓库已经创建且初始化完毕（代码已经提交到本地版本库）

本机命令，添加远程源：git remote add origin ssh@*.git
	采用ssh协议的remote源
```

## 创建电脑的公钥私钥

[添加公钥教程：https://gitee.com/help/articles/4181#article-header0](https://gitee.com/help/articles/4181#article-header0)

```
官网：

本机命令，生成公钥：ssh-keygen -t rsa -C "*@*.com"
	邮箱可以任意填写
本机命令，查看公钥：cat ~/.ssh/id_rsa.pub

码云线上添加公钥：项目仓库 => 管理 => 部署公钥管理 => 添加公钥 => 添加个人公钥
```

## 全局公钥

在这里添加自己的公钥，然后在自己每一个项目都可以使用，并且不需要重复添加

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128202941140-842894680.png)





## 配置项目公钥

将自己的的公钥直接配置给一个项目，这样在这个项目中可以使用，

在别的项目中不能使用

如果想使用的话可以在别的项目下，进行激活添加，不能重复部署一个公钥，这样会出现冲突。

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128202926124-147116485.png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128202943017-731290066.png)

## 提交本地代码到远程仓库

```
在一次拉取和提交
拉取代码：git pull origin master
命令：git push origin master
```

# remote源操作

主要是记录仓库的状态

```python
"""
1）查看仓库已配置的远程源
>: git remote
>: git remote -v

2）查看remote命令帮助文档
>: git remote -h

3）删除远程源
>: git remote remove 源名
eg: git remote remove origin

4）添加远程源
>: git remote add 源名 源地址
>: git remote add orgin git@*.git
"""
```

# 多分支开发

## 分支操作

1. 在团队操作的时候一般的情况都是创建分支dev，proj
2. dev分支是开发的分支
3. proj是对客户展示的分支
4. 一般情况下我们都是在dev分支进行开发，在dev分支上进行拉取项目
5. 如果我们开发的时候需要自己在dev分支上创建新的分支，新的分支上会又dev分支上所有的内容，在我们新的分支下进行开发和测试，如果测试通过后，可以将分支与dev分支进行合并，然后进行删除分支操作
6. 随后将分支dev进行上传步骤
7. 如果上传无错误的情况下，由操作者，将master分支和dev分支进行合并。

```python
"""
1.创建分支
>: git branch 分支名

2.查看分支
>: git branch


3.切换分支
>: git checkout 分支名

4.创建并切换到分支
>: git checkout -b 分支名

5.删除分支
>: git branch -d 分支名
git branch -d develop
git branch -D develop (强制删除)

6.查看远程分支
>: git branch -a

7.合并分支:合并自己下面的冲突，需要在自己这个分支上，进行合并下一个分支，合并到自己上
>: git merge 分支名

8. 把 develop 分支推送到远程仓库**
git push origin develop
如果你远程的分支想取名叫 develop2 ，那执行以下代码：
git push origin develop:develop2
一般不使用
"""
```



## 线上分支合并

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191129170124668-2099797202.png)



![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128182748055-881586323.png)







![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191129170119854-1185390774.png)



# 团队开发和冲突解决

1. 首先自己要先配置好在自己的密钥， 然后将收到的邀请进行同意后，可以创建自己的文件，然后进行拉取线上文件
2. 或者给出自己的公钥，获取仓库地址
3. 每次提交前都要拉取，这样可以确定线上是不是被更新过，确定自己是最新的版本

```
1. git init
2. git pull 仓库
3. 修改文件，添加功能
4. git add .
5. git commit -m "提交信息"
6. git push 仓库
7. git push origin master
8. 如果成功，就上传成功
9. 如果不成功，可以从新拉取 进行比对
10. git pull origin master    这个是拉取命令
11. 重复操作，git push origin master 提交命令 
```



![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128202934384-677071396.png)



![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128202936625-1869053467.png)





![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128202943017-731290066.png)

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128202559695-1377297615.png)



## 解决冲突

1. 出现冲突后，大部分的解决方案是从新拉取，在文件中进行查看冲突的不部分

2. 然和和源码开发者进行商议，然后进行修改，确定修改的内容

3. 直接删除的方法不可取

4. 如果产生冲突，在提交自己的版本后，进行拉取的时候，可能会删除自己的文件，下面保存自己的文件

   ```
   git push origin master
   git log  查看版本
   git reset head HEAD^ 回归到上一个版本
   git reset head 版本号    去向某一个版本
   选择需要的文件,进行创建文件保存
   在切换版本，将文件添加进去，在进行提交
   ```

5. 产生冲突后:q退出，然后在文件中查看冲突的文件，并且进行修改（和提交代码的的人进行商议修改）

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191129142637470-1013466544.png)



# pycharm操作git

1. 首先要在项目中进行初始化

   ```
   git init
   ```

2. pycharm会自动提交

3. 也可以手动选择提交

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128225531489-886486150.png)
![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191128225535477-286597187.png)

# 资源与资料下载

[git更详细的教程](https://www.cnblogs.com/syp172654682/p/7689328.html)

- 权威Git书籍 [ ProGit（中文版）](http://git.oschina.net/progit/)
- git官网： [http://git-scm.com](http://git-scm.com/)
- git手册： [git手册](http://git-scm.com/docs)
- 网友整理的Git@osc教程，请 [点击这里](http://git.oschina.net/oschina/git-osc/wikis/帮助#继续阅读)；
- 一份很好的 Git 入门教程，请 [点击这里](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001373962845513aefd77a99f4145f0a2c7a7ca057e7570000)；
- [Git图解教程：http://www.cnblogs.com/yaozhongxiao/p/3811130.html)](http://www.cnblogs.com/yaozhongxiao/p/3811130.html)

资料链接: https://pan.baidu.com/s/1c20DVOW  密码: p9ri

[Git教程下载_王亮（大神）：https://pan.baidu.com/s/1qYlw2YS](https://pan.baidu.com/s/1qYlw2YS)

