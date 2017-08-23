# Chapter 1: Installing Python & Django

Before going further and implement our beloved Blog in Django, we need to install Python and Django. Installing both is simple and requires not much of hassle.

In this book we'll be writing our web site in **Python 3** and **Django 1.11**.

Necessary tools to get up and running are:

* [Python 3](https://www.python.org/download/releases/3.0/)
* [PIP](https://pip.pypa.io/en/stable/)
* [Django 1.11](https://www.djangoproject.com/download/)

## GNU/Linux & Mac OSX

Both GNU/Linux and Mac OSX come with Python installed as default, be sure to have Python 3 installed.

To know which version of Python you have installed on your machine, you can pass `-V` parameter to python.

On my machine which is ArchLinux 64 bit, running `python -V` would print:

```
$ python -V
Python 3.6.2
```

That indicates I'm using Python 3 by default on my machine, if you get the same output, you got it all working,

If you don't have Python 3 version you can install it by using your OS package manager.

Next would be installing PIP.

### PIP

Installing PIP is easy, its official documentation have done a great job to tell you how to install it.

Head over to:  
[https://pip.pypa.io/en/stable/installing/\#do-i-need-to-install-pip](https://pip.pypa.io/en/stable/installing/#do-i-need-to-install-pip)

While running:

```
$ python get-pip.py
```

Be sure to use the binary of Python 2, if your `python` command is linked to Python 3, then PIP will be installed for that Python version and will download and install the packages against it.

### Django

We'll be using _pip_ to install Django package from PyPI.

```
$ pip install Django
```

To verify Django installation, let's check its version from command line:

```
$ django-admin.py --version
1.11.4
```

If you get anything else or an error, you might want to repeat the steps to make sure you didn't miss anything.

## Summary

In this chapter we have installed:

* Python: A great general purpose language that makes every one life easier.
* PIP: Python Package Manager that install.
* Django: The heavy lifting Python Web Framework.

That made our tools for developing our Django Blog ready.

