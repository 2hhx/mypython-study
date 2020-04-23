![img](https://img.php.cn/upload/article/000/000/037/5d452e28022cf816.jpg)

# 在注册表中彻底清除navicat

1.关闭Navicat程序。

2.键盘按键win+R，输入regedit调出Windows系统注册表。

3.删除HKEY_CURRENT_USER\SOFTWARE\PremiumSoft\Data：

4.展开HKEY_CURRENT_USER\SOFTWARE\Classes\CLSID，然后展开每一个子文件夹查看，如果里面只包含一个名为Info的文件夹，就删掉它：

# 步骤

1、找到控制面板,找到Navicat，点击卸载。

![1564814512(1).png](https://img.php.cn/upload/image/358/206/350/1564814662529694.png)

2、卸载残留

我是下载了一个Everything(搜索工具)用来全局搜索，把相关Navicat的东西全部删除掉。

下载地址:https://www.voidtools.com/zh-cn/

![1564814530(1).png](https://img.php.cn/upload/image/771/652/326/1564814683254280.png)

相关推荐：《[Navicat for mysql使用图文教程](https://www.php.cn/tool/navicat/)》

注：尤其是在navicat的默认安装目录下C:\Program Files\PremiumSoft的Navicat删除掉

![1564814555(1).png](https://img.php.cn/upload/image/281/545/196/1564814738388148.png)

3、注册表删除

Win+R 之后输入：regedit 进入注册表

![1564814571(1).png](https://img.php.cn/upload/image/692/321/770/1564814756425999.png)

找到：计算机\HKEY_CURRENT_USER\Software\Data

![1564814587(1).png](https://img.php.cn/upload/image/690/632/321/1564814773975204.png)

找到HKEY_CURRENT_USER\SOFTWARE\Classes\CLSID

展开后，删除所有目录下的Info！不过一般在你卸载完Navicat之后这些Info文件也会被删除。

![1564814604(1).png](https://img.php.cn/upload/image/207/288/449/1564814793273643.png)

[来源https://www.php.cn/tool/navicat/427605.html](https://www.php.cn/tool/navicat/427605.html)

