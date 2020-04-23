# 一 什么是SHELL

shell一般代表两个层面的意思，一个是命令解释器，比如BASH，另外一个就是shell脚本。本节我们站在命令解释器的角度来阐述shell

命令解释器SHELL的发展历史,SH-CSH-KSH-TCSH-BASH，我们先来了解下命令解释器BASH

# 二 命令的优先级

命令分为：

==> alias  
==> Compound Commands  
==> function  
==> build_in  
==> hash  
==> $PATH  
==> error: command not found

获取一个命令会按照上述优先级取寻找，先找同名的alias命令，再找compound命令。。。  
  

**=================================part1**

让我们先从最简单的入手：别名、内部命令、外部命令，来探讨它们三者的优先级

**别名** ：别名命令是为了简化输出给一个长参数命令的整合，别名的定义方法 alias la='ls -al' 取消别名 unalias la  
**内部命令** ：是BASH自带的命令 功能简单,内部命令的帮助在builtin(1)里  
**外部命令** ：是就是一个小程序存在于/bin/ /sbin/ /usr/bin 等地方  
  

  
  
[root@seker ~]#  
[root@seker ~]# alias cd  
-bash: alias: cd: not found  
cd是一个内部命令 属于bash软件自带命令(参考man cd) 它没有定义别名  
  
[root@seker ~]#  
[root@seker ~]# alias ls  
ls被定义了别名  
alias ls='ls --color=tty'  
[root@seker ~]#  
[root@seker ~]# which ls  
alias ls='ls --color=tty'  
/bin/ls  
[root@seker ~]#  
ls实际是一个外部命令 属于可执行程序 是通过C代码编译得出的可执行程序  
[root@seker ~]# file /bin/ls  
/bin/ls: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), for
GNU/Linux 2.6.9, dynamically linked (uses shared libs), for GNU/Linux 2.6.9,
stripped  
[root@seker ~]#

  
验证:别名是优先于内部命令和外部命令的

[root@MiWiFi-R3-srv ~]# alias cd='ls -l' #建立一个别名是cd但实际指向的是/bin/ls的别名  
[root@MiWiFi-R3-srv ~]# cp /bin/hostname /usr/local/sbin/cd
#在PATH第一个目录里放入一个cd命令  
You have new mail in /var/spool/mail/root

  
[root@MiWiFi-R3-srv ~]# cd #此时执行cd命令 是找到得别名cd优先  
total 900  
-rw-r--r--. 1 root root 0 Mar 20 10:10 4.txt  
-rw-------. 1 root root 956 Mar 8 09:17 anaconda-ks.cfg  
-rw-r--r--. 1 root root 0 Mar 20 10:09 a.txt  
-rw-r--r--. 1 root root 0 Mar 20 10:10 D.txt  
drwxr-xr-x. 9 jack jack 4096 Mar 19 14:40 nginx-1.10.3  
-rw-r--r--. 1 root root 911509 Mar 19 12:49 nginx-1.10.3.tar.gz  
[root@MiWiFi-R3-srv ~]# unalias cd #删除了别名,此时在去搜索 就是内部命令优先 得到了真正的cd命令  
[root@MiWiFi-R3-srv ~]# cd #执行的就是系统内置的cd

[root@MiWiFi-R3-srv ~]# /usr/local/sbin/cd #此时想越过内部命令去执行外部命令 就是之前cp
/bin/hostname /bin/cd留下的cd  
MiWiFi-R3-srv

[root@seker ~]# rm -rf /usr/local/sbin/cd  
[root@seker ~]#

**小结一** ：命令的执行搜索顺序  
==>别名 (alias可以查看)  
==> bash内部命令  
==> $PATH 中按冒号分割的每个路径中去搜索

**ps：登陆后的预置别名从何而来** （别名应该学会的东西:取消别名和建立别名,固化别名配置我们会在后续章节介绍，无非将alias定义到文件中）:

[root@MiWiFi-R3-srv ~]# alias  
alias cp='cp -i'  
alias egrep='egrep --color=auto'  
alias fgrep='fgrep --color=auto'  
alias grep='grep --color=auto'  
alias l.='ls -d .* --color=auto'  
alias ll='ls -l --color=auto'  
alias ls='ls --color=auto'  
alias mv='mv -i'  
alias rm='rm -i'  
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-
tilde'

[root@seker ~]#

一部分来自/root/.bashrc 普通用户的.bashrc不包含别名,因为普通用户的.bashrc模板是:/etc/skel/.bashrc  
而root这个.bashrc在安装完系统就预置了.  
[root@MiWiFi-R3-srv ~]# grep '^alias' /root/.bashrc  
alias rm='rm -i'  
alias cp='cp -i'  
alias mv='mv -i'  
[root@seker ~]#  
一部分来自/etc/profile.d/目录里的可执行文件

[root@MiWiFi-R3-srv ~]# grep -rn 'alias' /etc/profile.d/  
/etc/profile.d/colorgrep.csh:9:alias grep 'grep --color=auto'  
/etc/profile.d/colorgrep.csh:10:alias egrep 'egrep --color=auto'  
/etc/profile.d/colorgrep.csh:11:alias fgrep 'fgrep --color=auto'  
/etc/profile.d/colorgrep.sh:5:alias grep='grep --color=auto' 2>/dev/null  
/etc/profile.d/colorgrep.sh:6:alias egrep='egrep --color=auto' 2>/dev/null  
/etc/profile.d/colorgrep.sh:7:alias fgrep='fgrep --color=auto' 2>/dev/null  
/etc/profile.d/which2.csh:5:# alias which 'alias | /usr/bin/which --tty-only
--read-alias --show-dot --show-tilde'  
/etc/profile.d/which2.sh:4:alias which='alias | /usr/bin/which --tty-only
--read-alias --show-dot --show-tilde'  
  

  
**=================================part2**

内置命令bash built-in commands  
很多种  
**第一种:bash自身带的功能 参见man cd**  
NAME  
bash, :, ., [, alias, bg, bind, break, builtin, cd, command, compgen,
complete,  
continue, declare, dirs, disown, echo, enable, eval, exec, exit, export, fc,
fg,  
getopts, hash, help, history, jobs, kill, let, local, logout, popd, printf,  
pushd, pwd, read, readonly, return, set, shift, shopt, source, suspend, test,  
times, trap, type, typeset, ulimit, umask, unalias, unset, wait

**第二种:Compound Commands**  
例如 for if while等  
参见 man bash  
  
**第三种:function**  
[root@MiWiFi-R3-srv ~]# function cd() { echo 'my function cd'; }  
有什么函数可以通过set命令找到  
[root@MiWiFi-R3-srv ~]# set |grep cd

cd ()  
echo 'my function cd'

**验证：alias，compund优先级**

[root@MiWiFi-R3-srv ~]# alias if='ls -l'  
[root@MiWiFi-R3-srv ~]#  
[root@MiWiFi-R3-srv ~]# if #证明别名比compound的优先级高  
total 900  
-rw-r--r--. 1 root root 0 Mar 20 10:10 4.txt  
-rw-------. 1 root root 956 Mar 8 09:17 anaconda-ks.cfg  
-rw-r--r--. 1 root root 0 Mar 20 10:09 a.txt  
-rw-r--r--. 1 root root 0 Mar 20 10:10 D.txt  
drwxr-xr-x. 9 jack jack 4096 Mar 19 14:40 nginx-1.10.3  
-rw-r--r--. 1 root root 911509 Mar 19 12:49 nginx-1.10.3.tar.gz

  
**验证：compound与function的优先级**

[root@MiWiFi-R3-srv ~]# function for() { echo 'my function for'; }  
[root@MiWiFi-R3-srv ~]# for((i=1;i<3;i++));do echo $i;done
#执行的仍然是compound命令，证明compound优先级比函数高  
1  
2

**验证：compound与function的优先级**

[root@MiWiFi-R3-srv ~]# function cd() { echo 'my function cd'; }  
[root@MiWiFi-R3-srv ~]# cd #执行的是自己的函数而不是内置命令cd，证明函数比内置命令优先级高  
total 900  
-rw-r--r--. 1 root root 0 Mar 20 10:10 4.txt  
-rw-------. 1 root root 956 Mar 8 09:17 anaconda-ks.cfg  
-rw-r--r--. 1 root root 0 Mar 20 10:09 a.txt  
-rw-r--r--. 1 root root 0 Mar 20 10:10 D.txt  
drwxr-xr-x. 9 jack jack 4096 Mar 19 14:40 nginx-1.10.3  
-rw-r--r--. 1 root root 911509 Mar 19 12:49 nginx-1.10.3.tar.gz

  
**小结二**  
==> alias  
==> Compound Commands  
==> function  
==> build_in  

**=================================part3**  
当别名和内部命令都搜到不到时 找$PATH

[root@MiWiFi-R3-srv ~]# sed -n 's/:/\n/gp' <<< $PATH  
/usr/local/sbin  
/usr/local/bin  
/sbin  
/bin  
/usr/sbin  
/usr/bin  
/root/bin

  
因为这个变量中包含的路径太多,每个路径中的可执行文件也很多  
如果每次都要搜索每个路径下的所有可执行文件,显然是不明智的  
为了减少$PATH的搜索,上一次搜索的内容能够被下一次执行重用  
bash对从$PATH中搜索得出的外部命令建立一个hash表,用于缓存  
这个缓存是会话级别独立拥有的.不可以对其他终端共享,因为每个用户的$PATH可能不同  

[root@MiWiFi-R3-srv ~]# hash -r #清除缓存  
[root@MiWiFi-R3-srv ~]# hostname #每执行一次就缓存一次  
MiWiFi-R3-srv  
[root@MiWiFi-R3-srv ~]# hash #查看缓存表，发现多了一条缓存

hits command

1 /bin/hostname

[root@MiWiFi-R3-srv ~]# mv /bin/hostname /sbin/ #将hostname命令移动到另外一个目录  
[root@MiWiFi-R3-srv ~]# hostname
#再次执行hostname，仍然是从缓存中读hostname所在的位置，证明hash优先级比path高，hash缓存的是path路径，而内置命令如cd等没有路径，因而无需罗嗦测试，内置的优先级肯定是比hash要高的  
bash: /bin/hostname: No such file or directory

**总结命令执行的获取顺序:**  
==> alias  
==> Compound Commands  
==> function  
==> build_in  
==> hash  
==> $PATH  
==> error: command not found  
  

# 三 元字符

bash中的特殊字符,键盘上能敲出来的特殊字符都有其特殊意义，强调一点： **元字符是被shell解释的**

**`` 命令替换 取命令的执行结果**

[root@MiWiFi-R3-srv ~]# ls  
4.txt anaconda-ks.cfg a.txt B.txt c.txt D.txt nginx-1.10.3 nginx-1.10.3.tar.gz  
[root@MiWiFi-R3-srv ~]# res=`ls` #取命令的运行结果，赋值给变量res  
[root@MiWiFi-R3-srv ~]# echo $res #查看变量res的值  
4.txt anaconda-ks.cfg a.txt B.txt c.txt D.txt nginx-1.10.3 nginx-1.10.3.tar.gz

**$()同上,但它弥补了``的嵌套缺陷**

[root@MiWiFi-R3-srv ~]# res=`echo `ls`` #嵌套使用后无法达到预想的效果：取echo 一堆文件名的效果。

[root@MiWiFi-R3-srv ~]# echo $res  
ls

[root@MiWiFi-R3-srv ~]# res=$(echo $(ls)) #替代方案  
[root@MiWiFi-R3-srv ~]# echo $res  
4.txt anaconda-ks.cfg a.txt B.txt c.txt D.txt nginx-1.10.3 nginx-1.10.3.tar.gz

**~ 家目录**

[root@MiWiFi-R3-srv tmp]# cd ~  
[root@MiWiFi-R3-srv ~]# pwd  
/root

**! 取非**

[root@MiWiFi-R3-srv ~]# ls /dev/sda  
sda sda1 sda2  
[root@MiWiFi-R3-srv ~]# ls /dev/sda[0123]  
/dev/sda1 /dev/sda2  
[root@MiWiFi-R3-srv ~]# ls /dev/sda[!01]  
/dev/sda2

**! 历史命令调用**

[root@MiWiFi-R3-srv ~]# !343  
hostname  
MiWiFi-R3-srv

**! 匹配最近一次历史命令**

[root@MiWiFi-R3-srv ~]# !ls  
ls /dev/sda[!01]  
/dev/sda2

**! ls 带空格 将命令的返回值取反**

[root@MiWiFi-R3-srv ~]# echo ok  
ok  
[root@MiWiFi-R3-srv ~]# echo $? #上一条命令执行的结果，0代表执行成功，非0代表执行失败  
0  
[root@MiWiFi-R3-srv ~]# ! echo ok #将结果取反  
ok  
[root@MiWiFi-R3-srv ~]# echo $?  
1

0-255之间,0则为真,非0位假

**@ 无特殊含义**

**# 注释**

$ 变量取值  
$() 同``  
${} 变量名的范围

$[] 整数计算 echo $[2+3] - * / % 浮点数用 echo "scale=3; 10/3" | bc -l

[root@MiWiFi-R3-srv ~]# money=10  
[root@MiWiFi-R3-srv ~]# echo $money  
10  
[root@MiWiFi-R3-srv ~]# echo 00000$money  
0000010  
[root@MiWiFi-R3-srv ~]# echo $money0000

[root@MiWiFi-R3-srv ~]# echo ${money}0000  
100000

**% 杀后台进程 jobs号; 取模**

**^ 取非 和 ! 雷同**

[root@MiWiFi-R3-srv ~]# ls /dev/sda[^01]  
/dev/sda2  
[root@MiWiFi-R3-srv ~]# ls /dev/sda[!01]  
/dev/sda2

**^ 替换 **

[root@MiWiFi-R3-srv ~]# systemctl restart network  
[root@MiWiFi-R3-srv ~]# ^network^sshd^  
systemctl restart sshd

**& 后台执行;&& 逻辑与**

*** 匹配任意长度字符串;计算乘法**

**() 在子进程中执行**

[root@MiWiFi-R3-srv ~]# x=1  
[root@MiWiFi-R3-srv ~]# (x=666)  
[root@MiWiFi-R3-srv ~]# echo $x  
1  
[root@MiWiFi-R3-srv ~]#  
[root@MiWiFi-R3-srv ~]# (x=666;echo $x)  
666

**\- 减号;区间;cd -;**

**_ 无特殊含义**

**\+ 加号 ;**

**= 赋值**

**| 管道; || 逻辑或**

**\ 转义;**

**{} 命令列表 ，注意括号内的开头和结尾必须是空格** **{ ls; cd /; }**

**[] 字符通配，匹配括号内之一;**

**: 空命令 真值**

[root@MiWiFi-R3-srv ~]# :  
[root@MiWiFi-R3-srv ~]# echo $?  
0

**; 可以接多个命令：ls；pwd；echo 123;无论对错，会一直执行到最后一条命令**

**"" 软引 ''硬引**

**< 输入重定向**

**> 输出重定向**

**> > 追加**

**< < here document**

**> & 合并2和1输出**

**, 枚举分隔符**

**. source ; 当前目录**

**/ 目录分隔符**

**? 单个字符**

**回车 命令执行**

*** 通配符:任意字符**  
 **? 通配符:任一字符**  
 **[abc] 列表项之一**  
 **[^abc] 对列表取非 也可以使用范围 [a-z] 代表aAbBcC...,[0-9]代表012345。。。**  
 **{} 循环列表**

[root@MiWiFi-R3-srv test]# touch {1..3}{a..d}.txt  
[root@MiWiFi-R3-srv test]# ls  
1a.txt 1b.txt 1c.txt 1d.txt 2a.txt 2b.txt 2c.txt 2d.txt 3a.txt 3b.txt 3c.txt
3d.txt

**控制变量名的范围 echo ${AB}C**

**硬引用与软引用**

[root@MiWiFi-R3-srv test]# x=1  
[root@MiWiFi-R3-srv test]# echo "$x" #双引号的代表软引用，引号内特殊字符有特殊意义，比如$,``等  
1  
[root@MiWiFi-R3-srv test]# echo '$x' #单引号代表硬引用，引号内所有字符都无特殊意义  
$x

**\转意**

[root@MiWiFi-R3-srv test]# echo \\\  
\  
[root@MiWiFi-R3-srv test]# echo \'  
'  
[root@MiWiFi-R3-srv test]# echo "'"  
'

# 四 bash属性

BASH SHELL 属性  
BASH中会存储一些自身属性的参数,启用或关闭某一项功能  
例如控制* .字符是否为通配  
查看参数 set -o  
关闭noglob参数  
# set -o noglob  
# ls *  
ls: *: 没有那个文件或目录  
# set +o noglob  
ls *

固化设定  
我们前面所学习的更改变量 属性等等都是在内存中修改 机器重新启动后就会恢复默认值  
那么怎么固化这些设置 让他们永久生效呢?  
这就需要了解BASH两种类型  
1.登录shell 2.非登录shell  
登录shell  
就是通过输入用户名 密码后 或 su - 获得的shell  
非登录shell 则是通过bash命令和脚本开启的shell环境  
那么他们有什么区别呢?和我们固化设定又有什么关系呢?  
我们知道在linux里一切皆为文件,同样,shell的属性加载也是写到文件里的  
在登陆时就会加载对应文件的内容来初始化shell环境,  
非登录与登录区别就在于加载的文件不同 从而导致获得的shell环境不同  
我们看看登录shell都加载了那些文件  
\--> /etc/profile  
\--> /etc/profile.d/*.sh  
\--> $HOME/.bash_profile  
\--> $HOME/.bashrc  
\--> /etc/bashrc  
再看非登录shell加载的文件  
\--> $HOME/.bashrc  
\--> /etc/bashrc  
\--> /etc/profile.d/*.sh  
可见,非登录shell加载的文件要少很多  
那么我们想要固化一个配置时在哪种登录下生效,就显而易见的知道该写在哪个文件里了  
通常,我们会将环境变量设置在 $HOME/.bash_profile 中  
如果不管哪种登录都想使用的变量 就设置在 $HOME/.bashrc中

命令补齐TAB键  
简化输入 提示 防止书写错误

  
历史记录  
上下键查  
history 查询 用!ID 调用  
ctrl+r 输入匹配

快捷键  
CTRL+A 行首  
CTRL+E 行尾  
CTRL+U 删除自光标到行首串  
CTRL+K 删除自光标到行尾串  
CTRL+L 清屏

