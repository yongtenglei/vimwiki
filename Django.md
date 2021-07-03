# Django learning log
### 创建虚拟环境
```python
python -m venv ll_env
```

### 激活环境
```python
source ll_env/bin/activate
```

### 退出环境
```python
deactivate
```

### 创建Django项目
```python
django-admin startproject Pizzeria .
```

### 创建数据库
In Pizzeria
```python
python manage.py migrate
```

### 查看/启动项目
```python
python manage.py runserver
```

### 创建应用程序
创建一个pizzas程序,其中包含Pizza模型,存储披萨名称Hawaiian和Meat Lovers. Topping模型,包含pizza和name,pizza是外键,name为配料pineapple、Canadian bacon和sausage.
```python
python manage.py startapp pizzas
```

### 定义模型
In pizzas
```python
models.py
from django.db import models
from django.db.models.deletion import CASCADE

# Create your models here.

class Pizza(models.Model):
    name = models.CharField(max_length=100)
    def __str__(self):
        return self.name

class Topping(models.Model):
    pizza = models.ForeignKey(Pizza, on_delete=CASCADE)
    name = models.TextField()
    class Meta():
        verbose_name_plural = 'ingredients'
    def __str__(self):
        if len(name) < 30:
            return self.name
        else:
            return f"{self.name[:30]}..."
```

### 激活模型
1. 添加model
In Pizzeria
```python
setting.py

# Application definition

INSTALLED_APPS = [
    # my app
    'pizzas',

    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

2. makemigrations
```python
python manage.py makemigrations pizzas
```

3. 迁移项目
```python
python manage.py migrate
```
### 管理网站
1. 创建超级用户
```python
python manage.py createsuperuser
```
2. 向管理网站注册模型
In pizzas
```python
admin.py

from django.contrib import admin

# Register your models here.

from .models import Pizza, Topping
admin.site.register(Pizza)
admin.site.register(Topping)
```
you can login website and make some pizza and add some ingredients

### Django shell
```python
python manage.py shell
```
using...

```python
from pizzas.models import Pizza
p = Pizza.objects.get(id = 1)
p.topping_set.all() # check foreign key Store for example, using t.store.set.all()
```

check all ingredients

### 创建页面
1. 映射URL
In Pizzeria

``` python
urls.py

from django.contrib import admin
from django.urls import path
from django.urls.conf import include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('pizzas.urls')),
]
```

In pizzas
```python
urls.py

""" define URL pattern of pizzas """ 
from django.urls import path
from . import views
app_name = "pizzas"
urlpatterns = [path("", views.index, name='index'),]
```

2. 编写视图view
In pizzas
```python
views.py

from django.shortcuts import render

### Create your views here.

def index(request):
    """ home of pizzas """
    return render((request, "pizzas/index.html"))
```

3. 编写模板html
In pizzas, mkdir -p ./templates/pizzas

```html
index.html

<p>pizza order and making</p>
<p>make or order A pizza what you like, add ingredient in it!</p>
```
