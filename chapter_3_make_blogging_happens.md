# Chapter 3: Make Blogging Happens

This chapter comes to be longer, since the core of Django Blog Application will be developed and it gonna be operational.

## Design

Our Blog application will have a minimal features, not fancy and of course not overloaded.
At its core it should comes with below features:

* Submitting Posts.
  * Drafting & Publishing.
  * Post as different Authors (users).
* An Index page to show all of the post sorted by the newest.
* A Detail page to show individual Post content.
* User Friendly URLs.

Yes, that's a good way to design an application when working with Django or any kind of Web Framework.

An application should do one job and only one job. That's why we don't have **comments** or **Visit Counts** or other features included in our Blog Application design.


Any extra feature should that can work independenly from the blog app should get developed as an external app or package.
