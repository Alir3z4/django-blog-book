# Chapter 2: Starting the Project

In this chapter we're going to star our Django Blog project and do the basic configuration so we can get to the development in next chapters.

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

## Summary

In this chapter we have started our project and created our single `blog`.  
Now, we have all the things we need to start developing our Django Blog.

