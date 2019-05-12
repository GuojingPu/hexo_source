---
title: django数据库查询related_name和related_query_name
categories:
  - 技术
  - 数据库
tags:
  - Django
comments: true
abbrlink: 47171
date: 2019-03-09 15:41:41
img:
---

### related_name和related_query_name：
有如下模型：

```
 class User(models.Model):
     username = models.CharField(max_length=20)
     password = models.CharField(max_length=100)

 class Article(models.Model):
     title = models.CharField(max_length=100)
     content = models.TextField()
     author = models.ForeignKey("User",on_delete=models.CASCADE)
```

##### related_name：
还是以User和Article为例来进行说明。如果一个article想要访问对应的作者，那么可以通过author来进行访问。但是如果有一个user对象，想要通过这个user对象获取所有的文章，该如何做呢？这时候可以通过user.article_set来访问，这个名字的规律是模型名字小写_set。示例代码如下：

```
user = User.objects.get(name='张三')
user.article_set.all()
```

如果不想使用模型名字小写_set的方式，想要使用其他的名字，那么可以在定义模型的时候指定related_name。示例代码如下：

```
class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    # 传递related_name参数，以后在方向引用的时候使用articles进行访问
    author = models.ForeignKey("User",on_delete=models.SET_NULL,null=True,related_name='articles')
```

以后在方向引用的时候。使用articles可以访问到这个作者的文章模型。示例代码如下：

```
user = User.objects.get(name='张三')
user.articles.all()
如果不想使用反向引用，那么可以指定related_name='+'。示例代码如下：

class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    # 传递related_name参数，以后在方向引用的时候使用articles进行访问
    author = models.ForeignKey("User",on_delete=models.SET_NULL,null=True,related_name='+')

```
以后将不能通过user.article_set来访问文章模型了。

####related_query_name：
在查找数据的时候，可以使用filter进行过滤。使用filter过滤的时候，不仅仅可以指定本模型上的某个属性要满足什么条件，还可以指定相关联的模型满足什么属性。比如现在想要获取写过标题为abc的所有用户，那么可以这样写：

```
users = User.objects.filter(article__title='abc')

```
如果你设置了related_name为articles，因为反转的过滤器的名字将使用related_name的名字，那么上例代码将改成如下：

```
users = User.objects.filter(articles__title='abc')

```
可以通过related_query_name将查询的反转名字修改成其他的名字。比如article。示例代码如下：

```
class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    # 传递related_name参数，以后在方向引用的时候使用articles进行访问
    author = models.ForeignKey("User",on_delete=models.SET_NULL,null=True,related_name='articles',related_query_name='article')
```

那么在做反向过滤查找的时候就可以使用以下代码：

```
users = User.objects.filter(article__title='abc')

```


