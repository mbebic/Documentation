# Creating a Landing Page in a Django Project

Creating a landing page is relatively simple and the directions are easy to follow! 

The first thing is start by creating a project in anaconda prompt. Keep in mind that you would ideally want to make an environment for the project with all necessary packages downloaded. To create a new environment, follow the How to Set Up a Virtual Environment documentation.

To start a project, run the following line:

```shell
django-admin startproject NAME
```

After the project is created, go to the directory via the <code>cd</code> command. 

```shell
cd NAME
```

Start up a server with the following line:

```shell
python manange.py runserver
```

After going to your local hosted webpage <code>127.0.0.1:8000</code> , a screen will show up with a rocket and a message saying "The install worked successfully! Congratulations!

This is not a landing page though. So, an "app" needs to be made to create dynamic web pages for your project.

Exit out of the server by CTRL-C, and then run the following line:

```shell
python manage.py startapp pages
```

When the app is made, we need to add it to Django's <code>INSTALLED_APPS</code>. We write the app name 'pages' at the end of the INSTALLED_APPS block in the project's settings.py file.

```shell
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'pages',
]
```

## URLS
When a request comes to a server to open a page, Django needs to be able to find a "view", or web page that will show when the response is processed. This is done within urlpatterns, in the projects main directory; in our case, it is NAME/urls.py.

In order to show the landing page on the local host and essentially on our main domain, we add the following code to pages/urls.py, on our app urls.py file.

```shell
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

A path is what comes after a domain in a URL. For example, in the URL <code>https://www.google.com/search?q=pages</code>, the path would be <code>search?q=pages</code>. Since we want our landing page to be on the <code>/</code> path, the first parameter we provide to the path method is <code>''</code>. The second argument is what view we need to render which is an <code>index</code> view in our case.

We also add our urlpatterns from our app into the project's urls.py file.

```shell
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('', include('pages.urls')),
    path('admin/', admin.site.urls),
]
```
## The View

The index view now needs to be created, which is done in the views.py file for the app pages.

```shell
from django.shortcuts import render

def index(request):
    return render(request, 'pages/index.html')
```

## The Template

We need a landing page to be created! This is where you can have fun with your HTML, CSS, and Bootstrap design of a page. You can look further into our Game24 code, where a fully developed process of this example is demonstrated.




