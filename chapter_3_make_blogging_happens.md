# Chapter 3: Make Blogging Happens

This chapter comes to be longer, since the core of Django Blog Application will be developed and it gonna be operational.

## Design

Our Blog application will have a minimal features, not fancy and of course not overloaded.
At its core it should comes with below features:

* Submitting Posts.
  * Drafting & Publishing.
  * Post as different Authors (users).
* An Index page to show all the post sorted by the newest.
* A Detail page to show individual Post content.
* User Friendly URLs.

Yes, that's a good way to design an application when working with Django or any kind of Web Framework.

An application should do one job and only one job and do that completely. That's why we don't have **comments** or **Visit Counts** or other features included in our Blog Application design.


Any extra features that can work independently from the blog app should get developed as an external app or package.

## Models

Let's define the database structure, in Django it should live in `models.py` file.

`blog/models.py`:
```
from __future__ import unicode_literals
from django.contrib.auth.models import User
from django.utils.encoding import python_2_unicode_compatible
from django.utils.translation import gettext as _
from django.db import models


@python_2_unicode_compatible
class Post(models.Model):
    STATUS_DRAFTED = 1
    STATUS_PUBLISHED = 2

    STATUS_CHOICES = (
        (STATUS_DRAFTED, _('Drafted')),
        (STATUS_PUBLISHED, _('Published')),
    )

    title = models.CharField(verbose_name=_('title'), max_length=155)
    slug = models.SlugField(verbose_name=_('slug'))
    content = models.TextField(verbose_name=_('content'), max_length=10000)
    user = models.ForeignKey(verbose_name=_('user'), to=User)
    status = models.CharField(
        verbose_name=_('status'),
        choices=STATUS_CHOICES,
        default=STATUS_DRAFTED
    )

    def __str__(self):
        """
        Post title as `Post` object representative.

        :returns: Post's title
        :rtype: str
        """
        return self.title

    def is_drafted(self):
        """
        :rtype: bool
        """
        return self.status == self.STATUS_DRAFTED

    def is_published(self):
        """
        :rtype: bool
        """
        return self.status == self.STATUS_PUBLISHED
```

Model `Post` is easy to read and follow.
Let's look at some good practices in this code that help you write a better Django application.

Since we're writing our code in Python 2 while using Django it's better to use:
```
from django.utils.encoding import python_2_unicode_compatible
```
Decorator `python_2_unicode_compatible` makes models with `__str__` method compatible to work with `unicode` data as well, in earlier versions of Django we would write make a method called `__unicode__` that would work with `unicode` strings as well.

Look how we started our models to be translation ready for Django, simply by:
```
from django.utils.translation import gettext as _
```

