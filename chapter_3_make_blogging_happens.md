# Chapter 3: Make Blogging Happens

This chapter comes to be longer, since the core of Django Blog Application will be developed and it gonna be operational.

## Design

Our Blog application will have a minimal features, not fancy and of course not overloaded.  
At its core it should comes with below features:

* Submitting Posts.
  * Drafting & Publishing.
  * Post as different Authors \(users\).
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
from django.contrib.auth.models import User
from django.utils.translation import ugettext_lazy as _
from django.db import models


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

    def __str__(self) -> str:
        """Post title as `Post` object representative."""
        return self.title

    def is_drafted(self) -> bool:
        """Returns True if the Post is drafted."""
        return self.status == self.STATUS_DRAFTED

    def is_published(self) -> str:
        """Returns True if the Post has been published."""
        return self.status == self.STATUS_PUBLISHED

    class Meta:
        verbose_name = _('Post')
        verbose_name_plural = _('Posts')
        get_latest_by = "id"
        ordering = ['-id', ]
```

Model `Post` is easy to read and follow.  
Let's look at some good practices in this code that help you write a better Django applications.

Look how we started our models to be [translation](https://docs.djangoproject.com/en/dev/topics/i18n/translation) ready for Django:

```python
from django.utils.translation import ugettext_lazy as _
```

I always make my projects translatable, even if I don't have any plans to add any translation in future. It's a good practice to follow while developing a software, applying it won't hurt anyone and most importantly not all of the world speaks or write English or your mother language.

Aliasing [`ugettext_lazy`](https://docs.djangoproject.com/en/dev/topics/i18n/translation/#lazy-translation) with `_` should be familiar to you, most of the other projects or languages use _underscore_ `_` to mark translatable strings.

Later on, you can run user `django-admin.py` to collect and compile translation files and make the internationalization and localization of your program much easier.

As keeping creation and modification of the posts, we have created two fields called:

* `created`: Will have a default value of creation `DateTime` by passing `auto_now_add` argument. Any time a post gets created, `created` default value would be set, after saving the `Post` instance it will never get updated.
* `updated`: Get the same default `DateTime` value as `created` field but will change to current date time whenever `Post` instance get modified and saved.

Both `created` and `updated` fields cannot not be modified from admin panel, because we have passed `editable` as argument with value of `False`.

After we have defined our Blog's Post required fields, we have created two helpers as well.

* `is_published`
* `is_drafted`

These two helpers will help us later in our code base to have our conditions in the code much easier. Our view templates are the ones that would benefit a lot as well as Post Admin change list.

## Admin

Now that we have our Post model ready, we need an interface to **C**reate/**R**ead/**U**pdate/**D**elete \(CRUD\) as an administrator.

Django comes with a powerful built-in Admin Framework to build an awesome admin panel right away with creating a simple file called `admin.py`, where we define how our Post model should be available to admin area.

> One of the most powerful parts of Django is the automatic admin interface. It reads metadata from your models to provide a quick, model-centric interface where trusted users can manage content on your site. The admin’s recommended use is limited to an organization’s internal management tool. It’s not intended for building your entire front end around. -- [The Django admin site](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#module-django.contrib.admin)

When we started the `blog` app with `./manage.py startapp blog` it should have created an empty file \(`blog/admin.py`\) for you.

Let's define blog `Post` model admin, in `admin.py`.

`blog/admin.py`:

```python
"""Blog Admin."""
from django.contrib import admin

from blog.models import Post


@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    """Post admin"""
    list_display = ('title', 'user', 'status', 'created', )
    list_filter = ('status', 'created', 'updated', )
    search_fields = ('title', 'content', )
    # TODO: pre=populate fields from title to slug
```

That's all it takes for our Post admin model.  
Let's go ahead and see what we have defined here.

* [`list_display`](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display): On the change list of the admin, where we see a list of all the posts, we'll be displaying some of the Post attributes such as: `('title', 'user', 'status', 'created', )`.
* [`list_filter`](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_filter): Filtering and Sorting the post list is another thing that Django Admin provides. Blog Post model has some fields that our suitable for filtering our data:
  * `status`: Can filter the Post list by `Draft` or `Published`.
  * `['updated', 'created']`: We'll be able to filter the data based on the last several days, months, and year.
* [`search_fields`](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/#django.contrib.admin.ModelAdmin.search_fields): Now a search field will appear on the change list that allows us search in our posts. If we look for a keyword or a text, it will search the given search phrase in `title` and `content` of all the posts.

## Views

The public face of blog has an logic behind it, Django calls it `Views` and has a comprehensive set of tools provided to make crafting HTTP requests pretty much easy.

`blog/views.py`:

```python
"""Blog Views."""
from django.views.generic import ListView, DetailView

from blog.models import Post


class PostListView(ListView):
    """
    Listing of all the posts.
    Paginating by 10 items per page.
    """
    model = Post
    paginate_by = 10
    context_object_name = 'posts'


class PostDetailView(DetailView):
    """Getting one Post based on its `slug`."""
    model = Post
    context_object_name = 'post'
```

There, that is for our blog views. Django Class Based Views makes it ridiculously simple to write. I write less code that does more, isn't amazing ?

View `PostListView` inherits from `django.views.generic.ListView`, we need to pass the model name, the rest is taken care of. I've added two other attributes such as `paginate_by` to limit the number of posts  that I want to list on the posts list view and `context_object_name` which we'll be using in the `post_list.html` template to access the post list.

View `PostDetailView` inherits from `django.views.generic.DetailView`, the defined attributes on this class are same as `PostListView`. The difference is, it will show a single post.

## URL Routing

> A clean, elegant URL scheme is an important detail in a high-quality Web application. Django lets you design URLs however you want, with no framework limitations.
>
> There’s no`.php`or`.cgi`required, and certainly none of that`0,2097,1-1-1928,00`nonsense.
>
> See [Cool URIs don’t change](http://www.w3.org/Provider/Style/URI), by World Wide Web creator Tim Berners-Lee, for excellent arguments on why URLs should be clean and usable.

When a user requests a page from your Django-powered site, this is the algorithm the system follows to determine which Python code to execute:

1. Django determines the root URLconf module to use. Ordinarily, this is the value of the [ROOT\_URLCONF ](https://docs.djangoproject.com/en/dev/ref/settings/#std:setting-ROOT_URLCONF)setting, but if the incoming `HttpRequest`object has a [urlconf ](https://docs.djangoproject.com/en/dev/ref/request-response/#django.http.HttpRequest.urlconf)attribute \(set by middleware\), its value will be used in place of the [ROOT\_URLCONF](https://docs.djangoproject.com/en/dev/ref/settings/#std:setting-ROOT_URLCONF) setting.
2. Django loads that Python module and looks for the variable `urlpatterns`. This should be a Python list of [django.conf.urls.url\(\)](https://docs.djangoproject.com/en/dev/ref/urls/#django.conf.urls.url) instances.
3. Django runs through each URL pattern, in order, and stops at the first one that matches the requested URL.
4. Once one of the regexes matches, Django imports and calls the given view, which is a simple Python function \(or a [class-based view](https://docs.djangoproject.com/en/dev/topics/class-based-views/)\). The view gets passed the following arguments:

   * An instance of [HttpRequest](https://docs.djangoproject.com/en/dev/ref/request-response/#django.http.HttpRequest).
   * If the matched regular expression returned no named groups, then the matches from the regular expression are provided as positional arguments.
   * The keyword arguments are made up of any named groups matched by the regular expression, overridden by any arguments specified in the optional **kwargs **argument to [django.conf.urls.url\(\)](https://docs.djangoproject.com/en/dev/ref/urls/#django.conf.urls.url).

5. If no regex matches, or if an exception is raised during any point in this process, Django invokes an appropriate error-handling view.



