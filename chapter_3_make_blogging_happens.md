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
```python
"""Blog models."""
from __future__ import unicode_literals
from django.contrib.auth.models import User
from django.utils.encoding import python_2_unicode_compatible
from django.utils.translation import ugettext_lazy as _
from django.db import models


@python_2_unicode_compatible
class Post(models.Model):
    """Blog Post model."""
    STATUS_DRAFTED = 1
    STATUS_PUBLISHED = 2

    STATUS_CHOICES = (
        (STATUS_DRAFTED, _('Drafted')),
        (STATUS_PUBLISHED, _('Published')),
    )

    title = models.CharField(verbose_name=_('title'), max_length=155)
    slug = models.SlugField(verbose_name=_('slug'), unique=True)
    content = models.TextField(verbose_name=_('content'), max_length=10000)
    user = models.ForeignKey(verbose_name=_('user'), to=User)
    status = models.CharField(
        verbose_name=_('status'),
        choices=STATUS_CHOICES,
        default=STATUS_DRAFTED
    )
    created = models.DateTimeField(
        verbose_name=_("created"),
        auto_now_add=True,
        editable=False
    )
    updated = models.DateTimeField(
        verbose_name=_('updated'),
        auto_now=True,
        editable=False,
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

    class Meta:
        verbose_name = _('post')
        verbose_name_plural = _('posts')
        get_latest_by = "id"
        ordering = ['-id', ]
```

Model `Post` is easy to read and follow.
Let's look at some good practices in this code that help you write a better Django applications.

Since we're writing our code in Python 2 while using Django it's better to use:
```python
from django.utils.encoding import python_2_unicode_compatible
```
Decorator `python_2_unicode_compatible` makes models with `__str__` method compatible to work with `unicode` data as well, in earlier versions of Django we would write make a method called `__unicode__` that would work with `unicode` strings as well.

Look how we started our models to be [translation](https://docs.djangoproject.com/en/dev/topics/i18n/translation) ready for Django:
```python
from django.utils.translation import ugettext_lazy as _
```
I always make my projects translatable, even if I don't have any plans to add any translation in future. It's a good practice to follow while developing a software, applying it won't hurt anyone and most importantly not all of the world speaks or write English or your mother language. 

Aliasing [`ugettext_lazy`](https://docs.djangoproject.com/en/dev/topics/i18n/translation/#lazy-translation) with `_` should be familiar to you, most of the other projects or languages use *underscore* `_` to mark translatable strings.

Later on, you can run user `django-admin.py` to collect and compile translation files and make the internationalization and localization of your program much easier.

As keeping creation and modification of the posts, we have created two fields called:

* `created`: Will have a default value of creation `DateTime` by passing `auto_now_add` argument. So any time a post gets created, `created` default value would be set, after saving the `Post` instance it will never get updated.
* `updated`: Get the same default `DateTime` value as `created` field but will change to current date time whenever `Post` instance get modified and saved.

Both `created` and `updated` fields cannot not be modified from admin panel, because we have passed `editable` as argument with value of `False`.

After we have defined our Blog's Post required fields, we have created two helpers as well.

* `is_published`
* `is_drafted`


These two helpers will help us later in our code base to have our conditions in the code much easier. Our view templates are the ones that would benefit a lot as well as Post Admin change list.

## Admin

Now that we have our Post model ready, we need an interface to **C**reate/**R**ead/**U**pdate/**D**elete (CRUD) as an administrator.

Django comes with a powerful built-in Admin Framework to build an awesome admin panel right away with creating a simple file called `admin.py`, where we define how our Post model should be available to admin area. When starting `blog` app with `./manage.py startapp blog` it should have created an empty file for you.

Let's define blog `Post` model admin, in `admin.py`.

`blog/admin.py`:
```python
"""Blog Admin."""
from django.contrib import admin

from blog.models import Post


class PostAdmin(admin.ModelAdmin):
    """Post admin"""
    list_display = ('title', 'user', 'status', 'created', )
    list_filter = ('status', 'created', 'updated', )
    search_fields = ('title', 'content', )


admin.site.register(Post, PostAdmin)
```

That's all it takes for our Post admin model.
Let's go ahead and see what we have defined here.

* [`list_display`](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display): On the change list of the admin, where we see a list of all the posts, we'll be displaying some of the Post attributes such as: `('title', 'user', 'status', 'created', )`.
* [`list_filter`](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_filter): Filtering and Sorting the post list is another thing that Django Admin provides. Blog Post model has some fields that our suitable for filtering our data:
  * `status`: Can filter the Post list by `Draft` or `Published`.
  * `['updated', 'created']`: We'll be able to filter the data based on the last several days, months, and year.
* [`search_fields`](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.search_fields): Now a search field will appear on the change list that allows us search in our posts. If we look for a keyword or a text, it will search the given search phrase in `title` and `content` of all the posts.


## Views

The public face of blog has an logic behind it, Django calls it ``Views`` and has a comprehensive set of tools provided to make crafting HTTP requests pretty much easy.

`blog/views.py`:
```python
"""Blog Views."""
from django.views.generic import ListView, DetailView

from blog.models import Post


class PostListView(ListView):
    """
    Listing of all the posts.
    Paginating by 10 items per page.
    
    :class: PostListView
    """
    model = Post
    paginate_by = 10
    context_object_name = 'posts'


class PostDetailView(DetailView):
    """
    Getting one Post based on its `slug`.
    
    :class: PostDetailView
    """
    model = Post
    context_object_name = 'post'
```

There, that is for our blog views. Django Class Based Views makes it ridiculously simple to write. I write less code that does more, isn't amazing ?

View `PostListView` inherits from `django.views.generic.ListView`, we need to pass the model name, the rest is taken care of. I've added two other attributes such as `paginate_by` to limit the number of posts  that I want to list on the posts list view and `context_object_name` which we'll be using in the `post_list.html` template to access the post list.

View `PostDetailView` inherits from `django.views.generic.DetailView`, the defined attributes on this class are same as `PostListView`. The difference is it will show a single post.

