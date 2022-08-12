# Creating a Landing Page in a Django Project

Creating a landing page is relatively simple and the directions are easy to follow! 

The first thing is start by creating a project in anaconda prompt. Keep in mind that you would ideally want to make an environment for the project with all necessary packages downloaded. To create a new environment, follow the How to Set Up a Virtual Environment documentation.

To start a project, run the following line:

```shell
django-admin startproject PROJNAME
```

After the project is created, go to the directory via the <code>cd</code> command. 

```shell
cd PROJNAME
```

Start up a server with the following line:

```shell
python manage.py runserver
```

After going to your local hosted webpage <code>127.0.0.1:8000</code> , a screen will show up with a rocket and a message saying "The install worked successfully! Congratulations!

This is not a landing page though. So, an "app" needs to be made to create dynamic web pages for your project.

Exit out of the server by CTRL-C, and then run the following line:

```shell
python manage.py startapp APPNAME
```

When the app is made, we need to add it to Django's <code>INSTALLED_APPS</code>. We write the app name 'APPNAME' at the end of the INSTALLED_APPS block in the project's settings.py file.

```shell
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'APPNAME',
]
```

## URLS
When a request comes to a server to open a page, Django needs to be able to find a "view", or web page that will show when the response is processed. This is done within urlpatterns, in the projects main directory; in our case, it is PROJNAME/urls.py.

In order to show the landing page on the local host and essentially on our main domain, we add the following code to APPNAME/urls.py, on our app urls.py file.

```shell
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

A path is what comes after a domain in a URL. For example, in the URL <code>https://www.google.com/search?q=pages</code>, the path would be <code>search?q=APPNAME</code>. Since we want our landing page to be on the <code>/</code> path, the first parameter we provide to the path method is <code>''</code>. The second argument is what view we need to render which is an <code>index</code> view in our case.

We also add our urlpatterns from our app into the project's urls.py file.

```shell
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('', include('APPNAME.urls')),
    path('admin/', admin.site.urls),
]
```
## The View

The index view now needs to be created, which is done in the views.py file for the app pages.

```shell
from django.shortcuts import render

def index(request):
    return render(request, 'APPNAME/index.html')
```

## The Template

We need a landing page to be created! This is where you can have fun with your HTML, CSS, and Bootstrap design of a page. You can look further into our Game24 code, where a fully developed process of this example is demonstrated.

## Migrating your Project and Creating a Superuser 

It is important to an initial migration, as this will set up your admin, auth, contenttypes, and sessions apps. These are built into Django when you initially start a project, but it is essential to migrate them over in order to not get into trouble later down the line when you want to implement login and logout features (more on that later).

Run the following lines:

``` shell
python manage.py makemigrations
python manage.py migrate 

```

This will run a bunch of migrations for the apps specified above. Once that passes through successfully, we can move on to creating a superuser. This is really handy to have as the superuser has access to the admin portal of your Django project, which allows you to have an unlimited amount of access into the way your models, forms, media, files, etc. are all stored in your project. 

To set up a superuser, run the following line:

``` shell
python manage.py createsuperuser
```

The console will prompt you to fill out a username, email address, and password twice. Once they are filled in, the console will notify you that <code>Superuser created successfully.</code>

From this point on, we are able to create login and logout responses in our app. This is super easy to do, so don't worry!

Firstly, we must create a login html page. The one I use is this:

``` shell
<div class="row">
    <div class="col">
        <div class="row">
            <div class="col">
                <div class="card">
                    <h1 class="text-center mt-4">Login to Your Account</h1>
                    <div class="card-body d-flex flex-column align-items-center">
                        {% if message %}
                            <div>{{ message }}</div>
                        {% endif %}
                        
                        <form action="{% url 'login' %}" class="text-center" method="post">
                            {% csrf_token %}
                            <div class="form-group">
                                <div class="mb-3">
                                    <input class="form-control" type="text" name="username" placeholder="Username">
                                </div>
                            </div>
                            <div class="form-group">
                                <div class="mb-3">
                                    <input class="form-control" type="password" name="password" placeholder="Password">
                                </div>
                            </div>
                            <input class="btn btn-primary" type="submit" value="Login">
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

In here, you can see a csrf token is used; this is to protect sensitive information, in this case, a username and password. There are multiple other levels of security that can be implemented.