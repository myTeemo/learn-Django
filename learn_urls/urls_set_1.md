# Django中url配置及传递参数


平时我们在浏览器中传递参数一般有两种：  

```shell
1. /127.0.0.1:8000/Blog/index?id=123&name=Tom
2. /127.0.0.1:8000/Blog/index/id/12/
```  

1.第一种方式

```python
# views.py 

def index(request):
    if request.method == 'GET':
        id = request.GET.get('id')
        name = request.GET.get('name')
        t = loader.get_template('index.html')
        c = Context({'id': id, 'name': name})
        return HttpResponse(t.render(c))
```

```python
# Teemo/urls.py

from django.conf.urls import url
from django.contrib import admin
from django.conf.urls import include

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^Blog/',include('Blog.urls')),
]


# Blog/urls.py

from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'index',views.index),
]

```

> 浏览器输入: http://127.0.0.1:8000/Blog/index?id=123&name=Jack  
> 根据在views中的设置，就可以获取相应的值，可以通过控制台打印或者模板中进行输出  


2.第二种方式

```python
# views.py

def show(request,arg1,arg2):
    t = loader.get_template('index.html')
    c = Context({'id': arg1, 'name': arg2})
    return HttpResponse(t.render(c))
	# arg1和arg2获取参数值
```  

```python
# Blog/urls.py 新增

urlpatterns = [
    url(r'index',views.index),
    url(r'show/(\d{4})/(\w{4})',views.show)
]
# 上述()内为需要传递的的参数,
# 第一个参数为长度是4的数字，第二个参数是长度为4的字符
```  
>浏览器输入: http://127.0.0.1:8000/Blog/show/1234/abcd  
>根据views中的函数设置，可以获取相应的值，可以通过控制台或者模板进行输出  

 3.指定参数名称(?P<>)
 
```python
	# Blog/urls.py 新增
    url(r'foo/(?P<id>\d{4})/(?P<name>\w{4})', views.foo),
    
   # views.py 中的foo()定义，参数必须为id和name。要和urls.py里面定义的一样
   def foo(request, id, name)
```  

>以上介绍了两种在浏览器中传递参数的方法，同时介绍了三种方式获取其中传进来的值。