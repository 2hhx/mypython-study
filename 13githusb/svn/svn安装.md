# SVN的安装和使用手册



官网下载址:https://www.visualsvn.com/visualsvn/download/tortoisesvn/

![img](https://img-blog.csdn.net/20180518145057267)

下载完成后是这样的 安装TortoiseSVN：

![img](https://img-blog.csdn.net/20180518145118802)

![img](https://img-blog.csdn.net/20180518145142147)

此处的安装地址建议不动，当然你也可以选择你要安装的地址



![img](https://img-blog.csdn.net/20180518145151624)



![img](https://img-blog.csdn.net/20180518145216586)



![img](https://img-blog.csdn.net/20180518145449554)

安装完成后在桌面点击右键查看



![img](https://img-blog.csdn.net/20180518145505398)

如果有标记的两个文件说明已经安装成功.

如果感觉英语看到有点困难的可以安装汉化TortoiseSVN：

下载语言包 ：

​    下载地址：http://tortoisesvn.net/downloads.html 

![img](https://img-blog.csdn.net/20180518145523478)

下载完后直接点下一步就OK了。



![img](https://img-blog.csdn.net/20180518145535654)



![img](https://img-blog.csdn.net/20180518145552514)

选择中文确定就ok了。



使用说明

### 检出项目

假如项目已经在服务器的仓库里，那么现在你要做的就是把它检出到本地。 
首先创建一个空文件夹。在空文件夹内右键，选择SVN检出。



![img](https://img-blog.csdn.net/20180518145609346)

现在你看到应该是这个界面，填入版本库地址，选择确定。



![img](https://img-blog.csdn.net/20180518145631544)

如果是第一次登陆，此时会弹出一个对话框让你输入账号密码，输入你的账号密码即可。记得勾选保存认证，不然每次操作都会让你输入。



![img](https://img-blog.csdn.net/20180518145644483)

过几秒就会检出完成



![img](https://img-blog.csdn.net/20180518145702436)

找到目录就可以开始工作了

![img](https://img-blog.csdn.net/20180518145745994)

导入项目

右键选着版本浏览器



![img](https://img-blog.csdn.net/20180518145802326)



![img](https://img-blog.csdn.net/20180518145826921)

根据自己的项目上传你的文件或者文件夹

![img](https://img-blog.csdn.net/20180518150613445)



选着你的项目或者文件后

![img](https://img-blog.csdn.net/20180518150131849)



确定看到目录完成就行了

但是，不要以为导入成功就可以了。你还得重新检出，重新检出的项目才是受SVN控制的，务必记得检出，如果不检出你操作的属于你没有上传之前的文件，当你下次上传可能会出现问题。

![img](https://img-blog.csdn.net/20180518150146942)



在SVNProject上右键检出到本地，然后在里面进行修改。现在就可以愉快的工作了。 
检出过后点击文件夹然后右键菜单变成了这样。

![img](https://img-blog.csdn.net/20180518150205541)

关于项目的提交

​       绿色表示当前文件没有被修改过（看不见颜色的重启下电脑就好了）。



![img](https://img-blog.csdn.net/20180518150222950)



如果在我的Dome里面对代码进行了修改。你就会发现现在变成了红色，红色表示已修改。



![img](https://img-blog.csdn.net/20180518150237535)

怎么提交修改？ 
在根目录下，右键选择提交。



![img](https://img-blog.csdn.net/20180518150252116)

务必记得输入提交信息（虽然不输入也能提交），提交信息可以方便日后查看。



![img](https://img-blog.csdn.net/20180518150429523)

提交完毕后，可以发现又恢复到了绿色。如果看到还是红色可以退出后在进入就行了。



![img](https://img-blog.csdn.net/20180518150400988)

假如现在加入了一个新文件。可以看出是蓝色的。蓝色表示不属于版本库的未知文件，未知文件是不能提交的。有可能什么都不显示。



![img](https://img-blog.csdn.net/20180518150637883)或![img](https://img-blog.csdn.net/20180518150649509)

记住选择增加把它加入到版本库里面去。



![img](https://img-blog.csdn.net/20180518150838282)

增加完毕后，变成了蓝色加号，表示新增加的版本库文件。



![img](https://img-blog.csdn.net/20180518151103416)

接下来，只需写代码，然后提交即可。 
删除文件也应该右键提交，如下。

![img](https://img-blog.csdn.net/20180518151155750)

记得随时检查你的文件状态，如果没有添加到版本控制里要及时添加进去，不然你的文件提交不上去。

更新：

​        假如你和B同事在协作。B同事写完代码提交到了SVN上，如果你想获取最新修改，就需要选择更新（如果服务器上已经有别人提交过的新的，你是提交不上去的，必须先更新再提交）。 
​        怎么知道服务器有没有更新？你可以直接选择更新，有没有更新一下就知道。或者右键检查修改，然后检查版本库，就能看到服务器上改了哪些文件。

![img](https://img-blog.csdn.net/2018051815121280)

![img](https://img-blog.csdn.net/20180518151231158)

右键选择版本比较。左边的表示你的代码，右边的表示服务器上的代码。

![img](https://img-blog.csdn.net/20180518151247660)

如果有修改记得及时更新到本地然后再继续工作。没有更新会提交失败。

![img](https://img-blog.csdn.net/20180518151259685)

但是有时候更新会冲突，比如你和服务器上的改了同一个地方。 
这时候你需要更新下来解决冲突。

![img](https://img-blog.csdn.net/20180518151311261)

于是可以查看日志，看前面谁进行了相同模块的更改。方便代码覆盖相同进行协商。



![img](https://img-blog.csdn.net/20180518151331779)

它会提示你哪个文件冲突，你只需打开那个文件，按照需求解决冲突即可



![img](https://img-blog.csdn.net/2018051815134764)

解决冲突有三种选择： 

​        A、放弃自己的更新，使用svn revert（回滚），然后提交。在这种方式下不需要使用svn resolved（解决） 

​        B、放弃自己的更新，使用别人的更新。使用最新获取的版本覆盖目标文件，执行resolved filename并提交(选择文件—右键—解决)。 

​        C、手动解决：冲突发生时，通过和其他用户沟通之后，手动更新目标文件。然后执行resolved filename来解除冲突，最后提交。

如何降低冲突解决的复杂度：

​        1、当文档编辑完成后，尽快提交，频繁的提交/更新可以降低在冲突发生的概率，以及发生时解决冲突的复杂度。

​        2、在提交时，写上明确的message，方便以后查找用户更新的原因，毕竟随着时间的推移，对当初更新的原因有可能会遗忘



​        3、养成良好的使用习惯，使用SVN时每次都是先提交，后更新。每天早上打开后，首先要从版本库获取最新版本。每天下班前必须将已经编辑过的文档都提交到版本库。



查看日志

​        择显示日志，可以看出团队里面的人干了什么。

![img](https://img-blog.csdn.net/20180518151410862)

可以看出谁谁谁，什么时间，干了什么事。最后那一列信息是自己提交的时候写的。建议大家提交时务必要填写提交信息，这样别人一看就知道你干了什么。提交信息对于自己也是有好处的，时间长了也能看到当初做了什么。



![img](https://img-blog.csdn.net/20180518151429296)

版本回滚

如果你改了东西，但是还没有提交，可以使用还原功能。



![img](https://img-blog.csdn.net/20180518151440738)

​        但是如果我们写错了东西并且提交了上去怎么办？通过版本回滚可以将文件恢复到 
​        以前的版本。右键更新至版本，通过查看日志来选择版本，然后回滚即可。 



![img](https://img-blog.csdn.net/20180518151450951)



有时候我们需要查看以前版本的代码。此时我们可以新建个文件夹检出到指定版本，不要把现在自己编写的版本覆盖就好



![img](https://img-blog.csdn.net/20180518151704942)

版本控制

版本控制有好几种方法，如下。

​    \1.    在提交发布版本时添加版本信息，这是最简单的一种方法。



![img](https://img-blog.csdn.net/20180518151711903)

​    2.打标签 
每次发布版本时应该打标签。右键选择分支/标记。在至路径以版本号打上标签即可 

![img](https://img-blog.csdn.net/20180518151719116)



![img](https://img-blog.csdn.net/20180518151727389)

这样你就有了一个v1.0版本的标签。 
以后如果你想查看某个版本的代码，只需切换过去就行 



![img](https://img-blog.csdn.net/20180518151741130)

创建分支合并相互操作

   项目中为何要创建分支，及合并？

​      比如我现在项目所有的文件放在主干上中，由于需求的变更，需要增加新的需求，但是我们主干上还要继续往下开发，在此我们可以新建一个分支，来做增加新的需求那一块，主干上继续开发，等分支上代码没有问题的时候，再合并到主干上来。

创建分支的最大的目的就是跟主线进行并行开发时候不影响主线的开发。

   如何操作？

​      假如我本地新建一个文件夹test下有2个文件夹Cs (存放主干上的代码)和C_s(存放分支上的代码)，如下所示：

![img](https://img-blog.csdn.net/20180518151820297)

新建分支

  从Cs（主干上）创建分支C_s步骤如下：右键Cs



![img](https://img-blog.csdn.net/20180518151829789)

现在我们可以再来看看本地branch文件夹了，我现在直接进入branch文件下，右键 --> Chenckout下，就可以把newBranch下的所有文件提取出来了，如下所示：



![img](https://img-blog.csdn.net/20180518151858429)

现在我们可以再来看看本地test文件夹了，我现在直接进入test文件下，右键 --> 检出下，就可以把C_s下的所有文件提取出来了，如下所示：

![img](https://img-blog.csdn.net/20180518151915270)

​        分支目前建立在svn的服务器端，本地并没有更新，对本地C_s文件夹 右键--> 更新即可，就可以更新到分支代码.

合并分支到主干上

​       比如我现在对C_s分支上新增 新的文件.txt文件，然后提交上去

![img](https://img-blog.csdn.net/20180518151935581)

我现在想把分支上的代码新增 新的文件.txt合并到主干上Cs，现在要怎么合并呢？步骤如下：

  \1. 回到我们刚刚的主干（Cs）文件夹下，鼠标右键该文件夹--> TortoiseSVN --> Merge(合并) 如下图所示：

![img](https://img-blog.csdn.net/20180518151948287)



![img](https://img-blog.csdn.net/20180518151957149)



接着点击【Next】下一步，如下图所示：



![img](https://img-blog.csdn.net/20180518152226169)



![img](https://img-blog.csdn.net/20180518152237562)

就可以看到主干Cs上多加了一个新的文件.txt，就是从分支上合并过来的。

![img](https://img-blog.csdn.net/20180518152723253)

合并主干到分支

 如果主干上有一些更新，比如说jar包更新等等，那么这些要更新到分支上去，如何操作呢？比如我现在在主干上新建一个1.txt文件



![img](https://img-blog.csdn.net/20180518152244724)

我现在的分支上目录如下：

![img](https://img-blog.csdn.net/20180518152309438)

现在是想把主干上的1.txt合并到分支上来，要如何操作？

步骤如下，还是和刚刚操作类似.

我们在分支点击C_s--> 右键TortoiseSVN--> 合并 如下图所示：

![img](https://img-blog.csdn.net/20180518152332466)

![img](https://img-blog.csdn.net/20180518152349155)

![img](https://img-blog.csdn.net/20180518152358419)



![img](https://img-blog.csdn.net/20180518152415299)





最后直接合并，就可以看到分支C_s上也有主干上的1.txt文件了，也就是说，合并主干到分支上也是可以的，如下图所示：



![img](https://img-blog.csdn.net/20180518152433265)

如果使用eclipse就没有这么麻烦，最多就是安装一个svn插件的问题，操作起来而且方便，因为我们公司有的开发没用eclipse才写下的这篇，从而分享出来的，希望能帮助你们。

[![返回主页](https://www.cnblogs.com/skins/custom/images/logo.gif)](https://www.cnblogs.com/xqx-qyy/)



# svn使用教程



 

## SVN使用指引（本地服务器为Windows）







## 1. 安装SVN客户端

使用SVN进行文件上传前，请在您的本地PC上安装SVN客户端。推荐使用1.7版本的SVN客户端。请不要升级到1.8版本，TortoiseSVN 1.8版本存在缺陷，可能会导致SVN同步异常。
本地服务器为Windows时，推荐使用TortoiseSVN，下载地址：http://sourceforge.net/projects/tortoisesvn/files/
下面的操作指引都以TortoiseSVN为例。





## 2. 创建本地目录并连接到SVN库

\1. 在您的本地机器上新建一个目录，例如“MyApp”，如下图所示：
![SVN_2.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_2.png)
\2. 进入该文件夹，鼠标右键点击空白处，在邮件菜单中选择“SVN Checkout...”，如下图所示：
![SVN_3.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_3.png)
\3. 在弹框里填入您的应用的SVN库的路径（你可能需要了解[如何获取SVN仓库地址](http://wiki.open.qq.com/wiki/SVN简介及使用限制#5._SVN.E5.BA.93.E5.9C.B0.E5.9D.80.E8.AF.B4.E6.98.8E)），弹框中的其它选项保持默认，如下图所示：
![SVN_4.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_4.png)
\4. 点击弹框中的“OK”按钮，首次登录时要求输入该SVN版本库的用户名和密码（即应用的云服务账号和密码，您可能需要了解[如何查看云服务账号和密码](http://wiki.open.qq.com/wiki/云服务账号管理_V2)）。
登录框如下图所示：
![SVN_21.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_21.png)
注意不要勾选下面的“Save authentication”，原因是如果1个开发者有多个应用，则有多个SVN库，保留1个SVN库的登录凭证可能会导致登录别的SVN库失败。
如果失败，请选择右键菜单的“TortoiseSVN”->“Settings”->“Save Data”对话框中，点击“Authentication data”旁的“Clear”按钮，清除登录凭证。 清除登录凭证如下图所示：

![SVN_22.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_22.png)

\5. 通过验证后，即开始从SVN库中checkout该SVN库里的所有文件。如下所示：
![SVN_5.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_5.png)
\6. Checkout成功后，即可在本地机器“MyApp”目录下看到该SVN库下的所有文件。
注意，对于CEE SVN库来说：
（1）如果输入的SVN库路径是该应用的SVN库地址，则checkout出该应用所有的WebService下的所有版本的代码。MyApp目录下是您的应用下所有的WebService的目录，每个WebService是1个文件夹。
![SVN_7.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_7.png)
（2）如果你输入的是您的应用的1个Web服务下的1个版本的SVN路径，则只会checkout出该版本下的代码。每个WebService下是所有的版本目录，每个版本是1个文件夹。
![SVN_8.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_8.png)
（3）在您还没有上传任何代码到SVN库之前，这里checkout出来的只是目录，版本目录下是没有文件的（除了自动生成的.svny文件夹以及index.html ）。
![SVN_9.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_9.png)





## 3. 提交文件

\1. 将您需要提交的复制到本地对应的目录下。
例如您需要将文件“test2.php”上传到Web服务“helloc”下的版本“1”里，则需要将您的应用程序复制到“MyApp/10507/helloc/1”目录下。如下图所示：
![SVN_10.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_10.png)
2.右键点击文件“test2.php”，选择“TortoiseSVN -> Add”菜单，即将刚才复制的代码添加到SVN工作目录中。
（只要是新增了文件，在提交前都必须先“add”，否则SVN不识别该文件） 如下图所示：
![SVN_11.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_11.png)
add成功后，该文件的图标变成蓝色的十字，如下图所示：
![SVN_12.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_12.png)
\3. 然后右键点击文件“test2.php”，选择“SVN Commit..”菜单，然后填写本次提交的日志（必填项，不填将导致提交失败），即将刚才复制的代码提交到SVN工作目录中。
如下图所示：
![SVN_13.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_13.png)
![SVN_14.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_14.png)
![SVN_15.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_15.png)
\4. 提交成功后，该文件的图标会变为绿色的对勾，如下图所示：
![SVN_16.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_16.png)







## 4. 修改文件

\1. 您可以直接在本地使用编辑器打开SVN工作目录下的某个文件并进行修改，修改完成后，可以看到该文件的图标变成红色的感叹号，如下图所示：
![SVN_17.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_17.png)
\2. 右键点击该文件，选择“SVN Commit...”，并填写本次提交的日志（必填项，不填将导致提交失败），即将该更新提交到SVN库。
\3. 对于CEE SVN库来说，对于当前生效版本，我们强烈建议您在提交前需保证所做的修改已经通过了测试，以避免影响现网服务。



## 5. 历史版本回滚

SVN服务器天然支持版本管理，因此如果开发者需要对某些历史版本进行回滚，可以直接在SVN客户端上进行历史版本回滚操作，将历史版本的目录或文件下载到本地服务器，然后再提交到SVN服务器即可。
详细说明如下：
\1. 在需要进行历史版本回滚的目录或文件上，点击右键，选择菜单“TortoiseSVN”->“Show log”，如下图所示：
![SVN_18.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_18.png)


\2. 在打开的“Log Messages”面板中，右键选中你要回滚的版本，在出现的右键菜单中选择“Revert to this revision”，即可执行回滚操作。如下图所示：
![SVN_19.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_19.png)


\3. 回滚成功后，本地的目录或文件就被历史版本的目录或文件替代，可看到其图标变成红色的感叹号。

\4. 开发者需要将该目录或文件提交到SVN服务器上，即右键选中该目录或文件，然后选择菜单中的“SVN Commit...”将文件提交到服务器。提交成功后，即完成历史版本的回滚。





## 6. 删除文件

步骤如下：
\1. 在需要删除的目录或文件上，点击右键，选择菜单“TortoiseSVN”->“Delete”，如下图所示：
![SVN_20.png](http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/SVN_20.png)
\2. 点击“Delete”后，可以看见本地已经不存在该文件或目录。
\3. 右键点击已删除文件原来所在的目录，选择“SVN Commit...”，并填写本次提交的日志（必填项，不填将导致提交失败），提交到SVN库。





## 7. 其它操作

SVN的操作与一般的SVN操作是一致的，这里不再列举，您可以参考[SVN手册](http://svndoc.iusesvn.com/)。