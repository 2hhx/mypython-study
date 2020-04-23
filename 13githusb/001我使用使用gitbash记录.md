[TOC]



svn     git

svn记录的是每一次版本变得的内容

git是将每个版本独立保存的 

git是通过三棵树来维持版本的控制的

![1571502997133](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/1571502997133.png)

工作区域：工作目录

暂存区：存放、add

git 仓库 commit







git@github.com:SkyOceanchen/SKY.git

## 1、简单配置

查看git版本号

git version

要检查已有的配置信息，可以使用 `git config --list` 命令



第一个要配置的是你个人的用户名称和电子邮件地址。这两条配置很重要，每次 Git 提交时都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录（如果没有配置，每次提交都要输入用户信息）:

```python
$ git config --global user.name "RandySun01“ # 自己的git名称

$ git config --global user.email “注册邮箱名称”

```

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。你也许会担心，如果有人故意冒充别人怎么办？这个不必担心，首先我们相信大家都是善良无知的群众，其次，真的有冒充的也是有办法可查的。

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

1. 查看当前的用户信息

```nginx
$ git config user.name
```

## 2、创建git仓库

什么是版本库呢？版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit

touch 文件
```

`pwd`命令用于显示当前目录。在我的Mac上，这个仓库位于`/Users/michael/learngit`。

 如果你使用Windows系统，为了避免遇到各种莫名其妙的问题，请确保目录名（包括父目录）不包含中文。

第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：

```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。

git add file  放在跟踪工作目录

### 你也可以看你这个文件的内容

`$ cat 文件名`查看文件内容



### 查看git当前的工作状态

git status 查看git当前的工作状态

On branch master  说明位于master分支里面
nothing to commit, working tree clean 当前的工作目录是干净的，没有改动。

### mit协议

最简单的协议，支持转载和一切的操作

```
Copyright (C) <year> <copyright holders>

　　Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
　　
　　The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```



没有git add 是没有被跟踪的状态

git reset WEAD 文件名      反悔不跟中，撤销git add 命令

git checkout -- file       覆盖，修改之前覆盖修改后的内容，慎用

每一次修改，用git add

### 查看提交记录

git log  提交记录

黄色的提交产生的id





git reset -- mixed HEAD~  快照回滚    现在回滚带暂存区域~后面加几次代表几次回滚

git rest --soft  HEAD~ 修改head指针，撤销错误的一次

git rest --head  HEAD~   会发生覆盖，回到工作目录，比较危险，慎用

快照，就是提交的次数的那一次

id 最少要写5个及以上

git reset  id 回滚带原来的id

打开新的





### git diff  比较暂存区域和工作目录

git diff  比较暂存区域和工作目录（提交后和修改后的未提交）,支持二进制

```
diff --git a/read.md b/read.md  拷贝到a  b  暂存区域和工作目录
index c3c78aa..ea88434 文件的id 100644 权限
--- a/read.md  存放在暂存文件
+++ b/read.md  工作
@@ -1,3 旧文件 开始行 结束行+1,4 新文件  @@  
+搭街坊拉萨的发      新文件特有的
 
 下面是文件内容，无意义
 copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
 　　
 　　The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.


```



比较俩个历史快照

git diff id1 id2

比较最新的和当前文件夹的

git diff HEAD

比较暂存区域和git仓库

git diff --cached  id





在提交的时可能会遇到的问题



git commit -- amend

i 插入，修改数据

esc shirt  保存

：q! 回车，退出不保存

## linux命令

f 一页页的下移动

b上移动

u上移动半页

d下移动半页

数字和G跳动第几行

/搜索从上往下，e下一个，E上一个

？搜索从下往上

h帮助文档

q退出

## 修改文件

git chackout -- file恢复文件（手动删除后，如何恢复）

get rm file  删除文件

git reset --soft HEAD 修正指针

get rm -f file 暴力删除,工作目录和暂存区域

git rm -- cached file  删除暂存区域

从命名

git mv oldfile  newfile





## 分支和分支管理

master默认分支，主分支

1. 创建分支
   1. git branch 分支名
2. git log --decorate 指向提交的引用和标签，常看指针指向那个分支（git log --decorate  --oneline只显示一行快照）
3. 切换分支
   1. git checkout 分支名
4. git log -- decorate --oneline --graph --all    精简显示，图形化显示，显示所有的数据,(顺序何以乱)，作用，检查分支的切换
5. 合并分支
   1. git  merge 分支名  （将这个分支合并到head指针现在指定的分支）
   2. 发生冲突的话，可能来个分支中相同文件内容不同，这个时候执行git status（查看状态）,然后可以进行修改
   3. 修改后进行add添加并且进行提交
   4. 合并
6. 删除分支
   1. git branch -d 分支名





多次提交的时候，head指向最后一个提交的内容

切换：git checkout HEAD~(上一个)，一个~代表回到一级

### checkot 功能

1.  从历史快照（或者暂存区域）中拷贝文件到工作目录中，如：git cheackout HEAD~，git cheackout file(覆盖工作目录)
2. 切换分支（主要功能）



## 使用github

### 目的

借用github托管项目代码



# git远程连接由http换成ssh

2019-04-19 09:43:19  更多



版权声明：本文为博主原创文章，遵循[ CC 4.0 BY-SA ](http://creativecommons.org/licenses/by-sa/4.0/)版权协议，转载请附上原文出处链接和本声明。本文链接：https://blog.csdn.net/u013983033/article/details/89393567

1、在github上建立了一个项目，发现push文件的时候，会出现如下错误
RPC failed; HTTP 413 curl 22 The requested URL returned error: 413 Request Entity Too Large
fatal: The remote end hung up unexpectedly
原因是使用了http方式 push
2、而且每次push的时候都需要输入密码很繁琐 所以使用ssh可以解决

具体过程：
在终端里边 输入 git remote -v

可以看到形如一下的返回结果
origin http://xxx/ios.git (fetch)
origin http://xxx/ios.git (push)

下面把它换成ssh方式

1. git remote rm origin
2. git remote add origin git@xxx/ios.git
3. git push origin xxx
4. ![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191122172449413-1867583046.png)

# 提交

```
git init

git add 文件

git commit -m ""

git remote add origin https://github.com/SkyOceanchen/nbbbs.git

git push origin master
```



git add 文件
方法一 git add 添加多个文件，文件之间以空格隔开

git add file1 file2 file3
1
方法二 多次git add

git add file1
git add file2
git add file2
1
2
3
方法三 添加指定目录下的文件
config目录下及子目录下所有文件，home目录下的所有.php文件

git config/*
git home/*.php
1
2
方法四 git add . 添加所有的文件， 或者 git add --all 添加所有的文件

git add .
git add --all
1
2
git add 文件夹
git add 文件夹名
1
git commit 提交到版本库
git add 目的是将修改文件由工作区提交到暂存区，可以多次提交
然后commit操作，将文件从暂存区提交到版本库

git commit -m "add new file"
















查看远端地址 `git remote –v` 
查看配置 `git config --list`

![这里写图片描述](https://img-blog.csdn.net/20160826180822476)

git status

![这里写图片描述](https://img-blog.csdn.net/20160826180836486)

```
 git add .  // 暂存所有的更改 git checkout . // 丢弃所有的更改 git status // 查看文件状态 git commit -m "本次要提交的概要信息" // 提交
```

- 1
- 2
- 3
- 4

![这里写图片描述](https://img-blog.csdn.net/20160826180923571)

设置远端仓库地址 `git remote set-url origin 你的远端地址` 
git push origin master出现以下情况：

![这里写图片描述](https://img-blog.csdn.net/20160826181031416)

解决办法：删除当前key，然后重新生成key，

![这里写图片描述](https://img-blog.csdn.net/20160826181103862)

会在本地C:\Users\你的用户名.ssh生成文件夹，里面有id_rsa和id_rsa.pub两个文件 
然后复制id_rsa.pub文件里面的内容，到https://github.com/settings/keys新建一个， 
![这里写图片描述](https://img-blog.csdn.net/20160826181118933) 
设置远程地址：（上面新建的） 
git remote add origin_new 新的地址 
git remote –v查看 
git push origin_new master重新推送 
下面是设置用户名 
Git config –global user.name “用户名” 
git config –global user.email 邮箱地址

设置代理： 
git config –global https.proxy [http://127.0.0.1:1080](http://127.0.0.1:1080/) 
取消设置代理： 
git config –global –unset https.proxy

取消git init操作时出现 rm: cannot remove ‘.git’: Is a directory 
是因为输入的命令是： rm -f .git 
解决办法：rm -rf .git 即删除整个.git目录

failed to push some refs to ‘git@github.com:*.git’ hint: Updates were rejected ··· 
使用git push origin master的时候出现一下错误：

![这里写图片描述](https://img-blog.csdn.net/20170316215739513?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaHVhaHVhNzg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

解决办法： 
git push -f origin master或者git pull下

![这里写图片描述](https://img-blog.csdn.net/20170316215805791?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaHVhaHVhNzg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

恢复不小心删除的 git stash 文件：

```
git fsck  //找到dangling的对象git show id  //上面列出的每一条记录的最后一个字符串，按 enter 查看具体信息git stash apply id
```

- 1
- 2
- 3

git 回滚提交

```
//reset将一个分支的末端指向另一个提交。这可以用来移除当前分支的一些提交, 让master分支向后回退了两个提交git checkout mastergit reset HEAD~2 //Revert撤销一个提交的同时会创建一个新的提交, 找出倒数第二个提交，然后创建一个新的提交来撤销这些更改，然后把这个提交加入项目中。git revert HEAD~2 
```

- 1
- 2
- 3
- 4
- 5
- 6

错误：Please enter a commit message to explain why this merge is necessary. 解决办法： 
\1. （可选）按键盘字母 i 进入insert模式 
\2. （可选）修改最上面那行黄色合并信息 
\3. 按键盘左上角”Esc” （退出insert模式） 
\4. 输入”:wq”,按回车键即可（提交）

gitignore notworking：

```
git rm -r --cached .git add .git commit -m "fixed untracked files"
```

- 1
- 2
- 3

git Failed to connect to www.google.com port 80: Timed out 可能是因为设置了代理：

```
git config --global http.proxy          //查看代理git config --global --unset http.proxy  //取消代理
```

- 1
- 2

HTTP Basic access denied on Git：

```
git config --global --unset credential.helpergit clone '···'login username，password
```

- 1
- 2
- 3

rebase 和 merge 区别

```
git pull --rebase origin master
```

- 1

rebase 选项告诉 Git 把你的提交移到同步了中央仓库修改后的 master 分支的顶部。rebase 操作过程是把本地提交一次一个地迁移到更新了的中央仓库master分支之上。这意味着可能要解决在迁移某个提交时出现的合并冲突，而不是解决包含了所有提交的大型合并时所出现的冲突。这样的方式让你尽可能保持每个提交的聚焦和项目历史的整洁。反过来，简化了哪里引入Bug的分析，如果有必要，回滚修改也可以做到对项目影响最小。

```
git pull origin master
```

- 1

如果没有 rebase， pull 操作仍然可以完成，但每次 pull 操作要同步中央仓库中别人修改时，提交历史会以一个多余的『合并提交』结尾。 
合并玩冲突之后，`git rebase --continue`，Git 会继续一个一个地合并后面的提交，如其它的提交有冲突就重复这个过程。 
如果你碰到了冲突，但发现搞不定，不要惊慌。只要执行下面这条命令，就可以回到你执行git pull –rebase命令前的样子：`git rebase --abort`



![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020205615297-430668900.png)



![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191027134301927-345258308.png)

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020205726855-1092900835.png)


![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191027134406620-852377150.png)











![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203131870-1157830565.png)

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203138799-374195665.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203142007-2319643.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203144317-2094867625.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203145966-1095338561.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203147815-892640319.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203149660-83205815.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203151474-466267511.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203153655-1848798418.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203155846-2119691469.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203158019-1478788515.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203200811-1753383268.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203203603-1698339346.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203205139-474317076.jpg)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203207251-1737618625.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203209883-1976889368.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191020203212125-262952525.png)