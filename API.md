# Introduction #

When creating your theme templates, certain php variables will be made available to you by Sweetcron.  This document tells you what those variables are.

# Generic Query #

Sometimes you might want to display specific content in a specific area.  For example, you might want your sidebar to always show the latest blog entry you've posted, or the last 10 youtube videos you've posted.  To make a generic query to the sweetcron database you can use

```
$this->sweetcron->query_items(TYPE, QUERY, OFFSET, LIMIT);
```

Parameters:

  * TYPE: site, tag or search
  * QUERY: the parameter to use to search against - can be a domain, a tag or a search keyword
  * OFFSET: defaults to 0
  * LIMIT: defaults to 10

Example:

```
$items = $this->sweetcron->query_items('site', 'youtube.com', 0, 10);
```

$items will then hold an array of the last 10 items Sweetcron has grabbed from youtube.com

You can loop through this array with a foreach loop, and each item will be a Sweetcron item object.  Example:

```
<?php $items = $this->sweetcron->query_items('site', 'youtube.com', 0, 10);
foreach ($items as $item): ?>
<a href="<?php echo $item->get_permalink()?>"><?php echo $item->get_image();?></a>
<?php endforeach; ?>
```

Would display the last 10 youtube videos in Sweetcron as thumbnail links.

Full API for Sweetcron item objects is explained further down this page.

# Config-Level Variables #

These variables enable you to get information about Sweetcron configuration such as the title of the lifestream etc.

### $this->config->item('base\_url') ###

Returns the full url of Sweetcron's base url

### $this->config->item('lifestream\_title') ###

Returns the title of the lifestream.

# Page-Level Variables #

These are variables that are available to individual theme pages.

### $page\_name ###

Returns the name of the current page (e.g. "Item Search" or "Items")

### $popular\_tags ###

An array of popular tags in your Sweetcron install.  Each tag object looks like this:

```
$tag->name //the human readable tag name
$tag->slug //the lowercase, alphanumeric slug to be used in tag searches
$tag->count //the total number of items with this tag
```

So you can use $popular\_tags to create a lis of popular tags in your theme templates, like this:

```
<?php foreach ($popular_tags as $tag): ?>
<?php echo $tag->name?>
<?php endforeach; ?>
```

### $tags ###

The same as above, except this array contains all available tags in your Sweetcron install.

### $items ###

An array of item objects for the current page, whether you are on the index page, an item search page, an item tag page etc etc.  See $item Object documentation below for full API reference.  Usage example:

```
<?php foreach ($items as $item): ?>
<?php echo $item->get_title()?>
<?php endforeach; ?>
```

# The $item Object #

Each $item object contains all available data about that particular item.  For example if it came from flickr, it might contain the url to a photo.  If it came from youtube, it might contain the embed code for a video.  It is your challenge as the theme developer to elegantly show the user what is available in this item, and you can do this with the following simple methods.

### $item->get\_feed\_id() ###
Returns the numeric feed id of the current item.

### $item->get\_feed\_title() ###
Returns the title of the feed of the current item.

### $item->get\_feed\_icon() ###
Returns the url to the icon of the feed of the current item.

### $item->get\_feed\_url() ###
Returns the url of the feed of the current item.

### $item->get\_feed\_data() ###
Returns an array of misc feed data.

### $item->get\_feed\_domain() ###
Returns the shorthand domain (e.g. `flick.com`) of the current item.

### $item->get\_feed\_class() ###
Returns the shorthand domain suitable for use as a class (e.g. `flickr_com`) of the current item.

### $item->get\_id() ###
Returns the numeric ID of the item.

### $item->get\_date() ###
Returns the unix timestamp of the current item.

### $item->get\_nice\_date() ###
Returns the relative date of the current item (e.g. `5 hours ago`).

### $item->get\_human\_date() ###
Returns the relative date of the current item if being viewed within the same day, otherwise returns a date stamp.

### $item->get\_content() ###
Returns the content of the item, pre formatted with html and any urls automatically turned into links.

### $item->get\_title() ###
Returns the title of the item.

### $item->get\_permalink() ###
Returns the link to the single item page on Sweetcron for this item.

### $item->get\_original\_permalink() ###
Returns the link to the origin of the item (i.e. link back to the original flickr photo)

### $item->get\_name() ###
Returns a url-safe version of the title (for use in SEO-style links if you like)

### $item->get\_data() ###
Returns an array of original item data.  This array contains all of the original feed content before Sweetcron changed any of it.  So if you need to access the "pure" item data from the original rss feed, you can get it here.  Do a print\_r() to see what's inside - it varies from item to item.

### $item->get\_video() ###
Returns a video if Sweetcron found one in the item.

### $item->get\_audio() ###
Returns audio if Sweetcron found any.

### $item->get\_image() ###
Returns the url of an image if Sweetcron found one in the item.

### $item->get\_tags() ###
Returns an array of tags for the item.

# $item Object conditionals #

### $item->has\_content() ###
Returns true if the item has content.

### $item->has\_permalink() ###
Returns true if the item has a permalink.

### $item->has\_original\_permalink() ###
Returns true if the item has a link back to the original item.

### $item->has\_video() ###
Returns true if Sweetcron managed to find a video in the item.

### $item->has\_audio() ###
Returns true if Sweetcron managed to find audio in the item.

### $item->has\_image() ###
Returns true if Sweetcron managed to find an image in the item.

### $item->has\_tags() ###
Returns true if the item has tags.

### $item->has\_tag('TAG') ###
Returns true if the item has a specific tag.

### $item->has\_data('key') ###
Returns true if the item has a specific piece of custom data.