# Elasticsearch之-Django/Flask集成

## 一 elasticsearch-dsl

```python
#安装： pip3 install elasticsearch-dsl
#示例
from datetime import datetime
from elasticsearch_dsl import Document, Date, Nested, Boolean, \
    analyzer, InnerDoc, Completion, Keyword, Text

html_strip = analyzer('html_strip',
    tokenizer="standard",
    filter=["standard", "lowercase", "stop", "snowball"],
    char_filter=["html_strip"]
)

class Comment(InnerDoc):
    author = Text(fields={'raw': Keyword()})
    content = Text(analyzer='snowball')
    created_at = Date()

    def age(self):
        return datetime.now() - self.created_at

class Post(Document):
    title = Text()
    title_suggest = Completion()
    created_at = Date()
    published = Boolean()
    category = Text(
        analyzer=html_strip,
        fields={'raw': Keyword()}
    )

    comments = Nested(Comment)

    class Index:
        name = 'blog'

    def add_comment(self, author, content):
        self.comments.append(
          Comment(author=author, content=content, created_at=datetime.now()))

    def save(self, ** kwargs):
        self.created_at = datetime.now()
        return super().save(** kwargs)


```

## 二 django集成

```python
from datetime import datetime
from elasticsearch_dsl import Document, Date, Nested, Boolean,analyzer, InnerDoc, Completion, Keyword, Text,Integer

from elasticsearch_dsl.connections import connections
# 创建连接(在项目中先执行）
connections.create_connection(hosts=["localhost"])


class Article(Document):
    title = Text(analyzer='ik_max_word',fields={'title': Keyword()})#mapping,看官方文档
    author = Text()
    def __str__(self):
        return self.title+self.author

    class Index:
        name = 'myindex'

    # def save(self, ** kwargs):
    #     return super(Article, self).save(** kwargs)


if __name__ == '__main__':
    # Article.init()  # 创建映射
    # 保存数据
    # article = Article()
    # article.title = "测试 牛逼"
    # article.author = "chen"
    # article.save()  # 数据就保存了

    #查询数据
    # s=Article.search().filter('match', title="测试")
    # # # s = s.filter('match', title="测试")
    # # #
    # results = s.execute()
    # print(results)
    # print(results[0])

    #删除数据
    # s = Article.search()
    # s = s.filter('match', title="测试").delete()

    #修改数据
    s = Article().search()
    s = s.filter('match', title="测试")
    results = s.execute()
    print(results[0])
    results[0].title="xxx"
    results[0].save()

```



# Elasticsearch之-Django/Flask集成

## 一 elasticsearch-dsl

```python
#安装： pip3 install elasticsearch-dsl
#示例
from datetime import datetime
from elasticsearch_dsl import Document, Date, Nested, Boolean, \
    analyzer, InnerDoc, Completion, Keyword, Text

html_strip = analyzer('html_strip',
    tokenizer="standard",
    filter=["standard", "lowercase", "stop", "snowball"],
    char_filter=["html_strip"]
)

class Comment(InnerDoc):
    author = Text(fields={'raw': Keyword()})
    content = Text(analyzer='snowball')
    created_at = Date()

    def age(self):
        return datetime.now() - self.created_at

class Post(Document):
    title = Text()
    title_suggest = Completion()
    created_at = Date()
    published = Boolean()
    category = Text(
        analyzer=html_strip,
        fields={'raw': Keyword()}
    )

    comments = Nested(Comment)

    class Index:
        name = 'blog'

    def add_comment(self, author, content):
        self.comments.append(
          Comment(author=author, content=content, created_at=datetime.now()))

    def save(self, ** kwargs):
        self.created_at = datetime.now()
        return super().save(** kwargs)


```

## 二 django集成

```python
from datetime import datetime
from elasticsearch_dsl import DocType, Date, Integer, Keyword, Text
from elasticsearch_dsl.connections import connections
from elasticsearch_dsl import Completion
from elasticsearch_dsl.analysis import CustomAnalyzer as _CustomAnalyzer

# Define a default Elasticsearch client
connections.create_connection(hosts=['localhost'])

class CustomAnalyzer(_CustomAnalyzer):
    def get_analysis_definition(self):
        return {}

ik_analyzer = CustomAnalyzer('ik_max_word',filter=['lowercase'])

class Article(DocType):
    title_suggest = Completion(analyzer=ik_analyzer, search_analyzer=ik_analyzer)
    title = Text(analyzer='ik_max_word', search_analyzer="ik_max_word", fields={'title': Keyword()})
    id = Text()
    url = Text()
    front_image_url = Text()
    front_image_path = Text()
    create_date = Date()
    praise_nums = Integer()
    comment_nums = Integer()
    fav_nums = Integer()
    tags = Text(analyzer='ik_max_word', fields={'tags': Keyword()})
    content = Text(analyzer='ik_max_word')

    class Meta:
        index = 'lcv-search'
        doc_type = 'article'
    #
    # def save(self, ** kwargs):
    #     self.lines = len(self.body.split())
    #     return super(Article, self).save(** kwargs)

    # def is_published(self):
    #     return datetime.now() < self.published_from
if __name__=="__main__":
  	Article.init() #创建映射
    article=Article()
    article.title="测试测试"
    ...
    article.save() # 数据就保存了
```

```python

```

