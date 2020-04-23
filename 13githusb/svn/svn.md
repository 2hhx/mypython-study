# svn命令使用手册

1. 查看版本库下的文件和目录列表

```
svn list(ls) svn://***** --verbose
```

1. 查看当前分支从哪个支线创建而来

```
svn log -v -r 1:HEAD --limit 1 --stop-on-copy
```

1. 查看当前工作副本所在URL

```
svn  info
```

1. 切换分支

```
svn　switch　svn://***(新分支)

svn　switch　svn://***　svn://***本地目录当前分支

svn switch http://localhost/test/456 . //(原为123的分支)当前所在目录分支到localhost/test/456
```

1. 新建、删除分支标签

```
svn copy base_branch new_branch -m "make B branch"
svn rm （分支）URL   -m "commit log" 

svn cp . （tag）URL
svn rm （tag）URL -m "commit log"
```

1. 同步代码到最新

```
svn　update(up)
```

1. 查看当前工作副本的状态

```
svn status(st)
```

1. 加锁/解锁

```
svn　lock　-m　“加锁备注信息文本“　[--force]　文件名
svn　unlock　文件名
```

1. 合并分支

```
#查找创建分支时的版本号reversion
svn log [-q] --stop-on-copy

svn merge -r 分支版本号:HEAD 分支的URL   --dry-run   #HEAD为当前主干上的最新版本

svn merge url -r xxxx:yyyy ./  (将url指定的code的xxxx版本到yyyy版本，merge到本地（注意：该方式不包括xxxx版本！！）)

svn merge url -r HEAD ./

svn merge url -c xxxx ./         (把svn 版本号为xxxx的改动合到你的本地)


svn st | grep ^C      # 查找合并时的冲突文件，手工解决冲突
svn resolved filename # 告知svn冲突已解决
svn commit -m ""      # 提示合并后的版本


有效选项: 
  -r [--revision] ARG      : ARG (一些命令也接受ARG1:ARG2范围)
                             版本参数可以是如下之一: 
                                NUMBER       版本号
                                '{' DATE '}' 在指定时间以后的版本
                                'HEAD'       版本库中的最新版本
                                'BASE'       工作副本的基线版本
                                'COMMITTED'  最后提交或基线之前
                                'PREV'       COMMITTED的前一版本
  -c [--change] ARG        : 在ARG版本(如同 -r ARG-1:ARG)作的修改
                             如果ARG为负数则等价于 -r ARG:ARG-1
  -N [--non-recursive]     : 过时；尝试 --depth=files 或 --depth=immediates
  --depth ARG              : 受深度参数 ARG(“empty”，“files”，“immediates”，或“infinity”) 约束的操作
  -q [--quiet]             : 不打印信息，或只打印概要信息
  --force                  : 强制操作运行
  --dry-run                : 尝试操作但没有修改
  --diff3-cmd ARG          : 使用 ARG 作为合并命令
  --record-only            : 标记版本为已合并(使用 -r 参数)
  -x [--extensions] ARG    : 缺省: “-u”。当 Subversion 调用外部比较程序时，ARG 直接传给它。但是当
                             Subversion 使用缺省的内置比较实现，或者正
                             显示追溯时, ARG 可以是: 
                                -u (--unified):
                                   输出三行统一上下文。
                                -b (--ignore-space-change):
                                   忽略空白数量的修改。
                                -w (--ignore-all-space):
                                   忽略所有的空白。
                                --ignore-eol-style:
                                   忽略行尾样式的改变。                            -p (--show-c-function):
                                   在比较输出中显示 C 函数名称。
  --ignore-ancestry        : 合并时忽略原始信息
  --accept ARG             : 指定自动解决冲突动作
                            ('postpone', 'base', 'mine-conflict',
                             'theirs-conflict', 'mine-full', 'theirs-full',
                             'edit', 'launch')
  --reintegrate            : 批量合并所有源 URL 中未合并的修改
```

1. 提交修改

```
svn add test.php ＜－ 添加test.php
svn add *.php ＜－ 添加当前目录下所有的php文件
svn commit -m “添加我的测试用test.php“ test.php
```

1. 

**SVN 常用命令一览表**

https://www.kancloud.cn/i281151/svn/197160

http://blog.sina.com.cn/s/blog_567e650201012jmq.html

| **命令**                                                     | **功能**                                   | **使用格式**                                           |
| ------------------------------------------------------------ | ------------------------------------------ | ------------------------------------------------------ |
| **checkout**                                                 | 检出                                       | **svn co URL**                                         |
| **up**                                                       | 更新到当前URL的末端                        | **svn up**                                             |
|                                                              |                                            |                                                        |
| **switch**                                                   | 更新到某一tag/branch                       | **svn switch (tag/分支)URL**                           |
| **add**                                                      | 增加                                       | **svn add 文件名**                                     |
|                                                              |                                            |                                                        |
| **rm**                                                       | 删除文件                                   | **svn rm 文件名**                                      |
| 删除目录                                                     | **svn rm 目录名**                          |                                                        |
| **diff**                                                     | 与base版本（最后检出或者更新到的版本）对比 | **svn diff**                                           |
| 与版本库中最新版本对比                                       | **svn diff -r head**                       |                                                        |
| 当前工作副本，两个版本之间对比                               | **svn diff -r reversion1:reversion2**      |                                                        |
| 版本库中任意两个tag做对比                                    | **svn diff (tag1)URL (tag2)URL**           |                                                        |
| **ci**                                                       | 提交                                       | **svn ci -m "commit log"**                             |
| **log**                                                      | 查看当前工作副本log                        | **svn log**                                            |
|                                                              |                                            |                                                        |
| 只查看指定版本的log                                          | **svn log -r**                             |                                                        |
| 打印log所有附加信息                                          | **svn log -v**                             |                                                        |
| 查看当前tag/branch版本详情                                   | **svn log --stop-on-copy -v**              |                                                        |
| **info**                                                     | 查看当前工作副本所在URL                    | **svn info**                                           |
| **status**                                                   | 查看工作副本的状态                         | **svn st**                                             |
| 查看文件的taglist                                            | **svn命令不支持，可执行cs taglist**        |                                                        |
| **tag**                                                      | 新增tag                                    | **svn cp . （tag）URL**                                |
|                                                              |                                            |                                                        |
| 删除tag                                                      | **svn rm （tag）URL -m "commit log"**      |                                                        |
| 覆盖已经存在的tag                                            | **不支持**                                 |                                                        |
| **分支开发**                                                 | 创建branch                                 | **svn cp （基线版本）URL （分支）URL -m "commit log"** |
| 删除branch                                                   | **svn rm （分支）URL -m "commit log"**     |                                                        |
| 同步                                                         | **svn co （主干）URL**                     |                                                        |
| **cd ~/wc**                                                  |                                            |                                                        |
| **svn merge （主干）URL （待同步tag）URL**                   |                                            |                                                        |
| **svn ci -m "commit log"**                                   |                                            |                                                        |
| **svn cp （主干）URL （以_PD_BL_MAIN结尾的tag）URL -m"commit log"** |                                            |                                                        |
| 合并                                                         | **svn co （合并目标）URL**                 |                                                        |
| **cd ~/wc**                                                  |                                            |                                                        |
| **svn merge （基线版本tag）URL （上线tag）URL**              |                                            |                                                        |
| **svn ci -m "commit log"**                                   |                                            |                                                        |
| **svn cp （合并目标）URL （上线tag_MERGE_的tag对应）URL -m"commit log"** |                                            |                                                        |





# linux下svn命令大全

 

**1.svnadmin create path  创建一个新的版本库，（path为你想创建版本库的目录路径，如创建版本库目录为cellsms：svnadmin create/home/c7mon/svn/cellsms）。**

 

**2.svn mkdir URL  创建目录，向版本库新添加一个目录,(立即提交，所以需要日志信息)：如新增trunk目录：svn mkdir file:///home/c7mon/svn/cellsms/trunk -m "此目录的说明信息"。**

  **svn mkdir newdir 在工作拷贝下新建一个目录，如新增hello目录，即在工作拷贝(svn checkout数据的目录下)：svn mkdir hello。**

 

**3.svn import URL 向版本库导入数据，需要日志信息。如导入当前目录下wwm目录到版本库trunk目录中：svn import file:///home/c7mon/svn/cellsms/trunk/wwm-m "日志说明信息"**

 

**4.svn checkout URL 导出一个工作拷贝，cd到你要存放导出数据的目录，如导出wwm目录：svn checkout file:///home/c7mon/svn/cellsms/trunk/wwm，或 svn checkoutsvn://192.168.3.33/home/c7mon/svn/cellsms/trunk/wwm。svn://方式需要用户名和密码,（svn co为svn checkout简写）。**

 

**5.svn commit  提交工作拷贝的修改到版本库中，如对wwm下的文件进行修改后提交：svn commit -m "",""可以为空，最好加上日志说明。svn ci 为其简写。**

 

**6.svn add file/dir 新添加的文件或目录，此处dir为linux命令mkdir在工作拷贝下创建的目录，不是svn mkdir命令创建的目录，所以需要svn add预订添加。svn add需要在工作拷贝下执行，如在4中svn co出的工作拷贝目录wwm下新添加了hehe.c：svn add hehe.c。 然后执行svncommit提交到版本库。**

 

**7.svn copy URL URL 完全的服务器端拷贝，通常用在分支和标签。 如wwm项目完成后发布版本wwm_1.0:**                                                                                   

**svn copy file:///home/c7mon/svn/cellsms/trunk/wwm file:///home/c7mon/svn/cellsms/trunk/tags/wwm_1.0 -m "it's the wwm_1.0 foranhui" 。                      或  svncopy svn://192.168.3.33/home/c7mon/svn/cellsms/trunk/wwmsvn://192.168.3.33/home/c7mon/svn/cellsms/trunk/tags/wwm_1.0 -m "it's thewwm_1.0 for anhui"。（tags目录需提前在版本库中建好。）**

 

**如果wwm_1.0版本出现BUG，可以拷贝tags目录下的wwm_1.0到建好的branches目录下，然后svn co 出branches下的wwm_1.0进行修改。这样trunk目录下的wwm可以继续进行开发，而不会与branches下1.0版本的修改产生冲突。当branches下的wwm_1.0版本修复bug后，可以使用svn merge命令，将修改的部分合并到trunk下的wwm工程中。**

 

**8.svn list URL 显示path目录下的所有属于版本库的文件和目录(简写 svn ls)**

​      **如列出cellsms下目录信息：svn list svn://192.168.3.33/home/c7mon/svn/cellsms**

**9.svn info URL 显示本地或远程条目的信息。**

​       **如查看cellsms下信息：svn infosvn://192.168.3.33/home/c7mon/svn/cellsms**

**10.svn log URL 查看版本日志信息。**

​       **如查看cellsms下所有版本信息：svn log svn://192.168.3.33/home/c7mon/svn/cellsms**

 

**11.svn mergeURL@version URL 合并，应用两组源文件的差别到工作拷贝路径。**

**假设情景：trunk/smpp  trunk为smpp项目开发的主目录，当开发完成时，smpp被拷贝打标签到tags/smpp_1.0.0,这里smpp_1.0.0为发布版本。此时trunk下smpp继续进行smpp第二版本的开发，这时发现发布的smpp_1.0.0版本有BUG需要修改，于是拷贝tags下的smpp_1.0.0到分支目录branches/smpp_1.0.0进行修改。当改好smpp_1.0.0后，问题来了，我们此刻需要把修改好后的smpp_1.0.0合并到此时正在trunk目录下正在开发smpp中。**

**解决：**

**(1)一个URL，为起始状态，其后的@version，表示取版本号，这个version就是svn cp到branches成功之后的那个版本。第二个URL，为最终状态，既修改好的smpp_1.0.0的URL。 其实就是左边和右边做了 一个diff，应用到当前工作区目录，也就是trunk的工作拷贝。此时  $svn status 就可以看到变化了**

**(2)我们需要知道smpp_1.0.0被拷贝到branches目录时得版本号，命令为：**

**svnlog -stop-on-copy svn://192.168.3.33/home/c7mon/svn/cellsms/branches/**

**(3)此命令需要在合并到trunk的工作拷贝中执行：(可以加上--dry-run选项模拟merge，但不真正的去做)**

**Svn merge**

**svn://192.168.3.33/home/c7mon/svn/cellsms/branches/mscid_1.0.0@18svn://192.168.3.33/home/c7mon/svn/cellsms/branches/mscid_1.0.0**

 

**(4)svn commit 提交。（是在trunk的工作拷贝中）**

 

**12.svn delete path/URL从工作拷贝或版本库删除一个项目。**

​    **如想删除工作拷贝smpp下的main.c文件: svndelete main.c (需cd到smpp的工作拷贝的目录下)。然后 svn commit提交。**

​    **如想删除trunk/smpp/main.c文件：svn delete –m “delete the main.c of smpp”**

**Svn://192.168.3.33/home/c7mon/svn/cellsms/smpp/main.c(需要日志信息，即-m “”)。然后svn commit提交。**

 

**13.svn revert取消所有的本地编辑。**

**如12中，你执行了svn delete后，想取消其delete操作，那么你在svn commit提交之前执行本命令。例如：svn revert main.c。**

 

**14. svn update更新你的工作拷贝。**

​    **如你的工作拷贝为trunk下smpp，现在你想将修改的smpp_1.0.0的内容更新到你当前的工作拷贝中，你可以：svn update –rversion(version为smpp_1.0.0版本号)。如果你执行：svn update命令，则默认更新最新的版本内容到当前工作拷贝中。**

 

**15.svn status打印工作拷贝文件和目录的状态。**

​       **如打印当前工作拷贝的状态：svnstatus –u。**

 

**16.svn lock 锁定工作拷贝中文件，避免其他用户修改导致冲突。**

​       **如：svn lock main.c(需要在工作拷贝中执行)。**

​       **如果有其他用户已锁定，你可以强制加锁，需要选项 --force。**

**17. svn unlock 解除锁定操作，如svn unlock main.c。**

 

**18.svn help 执行此命令可以查看svn所有的子命令。**

  **svn 子命令 --help，查看子命令选项信息，如：svn lock - -help。**

 

 

 

**1、将文件checkout到本地目录** 

svn checkout path（path是服务器上的目录） 
例如：svn checkout svn://192.168.1.1/pro/domain 
简写：svn co 
**2、往版本库中添加新的文件** 
svn add file 
例如：svn add test.php(添加test.php) 
svn add *.php(添加当前目录下所有的php文件) 
**3、将改动的文件提交到版本库** 
svn commit -m “LogMessage“ [-N] [--no-unlock] PATH(如果选择了保持锁，就使用–no-unlock开关) 
例如：svn commit -m “add test file for my test“ test.php 
简写：svn ci 
**4、加锁/解锁** 
svn lock -m “LockMessage“ [--force] PATH 
例如：svn lock -m “lock test file“ test.php 
svn unlock PATH 
**5、更新到某个版本** 
svn update -r m path 
例如： 
svn update如果后面没有目录，默认将当前目录以及子目录下的所有文件都更新到最新版本。 
svn update -r 200 test.php(将版本库中的文件test.php还原到版本200) 
svn update test.php(更新，于版本库同步。如果在提交的时候提示过期的话，是因为冲突，需要先update，修改文件，然后清除svn resolved，最后再提交commit) 
简写：svn up 
**6、查看文件或者目录状态** 
1）svn status path（目录下的文件和子目录的状态，正常状态不显示） 
【?：不在svn的控制中；M：内容被修改；C：发生冲突；A：预定加入到版本库；K：被锁定】 
2）svn status -v path(显示文件和子目录状态) 
第一列保持相同，第二列显示工作版本号，第三和第四列显示最后一次修改的版本号和修改人。 
注：svn status、svn diff和 svn revert这三条命令在没有网络的情况下也可以执行的，原因是svn在本地的.svn中保留了本地版本的原始拷贝。 
简写：svn st 
**7、删除文件** 
svn delete path -m “delete test fle“ 
例如：svn delete svn://192.168.1.1/pro/domain/test.php -m “delete test file” 
或者直接svn delete test.php 然后再svn ci -m ‘delete test file‘，推荐使用这种 
简写：svn (del, remove, rm) 
**8、查看日志** 
svn log path 
例如：svn log test.php 显示这个文件的所有修改记录，及其版本号的变化 
**9、查看文件详细信息** 
svn info path 
例如：svn info test.php 
**10、比较差异** 
svn diff path(将修改的文件与基础版本比较) 
例如：svn diff test.php 
svn diff -r m:n path(对版本m和版本n比较差异) 
例如：svn diff -r 200:201 test.php 
简写：svn di 
**11、将两个版本之间的差异合并到当前文件** 
svn merge -r m:n path 
例如：svn merge -r 200:205 test.php（将版本200与205之间的差异合并到当前文件，但是一般都会产生冲突，需要处理一下） 
**12、SVN 帮助** 
svn help 
svn help ci 
------------------------------------------------------------------------------
以上是常用命令，下面写几个不经常用的 
------------------------------------------------------------------------------
**13、版本库下的文件和目录列表** 
svn list path 
显示path目录下的所有属于版本库的文件和目录 
简写：svn ls 
**14、创建纳入版本控制下的新目录** 
svn mkdir: 创建纳入版本控制下的新目录。 
用法: 1、mkdir PATH… 
2、mkdir URL… 
创建版本控制的目录。 
1、每一个以工作副本 PATH 指定的目录，都会创建在本地端，并且加入新增 
调度，以待下一次的提交。 
2、每个以URL指定的目录，都会透过立即提交于仓库中创建。 
在这两个情况下，所有的中间目录都必须事先存在。 
**15、恢复本地修改** svn revert: 恢复原始未改变的工作副本文件 (恢复大部份的本地修改)。revert: 
用法: revert PATH… 
注意: 本子命令不会存取网络，并且会解除冲突的状况。但是它不会恢复 
被删除的目录 
**16、代码库URL变更** 
svn switch (sw): 更新工作副本至不同的URL。 
用法: 1、switch URL [PATH] 
2、switch –relocate FROM TO [PATH...] 
1、更新你的工作副本，映射到一个新的URL，其行为跟“svn update”很像，也会将 
服务器上文件与本地文件合并。这是将工作副本对应到同一仓库中某个分支或者标记的 
方法。 
2、改写工作副本的URL元数据，以反映单纯的URL上的改变。当仓库的根URL变动 
(比如方案名或是主机名称变动)，但是工作副本仍旧对映到同一仓库的同一目录时使用 
这个命令更新工作副本与仓库的对应关系。 
**17、解决冲突** 
svn resolved: 移除工作副本的目录或文件的“冲突”状态。 
用法: resolved PATH… 
注意: 本子命令不会依语法来解决冲突或是移除冲突标记；它只是移除冲突的 
相关文件，然后让 PATH 可以再次提交。 
**18、输出指定文件或URL的内容。** 
svn cat 目标[@版本]…如果指定了版本，将从指定的版本开始查找。 
svn cat -r PREV filename > filename (PREV 是上一版本,也可以写具体版本号,这样输出结果是可以提交的)