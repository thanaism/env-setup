# environment setup

## setup virtual env

```bash
$ python3 -m venv myvenv
```

to enter;

```bash
$ source myvenv/bin/activate
```

if you failed;

```bash
$ . myvenv/bin/activate
```

## install Django

In venv, you can use python instead python3 coz venv automatically recognize your python version.

```bash
(myvenv) ~$ python -m pip install --upgrade pip
touch requirements.txt
```

```bash
# requirements.txt
Django~=2.2.4
```

```bash
$ pip install -r requirements.txt
```

## create a new project

```bash
$ django-admin startproject mysite .
```

## edit settiing.py

```py
TIME_ZONE = 'Asia/Tokyo'
LANGUAGE_CODE = 'ja'
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
ALLOWED_HOSTS = ['127.0.0.1', '.pythonanywhere.com', '.amazonaws.com']
```

## create a database

```bash
$ python manage.py migrate
```

## launch web-server

```bash
(myvenv) ~/djangogirls$ python manage.py runserver
```

## create app

```bash
(myvenv) ~/djangogirls$ python manage.py startapp blog
```

inform django to use new app;

```py
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

```bash
(myvenv) ~/djangogirls$ python manage.py makemigrations blog
```

add django-created files to datebase;

```bash
python manage.py migrate blog
```

to crud;

```bash
$ open blog/admin.py
```

replace contents as below;

```py
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```
