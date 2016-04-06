# Chapter 2: Starting the Project

In this chapter we're going to star our Django Blog project and do the basic configuration so we can get to the development in next chapters.

Using `django-admin.py`, we can start our project directory and its default settings right away. Let's go ahead and fire it up and see the result:

```
$ django-admin.py startproject django_blog && cd django_blog
```
If you have `tree` installed on your machine, it should output the following result when running it inside of `django_blog` directory:
```
$ tree 
.
├── django_blog
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py

1 directory, 5 files
```

Awesome!

At the moment there's nothing to touch or modify, we can just keep the current files and their structure as they are.

What we have now, is a **Django Project** that is responsible for our *settings* and holding the main *urls* and a script called `manage.py` that is required to run Django server and lots of other things.

While you're in `django_blog` directory, you can see how many usefull commands are available through `manage.py`, let's try and see what kind of options we have:

```
$ python manage.py 

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


