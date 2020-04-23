# robots.txt  协议

1. Robits Exclusion Standard网络爬虫排除标准

2. 作用：网站告知网络爬虫那些页面可以爬取，那些不行。

3. 形式：在网站根目录下的robots.txt文件

   百度的robots协议： 	https://www.baidu.com/robots.txt

   ```python
   # 例子：
   
   
   User-agent: Baiduspider   #(百度爬虫引擎)允许那些用户爬取
   Allow: /article       #允许访#代表运行爬取的内容问/article.html、/article/12345.com
   Disallow: /baidu      # 不允许爬取的内容
   Disallow: /s?		# 不允许爬取的内容
   Disallow: /ulink?	# 不允许爬取的内容
   Disallow: /link?	# 不允许爬取的内容
   Disallow: /home/news/data/	# 不允许爬取的内容
   Disallow: /     # 禁止爬取除了Allow规定页面之外的其他任何界面。
   ```

   以Allow开头URL 链接的是可以进行爬取的内容，以Disallow开头的链接是不允许访问爬取的。

   如果没有robots.txt文件，那么就可以爬取所有的内容。

   网站开发者对于网络爬虫的规范的公告,你可以不遵守`可能存在法律风险`,但尽量去遵守

   Robots协议:在网页的根目录+robots.txt

   Robots协议的基本语法:

   ```
   #注释,*代表所有,/代表根目录
   User-agent:* #user-agent代表来源
   Allow:/ 
   Disallow:/ #代表不可爬取的目录,如果是/后面没有写内容,便是其对应的访问者不可爬取所有内容
   并不是所有网站都有Robots协议
   ```

   如果一个网站不提供Robots协议,是说明这个网站对应所有爬虫没有限制

   ```
   类人行为`可以不参考robots协议,比如我们写的小程序访问量很少,内容也少`但是内容不能用于商业用途
   ```

   1. 

   

4. ![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190820211859413-1196782741.png)

   ![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190820211904377-384688803.png)

   

总的来说请准守Robots协议