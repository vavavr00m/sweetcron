# Where are the themes? #

here -> `system/application/views/themes`

# Copy the Sandbox Theme #

The best method for creating a new theme is to make a duplicate of the `sandbox` folder inside `system/application/views/themes` and to rename the duplicate to your desired theme name.  Folder names cannot contain spaces.  Once you create a new folder in the themes folder, it becomes available as a selectable theme in the admin panel.  Use your new theme to edit and experiment with.

# Theme Contents #

A theme folder should always contain these components:

```
_activity_feed.php //the main activity list of your theme 
_header.php //the header of your site 
_footer.php //the footer of your site 
_sidebar.php //the sidebar of your site 
home.php //the index page of your site
items.php //the item page of your site (search / tag results)
single.php //the single item view page
rss_feed.php //formatting for the rss feed
main.css //the css for your site
```

Sweetcron allows for a lot of customisation beyond this.  You can put any files you want into your theme folder and include them like you would using regular php / html in your theme templates.  For example, the sandbox theme also contains an images folder.

# The Item Loop #

View `_activity_feed.php` for an example of the item loop.  For your site's index page and item pages, an `$items` object is provided to the theme templates.  You can use this to loop through available items like so:

```
<!-- some html -->
<?php foreach ($items as $item): ?>
<?php echo $item->get_title()?>
<?php endforeach; ?>
<!-- some html -->
```

### For the full API reference detailing what you can do with the $item objects and other variables that are available to theme files, please see the API page. ###
http://code.google.com/p/sweetcron/wiki/API

# Creating different styles for different types of items #

Is very easy.  To understand this fully, you will need to read the API documentation:
http://code.google.com/p/sweetcron/wiki/API

Here's one basic method you could try.  Loop through your items like so:

```
<?php foreach ($items as $item): ?>
<div class="<?php echo $item->get_feed_class()?>">

<?php echo $item->get_title()?>

</div>
<?php endforeach; ?>
```

get\_feed\_class() will return the domain name of your item in a "css-safe" manner.  For example, youtube.com would become youtube\_com.  Then it would simply be up to you to create styles associated with the websites you are pulling data from, e.g.

```
div.youtube_com {
   background: red;
}

div.flickr_com {
   background: blue;
}
```

and so on.

When you want to get more specific, you can start using conditional tags.  For example:

```
<?php foreach ($items as $item): ?>
<div class="<?php echo $item->get_feed_class()?>">

<?php echo $item->get_title()?>

<?php if ($item->get_feed_domain() == 'youtube.com'): ?>
Hey! this is from youtube!
<?php echo $item->get_video()?>
<?php endif; ?>

<?php if ($item->get_feed_domain() == 'flickr.com'): ?>
Hey! this is from flickr!
<img src="<?php echo $item->image()?>" alt="" />
<?php endif; ?>

</div>
<?php endforeach; ?>
```

See the `_activity_feed.php` file in the sandbox and boxy but good themes for more examples.
But...most importantly...read the API!
http://code.google.com/p/sweetcron/wiki/API

The possibilities are endless once you learn how to control the API :)