# 扩展Django默认USER  

- 使用AbstractUser进行扩展  

```python 
	# -*- coding:utf-8 -*-
	
	from django.contrib.auth.models import AbstarctUser
	
	
	# user/models/UserProfile
	class UserProfile(AbstractUser):
		nick_name = models.CharField(max_length = 50,verbose_name =  u'昵称',default = u'')
		birthday = models.DateField(verbose_name = u'生日',null = True,blank = True)
		gender = models.CharField(max_length = 7,choices = (('male',u'男'),('female',u'女')),default = 'female')
		image = models.ImageField(upload_to = 'image/%Y/%m',default = 'image/default.png',max_length = 100)
		
		class Meta:
			verbose_name = u'用户信息'
			verbose_name_plural = verbose_name
		
		def __unicode(self):
			return self.username	
```

在使用ImageField字段的时候，要安装Pillow库

扩展完毕后，我们需要修改settings.py文件，告诉系统，我们要使用的models

在settings.py 文件中添加:
```python
	# 填写相应的models 使用app+class ,
	AUTH_USER_MODEL = 'users.UserProfile'
```