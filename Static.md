# Introduction #

Sometimes you want static pages on your lifestream such as an "about" page with your personal info or a contact page with a contact form.

# It's very easy... #

First, find your theme folder.  If you are using the sandbox theme, this is located at:

`system/application/views/themes/sandbox`

If you want to create a static page called "About", simply create a file here called `about.php`.

If you want to create a static page called "Info About Me", simply create a file here called `info_about_me.php`.  Sweetcron will remove the underscores and capitalize the words when it displays your page title.

Your new pages will be viewable at:

`mysite.com/p/about`

and

`mysite.com/p/info_about_me`

Then it is simply a case of editing your new files!

# Reserved Names #

Be careful not to confuse Sweetcron - there are some "reserved" names that you cannot use for your static page.  This should be obvious as the files already exist in the theme folder:

```
_activity_feed.php
_footer.php
_header.php
_sidebar.php
home.php
items.php
rss_feed.php
single.php
```