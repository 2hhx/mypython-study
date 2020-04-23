#主流的Linux发行版
Ubuntu， Debian ，Fedora， CentOS，Red Hat，Red-flag Linux

*************************************************************************************************

#常用命令
rmdir 命令删除目录
mkdir命令创建目录
tar命令，用于压缩解压
pwd命令，作用为查看”当前工作目录“的完整路径
mv命令作用为移动文件
cp 命令，作用复制
tree命令，显示树形的层级目录结构，非原生命令，需要安装tree
cd 命令
ls
ps 命令显示运行的进程，还会显示进程的一些信息如pid, cpu和内存使用情况等
kill 命令用于终止进程
crontab命令是启动linux定时任务的服务
top 命令是Linux下常用的性能分析工具，能够实时显示系统中各个进程的资源占用状况
chmod 更改权限命令
useradd 命令建立用户账号
userdel 删除用户
groupadd　命令用于将新组加入系统
groupdel　命令删除组
passwd 设置用户的密码
cat　用途是连接文件或标准输入并打印

*************************************************************************************************


#ifconfig的功能是什么？
ifconfig是linux中用于显示或配置网络设备（网络接口卡）的命令，英文全称是network interfaces configuring



*************************************************************************************************

#查看cpu信息
ubuntu 
在终端输入lscpu
centos7
在终端输入cat /proc/cpuinfo

*************************************************************************************************


#查看cpu 使用率最高的程序
top

*************************************************************************************************

#显示日志文件末尾50行
tail -n 50 /var/log/testlog

#显示文件前20行
tail -n +20 filename

#逆序显示filename最后10行
tail -r -n 10 filename

#监视filename文件的尾部内容（默认10行），刷新显示在屏幕上
tail -f filename
相当于
tail -f -n 10 filename

*************************************************************************************************


#查看日志文件是否发生过异常，如果文件中有输出error就代表有异常
cat /var/log/testlog | grep error

*************************************************************************************************

#硬件测试工具
比如频谱分析仪、示波器、高精度电源、信号发生器、逻辑分析仪、高低温箱等
熟悉串口、RJ45、等接口测试以及wifi、ZigBee、433天线测试，了解无线通信相关知识


#软件测试工具
功能测试工具：QTP

接口：Jmeter、Postman、soapUI，apizza

自动化：测试Web结构的Selenium，测试移动应用结构的Appium以及 Watir、MaxQ、WebInject等

性能：
LoadRunner，Jmeter、OpenSTA、DBMonster、TPTEST、Web Application Load Simulator
NovaBench：综合测试工具
Dbench：文件服务器的测试工具，只对io产生负载
Ramspeed：内存测速
super_pi:计算圆周率的工具，测试ram和cpu的稳定性
Iperf：网速测试工具
yslow、dyanTrace：网站性能测试
Monkey：稳定性测试
locust ：性能测试
GT：App性能测试
appscan：web渗透测试


单元测试：Junit及TestNG

*************************************************************************************************

bug管理工具：Bugfree、Bugzilla、TestLink、mantis，jira，禅道
用例管理：Quality Center，Test Direct，ALM，teambition,worktile
回归测试：QuickTest Professional

*************************************************************************************************

敏捷开发工具：
1，VersionOne
VersionOne市场排名第一。支持SaaS模式和本地安装模式，web客户端，支持Scrum, Extreme Programming, DSDM and Agile UP等多种敏捷开发方法。团队可以使用“V1：敏捷团队”来管理产品和sprint backlog，通过交互式的“任务板（taskboards）“和”测试板（testboards）” 进行每日开发活动，藉由报表和燃烧图查看进度，以及其他活动。

2，Leangoo
Leangoo国内的一款scrum工具，由国内最权威的Scrum中文网研发，看板式管理，界面非常的简洁，轻量，容易上手，有三个版本，永久免费个人版，在线企业版，私有部署企业版。
不仅很好的支持Scrum，更支持和DevOps／持续集成平台互通！
它的团队工作体现为卡片，看板上的主要元素包括列表和泳道，列表管理工作的不同阶段或状态，泳道实现任务的分组对应，从两个纬度让团队的工作高度可视化。
支持燃尽图，工作量估算，泳道以及仪表盘，团队速度的统计，任务的分布，任务的周期以及缺陷分布，企业级仪表盘，吞吐量以及需求的趋势图等等等等！

3，ScrumWorks
ScrumWorks市场排名前3甲，有两个种版本，Basic是免费版，Pro是收费版，按用户每年249美元。支持BS和CS两种模式，服务器端是要安装的。
支持对Bugzilla和Jira的集成，带有主题过滤功能的burndown图表，以及其他辅助了解项目状况和走势的功能，还有众多别的特性。
ScrumWorks Pro与Bugzilla和Jira的集成，体现在它可以导入两者中的条目作为backlog条目，并且可以像对其他backlog条目一样，对这些条目 进行操作。可以使用搜索来选择感兴趣的条目，并进行单独或多项导入操作。

4，LeanKit
LeanKit使用一个基于云的whiteboard来规划组织过程。每个卡代表一个工作项目，并提供状态更新选项。使用LeanKit的团队可以看见工作负载分配并导出历史数据。

5. confluence
进行敏捷开发怎么能少了Confluence，它一个专业的wiki程序。它是一个知识管理的工具，通过它可以实现团队成员之间的协作和知识共享。想想维基百科，你就知道confluence的便利之处。