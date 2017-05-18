# Django 常用配置url方法

>在网站的搭建中，url的配置是非常重要的，一个好的url配置可以极大的方便网站的维护同时也方便阅读。  

在创建Django项目的时候，Django默认为我们创建了一个默认的urls.py文件。里面可以添加我们需要的url配置。

1. 最简单的url配置  
  >已创建好了一个叫Blog的应用  
  
  ```python
  # urls.py 新增
  	import Blog
  	
  	url(r'^Blog/index/$',Blog.urls.views.index),
  	
  ```

2. 改进版
	> 当我们的应用逐渐增多的时候，url的维护就变得相当困难,上述方法就变得有点相形见绌了。  
	  
- 第一种改进方法

```python
	import Blog
	
	urlpatterns = [Blog.urls.views,
			url(r'^Blog/index/$',index),
			url(r'^Blog/time/$',time),
	]
	
	urlpatterns += [article.urls.views,
			url(r'^article/index/$',index),
			url(r'^article/time/$',time),
	]

```

- 第二种改进方法

```python 
	# 在项目的urls.py里面新增如下配置
	
	from django.conf.urls import include


	urlpatterns =[
		url(r'^Blog/',include('Blog.urls')),
		url(r'^Article/',include('Article.urls')),
	]
	
	# 在Blog应用下新建urls.py文件，新增如下配置
	
	from django.conf.urls import url
	from . import views
	
	
	urlpatterns =[
		url(r'index',views.index),
	]
	

```

> 上面的url配置各有优缺点，具体的配置，还需要在项目的实践中，做相应取舍。