# Chapter 1: Installing Python & Django 

Before going further and implement our beloved Blog in Django, we need to install Python and Django. Installing both is simple and requires not much of hassle.

In this book we'll be writing our web site in **Python 2** and **Django 1.9**.

Necessary tools to get up and running are:

* [Python 2](https://www.python.org/download/releases/2.7.2/)
* [PIP](https://pip.pypa.io/en/stable/)
* [Django 1.9](https://www.djangoproject.com/)

## GNU/Linux & Mac OSX

Both GNU/Linux and Mac OSX come with Python installed as default, be sure to have Python 2 installed.

To know which version of Python you have installed on your machine, you can pass `-V` parameter to python.

On my machine which is ArchLinux 64 bit, running `python -V` would print:

```
[alireza@arch ~]$ python -V
Python 3.5.1
```
That indicates I'm using Python 3 by default on my machine, if you're in the same situation you might have Python 2 installed as well.

You can can find out how many python version you have on your machine by looking it with `whereis` command:

```
[alireza@arch ~]$ whereis python
python: /usr/bin/python /usr/bin/python3.5m /usr/bin/python3.5m-config /usr/bin/python2.7-config /usr/bin/python3.5 /usr/bin/python3.5-config /usr/bin/python2.7 /usr/lib/python3.5 /usr/lib/python2.7 /usr/include/python3.5m /usr/include/python2.7 /usr/share/man/man1/python.1.gz
```

There, I have `python2` as well.
Let's pass `-V` to python again and see:
```
[alireza@arch ~]$ python2 -V
Python 2.7.11
```

Yep, `python2` is the binary that I'm looking for.

If you don't have required Python version you can install it by using your OS package manager.

Next would be installing PIP.

### PIP

Installing PIP is easy, its official documentation have done a great job to tell you how to install it.

Head over to:  
https://pip.pypa.io/en/stable/installing/#do-i-need-to-install-pip

While running:
```
python get-pip.py
```

Be sure to use the binary of Python 2, if your `python` command is linked to Python 3, then PIP will be installed for that Python version and will download and install the packages against it.