# 如何使用git上传项目到github（简单的方法）

首先，你肯定得有一个github的账号，没有的可以先去注册；

然后你要使用git就需要安装git工具，没有的也先去安装吧；

下面开始上传步骤：

1. **进入Github首页，点击New repository新建一个项目**



![img](https://pic4.zhimg.com/80/v2-efca736f8584c5bae11e51829a95627f_hd.png)

2.**填写相应信息后点击create**

**Repository name: 仓库名称**

**Description(可选): 仓库描述介绍**

**Public, Private : 仓库权限（公开共享，私有或指定合作者）**

**Initialize this repository with a README: 添加一个readme.md**

**gitignore: 不需要进行版本管理的仓库类型，对应生成文件.gitignore**

**license: 证书类型，对应生成文件LICENSE**



![img](https://pic4.zhimg.com/80/v2-c044de650404925636e053ba801ed923_hd.png)



![img](https://pic4.zhimg.com/80/v2-cdf8430b9053a0852ab561a67b94d40f_hd.png)

4.**点击Clone or download，会出现一个地址，复制这个地址；**

![img](https://pic1.zhimg.com/80/v2-5fab05231f6898ab178461430feea2e4_hd.png)

5.**接下来就是本地操作了，首页右键你的项目，如果你之前安装git成功，会出现两个新选项，分别是Git Gui Here,Git Bash Here,在这里我们选择Git Base Hrer,进入如下界面，自定义滚动条就是我的项目名，因为不识别中文，所以都是问号，因此推荐文件命名时用英文命名。**



![img](https://pic4.zhimg.com/80/v2-bf34d582bf28336c6baf80cfd6652ad3_hd.png)

6.接下来输入如下代码，把github上面的仓库克隆到本地

git clone [https://github.com/](https://link.zhihu.com/?target=https%3A//github.com/)[HEternally/Custom-scroll-bar](https://link.zhihu.com/?target=https%3A//github.com/HEternally/Custom-scroll-bar.git).git （后面的地址替换成你之前复制的地址)



![img](https://pic2.zhimg.com/80/v2-24eff9313dce00a8d5f3fb57e96dc975_hd.png)

\7. **这个步骤以后你的本地项目文件夹下面就会多出个文件夹，该文件夹名即为你github上面的项目名，如图我多出了个Custom-scroll-bar文件夹，我们把本地项目文件夹下的所有文件（除了新多出的那个文件夹不用），其余都复制到那个新多出的文件夹下**



![img](https://pic2.zhimg.com/80/v2-83df39f10287fb37fa729df9a3cf24a1_hd.png)

8.**接着继续输入命令cd Custom-scroll-bar,进入Custom-scroll-bar文件夹**



![img](https://pic4.zhimg.com/80/v2-007c1ea03a7272773e2cb4298b3363b7_hd.png)

9.**接下来依次输入以下代码即可完成其他剩余操作：**

**git add . （注：别忘记后面的.，此操作是把Custom-scroll-bar文件夹下面的文件都添加进来）**

**git commit -m "提交信息" （注：“提交信息”里面换成你需要，如“first commit”）**

**git push -u origin master （注：此操作目的是把本地仓库push到github上面，此步骤第一次需要你输入帐号和密码）**



![img](https://pic1.zhimg.com/80/v2-d50eda1d3e339ea260597d9c6da13628_hd.png)

10.**到这里你已经把你的项目上传成功拉，去github上刷新下就可以看到了，如果觉得这篇文章对你有帮助，欢迎点赞，如果有什么新的建议，也欢迎评论。**