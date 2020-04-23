# [DRF的分页](https://www.cnblogs.com/jiadi321/p/9892205.html)



**0**|**1****DRF的分页**

其实这个不说大家都知道，大家写项目的时候也是一定会用的，

我们数据库有几千万条数据，这些数据需要展示，我们不可能直接从数据库把数据全部读取出来，

这样会给内存造成特别大的压力，有可能还会内存溢出，所以我们希望一点一点的取，

那展示的时候也是一样的，总是要进行分页显示，我们之前自己都写过分页。

那么大家想一个问题，在数据量特别大的时候，我们的分页会越往后读取速度越慢，

当有一千万条数据，我要看最后一页的内容的时候，怎么能让我的查询速度变快。

DRF给我们提供了三种分页方式，我们看下他们都是什么样的~~

分页组件的使用

DRF提供的三种分页

 

```
from rest_framework.pagination import PageNumberPagination, LimitOffsetPagination, CursorPagination
```

 

全局配置

 

```
REST_FRAMEWORK = {
    'PAGE_SIZE': 2
}
```

 

#### 第一种 PageNumberPagination  看第n页，每页显示n条数据

http://127.0.0.1:8000/book?page=2&size=1

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
class MyPageNumber(PageNumberPagination):
    page_size = 2  # 每页显示多少条
    page_size_query_param = 'size'  # URL中每页显示条数的参数
    page_query_param = 'page'  # URL中页码的参数
    max_page_size = None  # 最大页码数限制
```

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)



```
class BookView(APIView):
    def get(self, request):
        book_list = Book.objects.all()
        # 分页
        page_obj = MyPageNumber()
        page_article = page_obj.paginate_queryset(queryset=book_list, request=request, view=self)
        
        ret = BookSerializer(page_article, many=True)
        return Response(ret.data)
```



![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)



```
class BookView(APIView):
    def get(self, request):
        book_list = Book.objects.all()
        # 分页
        page_obj = MyPageNumber()
        page_article = page_obj.paginate_queryset(queryset=book_list, request=request, view=self)

        ret = BookSerializer(page_article, many=True)
        # return Response(ret.data)
        # 返回带超链接 需返回的时候用内置的响应方法
        return page_obj.get_paginated_response(ret.data)
```



#### 第二种 LimitOffsetPagination 在第n个位置  向后查看n条数据

http://127.0.0.1:8000/book?offset=2&limit=1

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
class MyLimitOffset(LimitOffsetPagination):
    default_limit = 1
    limit_query_param = 'limit'
    offset_query_param = 'offset'
    max_limit = 999
```

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)



```
# 视图和上面的大体一致
# 只有用的分页类不同，其他都相同
class BookView(APIView):
    def get(self, request):
        book_list = Book.objects.all()
        # 分页
        page_obj = MyLimitOffset()
        page_article = page_obj.paginate_queryset(queryset=book_list, request=request, view=self)

        ret = BookSerializer(page_article, many=True)
        # return Response(ret.data)
        # 返回带超链接 需返回的时候用内置的响应方法
        return page_obj.get_paginated_response(ret.data)
```



#### 第三种 CursorPagination 加密游标的分页 把上一页和下一页的id记住

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
class MyCursorPagination(CursorPagination):
    cursor_query_param = 'cursor'
    page_size = 1
    ordering = '-id'
```

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)



```
class BookView(APIView):
    def get(self, request):
        book_list = Book.objects.all()
        # 分页
        page_obj = MyCursorPagination()
        page_article = page_obj.paginate_queryset(queryset=book_list, request=request, view=self)

        ret = BookSerializer(page_article, many=True)
        # return Response(ret.data)
        # 返回带超链接 需返回的时候用内置的响应方法
        return page_obj.get_paginated_response(ret.data)
```