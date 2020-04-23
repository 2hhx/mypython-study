# Jupyter Notebook更换主题

每次我们打开jupyter的时候都是一样的主题，一样的界面，不免有些单调，所以本文主要帮助大家修改jupyter的主题。

## 安装

首先，我们需要安装一个工具包

    
    
    pip install jupyterthemes

安装完之后我们就可以尝试这修改主题了

## 切换主题

cl options | arg | default  
---|---|---  
Usage help | -h | \--  
List Themes | -l | \--  
Theme Name to Install | -t | \--  
Code Font | -f | \--  
Code Font-Size | -fs | 11  
Notebook Font | -nf | \--  
Notebook Font Size | -nfs | 13  
Text/MD Cell Font | -tf | \--  
Text/MD Cell Fontsize | -tfs | 13  
Pandas DF Fontsize | -dfs | 9  
Output Area Fontsize | -ofs | 8.5  
Mathjax Fontsize (%) | -mathfs | 100  
Intro Page Margins | -m | auto  
Cell Width | -cellw | 980  
Line Height | -lineh | 170  
Cursor Width | -cursw | 2  
Cursor Color | -cursc | \--  
Alt Prompt Layout | -altp | \--  
Alt Markdown BG Color | -altmd | \--  
Alt Output BG Color | -altout | \--  
Style Vim NBExt* | -vim | \--  
Toolbar Visible | -T | \--  
Name & Logo Visible | -N | \--  
Kernel Logo Visible | -kl | \--  
Reset Default Theme | -r | \--  
Force Default Fonts | -dfonts | \--  
  
打开命令行工具

    
    
    > jt -l  # 列出可用的主题名称
    
    """
    Available Themes:
       chesterish
       grade3
       gruvboxd
       gruvboxl
       monokai
       oceans16
       onedork
       solarizedd
       solarizedl
    """

切换主题

> jt -t 主题名

恢复默认

> Jt -r

## 各主题样式

Oceans16

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011100028565-163174079.png)  

One dork

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011100042686-402151486.png)  
/div>

Chesterish

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011100059212-1234500601.png)  

Grade3

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011100150985-301786157.png)  

[GitHub项目](https://github.com/dunovank/jupyter-themes)

