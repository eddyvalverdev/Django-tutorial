# Writing your first Django app, part 1

Let’s learn by example.

Throughout this tutorial, we’ll walk you through the creation of a basic poll application.

It’ll consist of two parts:

A public site that lets people view polls and vote in them.

An admin site that lets you add, change, and delete polls.

We’ll assume you have Django installed already. You can tell Django is installed and which version by running the following command in a shell prompt (indicated by the $ prefix):

```console
...\> py -m django --version
```
If Django is installed, you should see the version of your installation. If it isn’t, you’ll get an error telling “No module named django”.

This tutorial is written for Django 5.1, which supports Python 3.10 and later

## Creating a project

From the command line, cd into a directory where you’d like to store your code and create a new directory named djangotutorial. (This directory name doesn’t matter to Django; you can rename it to anything you like.)

```console
...\> mkdir Django-tutorial
```

Then, run the following command to bootstrap a new Django project:

```console
...\> django-admin startproject mysite Django-tutorial
```

This will create a project called mysite inside the djangotutorial directory. If it didn’t work, see Problems running django-admin.

Let’s look at what startproject created:

```console
djangotutorial/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

These files are:

**manage.py**: A command-line utility that lets you interact with this Django project in various ways. You can read all the details about manage.py in django-admin and manage.py.

**mysite/**: A directory that is the actual Python package for your project. Its name is the Python package name you’ll need to use to import anything inside it (e.g. mysite.urls).

**mysite/__init__.py**: An empty file that tells Python that this directory should be considered a Python package. If you’re a Python beginner, read more about packages in the official Python docs.

**mysite/settings.py**: Settings/configuration for this Django project. Django settings will tell you all about how settings work.

**mysite/urls.py**: The URL declarations for this Django project; a “table of contents” of your Django-powered site. You can read more about URLs in URL dispatcher.

**mysite/asgi.py**: An entry-point for ASGI-compatible web servers to serve your project. See How to deploy with ASGI for more details.

**mysite/wsgi.py**: An entry-point for WSGI-compatible web servers to serve your project. See How to deploy with WSGI for more details.

## The development server

Let’s verify your Django project works. Change into the djangotutorial directory, if you haven’t already, and run the following commands:

```console
...\> py manage.py runserver
```

You’ll see the following output on the command line:
```console
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

November 11, 2024 - 15:50:53
Django version 5.1, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
## Creating the Polls app

To create your app, make sure you’re in the same directory as manage.py and type this command:

```console
...\> py manage.py startapp polls
```

That’ll create a directory polls, which is laid out like this:

```console
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

## Write your first view

Let’s write the first view. Open the file polls/views.py and put the following Python code in it:

```python
#polls/views.py
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

To define a URLconf for the polls app, create a file polls/urls.py with the following content:

```python
#polls/urls.py
from django.urls import path

from . import views

urlpatterns = [
    path("", views.index, name="index"),
]
```

Your app directory should now look like:

```console
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    urls.py
    views.py
```

The next step is to configure the global URLconf in the **mysite** project to include the URLconf defined in polls.urls. To do this, add an import for **django.urls.include** in **mysite/urls.py** and insert an **include()** in the **urlpatterns** list, so you have:

```python
#mysites/urls.py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path("polls/", include("polls.urls")),
    path("admin/", admin.site.urls),
]
```

You have now wired an index view into the URLconf. Verify it’s working with the following command:

```console
...\> py manage.py runserver
```