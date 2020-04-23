[TOC]



# pycharm配置并启动vue项目

```
1) 用pycharm打开vue项目
2) 添加配置npm启动
```

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191114120350177-1923556983.png)

用pycharm打开vue项目后可能发现无法识别vue语法

## Pycharm配置支持vue语法

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191114120210308-912728450.png)
![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191114120212403-644311602.png)



**在重启之后就可以pycharm就可以识别vue语法了**



# pycharm打开.vue文件由于ESLint语法检查代码出现红色波浪线

## 方法1

重新建一个项目，不安装eslint这个插件

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191114194918113-1994450244.png)

## 方法2

**设置不使用 ESLint 语法检查**

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191114194856437-94088287.png)





## 方法3

通过ESLint中的fix eslint problems对.vue文件语法进行自动修复



选中要修复的.vue文件，也可以选中整个文件夹进行修复（修复时间受修复的文件数量影响，如果文件数量比较大，不建议修复，可以设置ESLint不验证语法）。

![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191114194859033-1537352638.png)

# vue项目中js文件报红的处理



![1573730368853](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/1573730368853.png)

根据提示js的配置进行修改