# Chapter 2: Starting the Project

In this chapter we're going to star our Django Blog project and do the basic configuration so we can get to the development in next chapters.

## Initializing Project

Using `django-admin.py`, we can start our project directory and its default settings right away. Let's go ahead and fire it up and see the result:

```
$ django-admin.py startproject django_blog && cd django_blog
```

If you have **tree** installed on your machine, it should output the following result when running it inside of `django_blog` directory:

```
$ tree 
.
├── django_blog
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py

1 directory, 5 files
```

Awesome!

At the moment there's nothing to touch or modify, we can just keep the current files and their structure as they are.

What we have now, is a **Django Project** that is responsible for our _settings_ and holding the main _urls_ and a script called `manage.py` that is required to run Django server and lots of other things.

While you're in `django_blog` directory, you can see how many useful commands are available through `manage.py`, let's try and see what kind of options we have:

```
$ ./manage.py

Type 'manage.py help <subcommand>' for help on a specific subcommand.

Available subcommands:

[auth]
    changepassword
    createsuperuser

[django]
    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations
    migrate
    sendtestemail
    shell
    showmigrations
    sqlflush
    sqlmigrate
    sqlsequencereset
    squashmigrations
    startapp
    startproject
    test
    testserver

[sessions]
    clearsessions

[staticfiles]
    collectstatic
    findstatic
    runserver
```

Lots of helpers, read about all of them at [django-admin and manage.py](https://docs.djangoproject.com/en/dev/ref/django-admin/) documentation page.

For now we need the command `startapp` to create our blog app.  
Let's create an app called `blog` which we will work on this app to create our blog logic and implementation:

```
./manage.py startapp blog
```

Run `tree` to see what we have:

```
$ tree blog/
blog/
├── admin.py
├── apps.py
├── __init__.py
├── migrations
│   └── __init__.py
├── models.py
├── tests.py
└── views.py

1 directory, 7 files
```

Well, done!

## Django application VS Django Project

If you have noticed, we first created a Django Project and withing that we started a Django application. Don't be confused about them, the flow is simple. 

A Django project is simply a Python package that keeps **settings**, **main URL configuration**, **WSGI application starting point **to feed into a web server such as \([gunicorn ](http://gunicorn.org/)or [uWSGI](https://uwsgi-docs.readthedocs.io/en/latest/)\) and finally a management file called [`manage.py`](https://docs.djangoproject.com/en/dev/ref/django-admin/) to run any built commands provided by Django on the project or a specific application.

A Django application is a python package as well, that gets created inside a Django project, an application should belong to a project. The logic of each application should be isolated inside itself and not the project. Each application should have a purpose and implement it. The good part is, other application can communicate and use each others functionalities as well. Just like how we import a module and use its classed, in Django we can import classes and other application and start using them.

In later chapters, we'll see how out blog application will use Django Auth application functionality for authentication, registration, creating posts.

We created a Django application called "blog" for publishing articles, later we can create another application such as "shop" that handles selling products and maybe another application named "metrics" that handle some stats such as how many visitors and posts have been published that can provide some sweet charts in a fancy dashboard. \(_Although in this book, we're going to create the blog part only._\)

## Dependency Management

We have Django installed, as of now the only direct dependency of our project is Django. Let's add it to `requirements.txt` file where we list all of project dependencies and their direct version.

In the root of the project directory a file named `requirements.txt` add Django:

```
Django==1.11.4
```

While working on any project, it's always good to keep `requirement.txt` file updated with any package we add to the project. All the CI and Build tools that I know, recognize this file and install all the package listed.

## Summary

In this chapter we have started our project and created our single `blog`.  
Now, we have all the things we need to start developing our Django Blog.

