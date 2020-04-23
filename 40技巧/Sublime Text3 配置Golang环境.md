# Sublime Text 3配置Go语言开发环境

[参考安装GOSublime：https://www.cnblogs.com/chengxuyuan326260/p/10095914.html](https://www.cnblogs.com/chengxuyuan326260/p/10095914.html)

[参考：https://www.cnblogs.com/rainight/p/10244748.html](https://www.cnblogs.com/rainight/p/10244748.html)

[参考github安装go：https://github.com/astaxie/build-web-application-with-golang/blob/master/zh/01.0.md](https://github.com/astaxie/build-web-application-with-golang/blob/master/zh/01.0.md)

1. 安装 Package Control

   打开Sublime Text，使用快捷键 ctrl+` （左上角Tab键上方，Esc键下方）或者使用菜单 View > Show Console menu，此时将出现Sublime Text的控制台，将如下代码分别放入执行(按回车)即可。

   ```
   import urllib.request,os,hashlib; h = 'df21e130d211cfc94d9b0905775a7c0f' + '1e3d39e33b79698005270310898eea76'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by) 
   ```

2. 安装完成会提示你重启软件，之后，你就可以发现在 Preferences 这个菜单下出现了菜单项 Package Control，大致如下图所示(我的Sublime Text3已经汉化了)：

   ![](https://img2020.cnblogs.com/blog/1746287/202004/1746287-20200403004258452-1481162555.png)

3. 安装 gosublime 插件，按住 Ctrl+Shift+p 会弹出一个会话框，在其中输入"install package"后安回车，会出现这样的画面：

   ![](https://img2020.cnblogs.com/blog/1746287/202004/1746287-20200403004534931-1065823894.png)输入"gosublime"，选中并回车，然后输入"go build"，选中并回车（可选）。

4. 配置gosublime插件

   首选项--->Package Settings--->GoSublime--->Settings - User

   打开之后如下图：

   ![](https://img2020.cnblogs.com/blog/1746287/202004/1746287-20200403005517443-1635604294.png)

   "GOPATH"是你的工程目录

   "GOROOT"是你安装Go的目录

5. 由于的我编译系统出了点小问题，运行go文件时还需要在输入一行命令，所以如果有和我一样的问题可以选择配置这一条

   工具--->编译系统--->新建编译系统

   输入一下代码：

   ```
   {
       "shell_cmd": "d:/Go/bin/go run $file"
   }
   ```

   保存文件，名字为Gorun.submlime-build

   然后选中刚刚保存的编译系统

   ![](https://img2020.cnblogs.com/blog/1746287/202004/1746287-20200403010143715-380419503.png)

   至此所有都配置完毕，测试一下：

   ![](https://img2020.cnblogs.com/blog/1746287/202004/1746287-20200403010355924-920011834.png)

   现在就可以愉快的敲代码了，Go！Go！Go！

## 代码提示

安装:golang Tools integration

Golang Tools Integration

```go
"gscomplete_enabled": true,

	// Whether or not gsfmt is enabled
	"fmt_enabled": false,

```





https://www.php.cn/tool/sublime/436436.html

# sublime自动补全

```
Preferences-&gt;Settings
在右面的settings-User添加上这句
{
	"ignored_packages":
	[
		"Vintage"
	],

	"auto_complete":true,
	"auto_match_enabled":true
}
```

