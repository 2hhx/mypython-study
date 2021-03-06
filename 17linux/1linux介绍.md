# 一. Linux介绍

## 1 Linux基本常识

### 1.1 Linux诞生的故事

**Unix篇:**

为了进一步强化大型主机的功能，让主机的资源可以提供更多的使用者来利用，所以在1964年， 由AT&A公司的贝尔实验室(Bell)、麻省理工学院(MIT)及奇异公司(GE美国通用电气公司)共同发起了Multics（多路信息计算系统）的计划， Multics计划的目的是让大型主机可以同时支持300个以上的终端机连线使用。

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122923779-652796933.png)

贝尔实验室有个叫Ken Thompson的人也参与了这个项目,并在Multics操作系统上开发了一款叫做"星际旅行"的游戏.不过,由于Multics计划的工作进度太慢,资金也短缺.所以1969年,贝尔实验室退出了Multics计划.

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122334376-2100263197.jpg)

那年的某一天,他的妻子带着孩子要回娘家探亲一个月,Ken Thompson为了打发自己无聊的时光,同时也为了可以继续玩他的"星际旅行".于是乎,他就决定写一个操作系统来移植自己的游戏.

于此,Unix的雏形,UNICS就诞生了.只不过此时的UNICS是用汇编语言写的.移植到其它计算机上需要改很多源代码,很不方便.

于是,他又开发一门编程语言---B语言,用B语言重写了UNICS.可是,B语言写的UNICS移植起来,依旧需要改一部分源代码,他对此并不满足.

于是,又开发了一门编程语言---大名鼎鼎的C语言,并用C语言重写了UNICS.

后来,大家取其谐音,称其为Unix.



**历史过渡篇:**

起初Unix是免费开源的,所有人都可以获得其源码,各个大学也将Unix应用于操作系统的教学.

直到1970年起,AT&A公司意识到Unix的商业价值,他们开始用法律手段试图保护Unix,使得Unix的源码私有化,并且1979起不再允许大学使用其源码用于教学.

1983年的时候,有个叫斯托曼的黑客坐不住了,他认为软件应该是自由的,每个人都应该可以免费的使用,并且可以直接拿到源码,并对源码进行改进.因此他发起了GNU计划.

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122338301-156113284.jpg)

在1984年的时候,出现了一个叫做塔能鲍姆的大学教授,为了教学目的,基于AT&A公司的system V 开发出了一款叫做Minix的操作系统.

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122352852-1356380839.jpg)

1985年,斯托曼创立了自由软件基金会来为GNU计划提供技术/法律/财政支持.

到了1990年的时候,GNU计划已经开发出了很多优秀的软件,那时唯一没有完成的就是操作系统的内核.



**嘣!Linux出来了!**

1991年,出现了一个叫做林纳斯的大学在校生,他基于Minux开发出了Linux的第一个版本,并在GNU的自由软件(GPL)条款下发布.在1992年的时候,成功与其它GNU软件结合.

**在Linux内核上封装了众多应用软件的操作系统就叫做Linux发行版.**

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109142540202-865547973.jpg)





### 1.2 Linux和Unix的关系



![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122417423-527504578.png)







![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122407735-1398875784.png)







### 1.3 Linux的读音

你们生活中一定会听到对Linux的各个读法,主流的有:**哩呐咳死**,**哩你咳死**,**哩牛咳死**

那到底哪个读音是正确的呢?

答案是:**都正确**

为什么呢,因为大家都这么读,你不管怎么读大家都能理解.

这就好比<甄嬛传>,大众都读甄huan传,虽然后来有人整幺蛾子,说嬛不读huan,读xuān.有用吗?并没有什么卵用.



### 1.4 Linux的吉祥物tux

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122359263-2070851680.png)

传说,林纳斯这哥们儿写出Linux之后,想着要给Linux定一个吉祥物.但是......百思不得其解

有次他去动物园玩,看企鹅的时候,因为在想吉祥物的事情,所以分神了,再所以就被企鹅咬了.

于是,他就瞅着那只企鹅."咬我?好,就这么定了,吉祥物就是你了"

于是,Linux的吉祥物就是一只企鹅了.



### 1.5 Linux的主要发行版

我们上面说过,**在Linux内核上封装了众多应用软件的操作系统就叫做Linux发行版.**

那么,主要的发行版都有哪些呢?

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122415183-1550127475.png)

这里面,有一个点要提一下.那就是centos和redhat公司的关系.

起初,redhat公司发行了**redhat**系统,发展着发展着,演变出了两个分支,一个分支是交由社区来维护和跟新,这个版本就是我们用的最多的**Centos**.而另一个分支,就是依旧由redhat公司自己负责维护和更新的版本,**redhat**,这个版本上面有些服务是需要收费的.



### 1.6 Linux和Windows的比较

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122428873-503905334.png)



## 2.为啥要学Linux

### 2.1  Linux运维工程师

​	比如说,想阿里,腾讯这样的公司,服务器有几万台甚至几十万台,这么多服务器,要保证他们的正常运行,那肯定不是python程序员干的活,得有专门的人负责这个事,那就是Linux运维工程师,

​	他们得保证服务器可以7*24小时能够不间断正常提供服务,比如说,服务器优化,日常监控,数据安全,数据备份等等.

### 2.2  linux嵌入式开发工程师

​	举个简单的例子,就比如说,你的智能手表,家里的智能家居,他们之所以智能,只因为硬件之上有操作系统,操作系统之上有软件.那么它们的操作系统,大部分就是定制裁剪后的Linux,而为他们提供软件支持的,就是Linux嵌入式开发工程师.

### 2.3  项目维护及部署

​	对于程序员,我们为什么要学Linux呢?

​	这么说吧,我们写的项目最终都是要部署到Linux下的,所以我们学Linux主要是要学习,Linux的基本操作,文件管理,环境搭建以及项目部署.

​	当然,不会也行,正常可以交给运维去做.但这对你自己本身的个人竞争力会有一个比较大的影响.



## 3.怎么学Linux

1.不需要掌握所有的Linux指令,要学会使用百度

​	那么多命令呢,记得住吗?记不住.怎么办?查就完了.

2.Linux不是编程,重点是实操



