# environment setup

## setup virtual env

```
$ python3 -m venv myvenv
```

to enter;

```
$ source myvenv/bin/activate
```

if you failed;

```
$ . myvenv/bin/activate
```

## install Django

In venv, you can use python instead python3 coz venv automatically recognize your python version.

```
(myvenv) ~$ python -m pip install --upgrade pip
touch requirements.txt
```

```
# requirements.txt
Django~=2.2.4
```

```
$ pip install -r requirements.txt
```

## create a new project

```
$ django-admin startproject mysite .
```

## edit settiing.py

```
TIME_ZONE = 'Asia/Tokyo'
LANGUAGE_CODE = 'ja'
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
ALLOWED_HOSTS = ['127.0.0.1', '.pythonanywhere.com', '.amazonaws.com']
```

## create a database

```
$ python manage.py migrate
```

## launch web-server

```
(myvenv) ~/djangogirls$ python manage.py runserver
```

## create app

```
(myvenv) ~/djangogirls$ python manage.py startapp blog
```

inform django to use new app;

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog.apps.BlogConfig',
]
```

add model to django;

```
(myvenv) ~/djangogirls$ python manage.py makemigrations blog
```

add django-created files to datebase;

```
python manage.py migrate blog
```

to crud;

```
$ open blog/admin.py
```

replace contents as below;

```
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```
