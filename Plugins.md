_This document is still pending completion!_

# Introduction #

Plugins are found in `system/application/plugins`

Plugins improve how Sweetcron imports data from different websites.

  * Example Problem: Sweetcron in its default state can only find thumbnail versions of photos I post to flickr.com
  * Example Solution: Using plugin architecture, it would be possible to write a flickr plugin for Sweetcron that grabs the thumbnail's larger version off the web and stores it in the $item object for use in theme templates.

On its own, Sweetcron is pretty good at pulling out things like images and tags from items that it imports.  However if we want Sweetcron to be able to understand more complex data from the websites we interact with, we need to teach it how.

**The challenge** is that all websites output their feeds in a siiiiightly different manner to each other.  So whilst - for example - youtube.com and vimeo.com both have videos in their feeds, they both put them _in_ their feeds in a slightly different way.

# Two Hooks #

We can manipulate feed data at two points.

  1. At import - before the item is put into the database.
  1. At display - before the item is rendered in a user's browser

At import is suitable for making tweaks to data that we know we want to be permanent.  At display is suitable for simple, non-processor-intensive tweaks that make small changes to the available data.

# Creating a plugin #

Your plugin should be named after the web service is it being built for, using the domain name and replacing the period mark with an underscore.  E.g.

  * youtube.com -> youtube\_com.php
  * flickr.com -> flickr\_com.php

Please see the plugins folder for more examples.  The classname should follow the filename.  E.g. youtube\_com.php looks like this:

```
<?php if (!defined('BASEPATH')) exit('No direct script access allowed');

class Youtube_com {
	
	//sample class for youtube
	
	function pre_db($item)
	{
		return $item;
	}
	
	function pre_display($item)
	{
		return $item;
	}

}
?>
```

Sweetcron will detect if a plugin exists or not for a feed that a user is trying to import items from, so there is no configuration on the user's side other than dropping the plugin file into the plugins folder.

# Plugin Functions #

You can manipulate item data at two points in an item's "lifetime" by using the pre\_db() and pre\_display() functions in your plugin.  In both cases, an item object is passed to the function by sweetcron.  Remember to return the item object back to Sweetcron after you're finished with it!

## When to use pre\_db() ##

pre\_db() should be used when you want to make "permanent" changes to data.  This might include stripping out unwanted data, or selectively setting certain items to "draft" status.

## When to use pre\_display() ##

pre\_display() should be used when you want to make final tweaks to data before they get displayed to the browser by sweetcron.  For example, the youtube.com plugin takes the permalink to the youtube video, figures out the embed code and stores it in the item's video array key $item->item\_data['video']

```
	function pre_display($item)
	{
		$link = $item->item_data['permalink'];
		$link = str_replace('?v=', '/v/', $link);
				
		$item->item_data['video'] = '<object width="212" height="178"><param name="movie" value="'.$link.'&hl=en&fs=1"></param><param name="allowFullScreen" value="true"></param><embed src="'.$link.'&hl=en&fs=1" type="application/x-shockwave-flash" allowfullscreen="true" width="212" height="178"></embed></object>';
		return $item;
	}
```

Note that we could do this at the pre\_db() stage instead, but if we did that we would be filling the database with lots of static html which is hard for the user to manipulate at a later stage.  So you have to think carefully about whether the goal you want to achieve with your plugin is best done at the pre\_db or pre\_display stage.

# Manipulating item attributes #

Is as simple as overwriting the existing attribute with something else that you want to save / display instead.  e.g.

```
function pre_display($item)
{
   $item->item_title = 'foo';
   return $item;
}
```

Available item attributes are:

```
$item->item_status //status of item.  "publish" by default, but you can set to "draft"
//IMPORTANT - only manipulate this at the pre_db stage if you're going to use it.
//changing the status on pre_display, in the words of Egon Spengler, would be bad.

$item->item_date  //date of the item in unix timestamp
$item->item_title //title of the item
$item->item_permalink //permalink back to the origin of the item
$item->item_content = //content of the item
$item->item_name = //url-safe name of the item, created from the item title
```

# Storing Custom Attributes #

Item objects also feature an `$item->item_data` array.  This can be used to store custom values.  Also, it contains extra information about the item so be careful you do not overwrite any of the preset keys (unless that is your intention).  Preset item\_data values:

```
$item->item_data['title'] //the raw title of the item before sweetcron cleaned it for use in $item->item_title
$item->item_data['permalink'] //raw permalink of item
$item->item_data['content'] //raw content
$item->item_data['enclosures'] //data that was found in the item's rss "enclosure" section
$item->item_data['categories'] //data that was found in the item's rss "categories" section	
$item->item_data['tags'] //tags that sweetcron was able to associate with the item						
$item->item_data['image'] //image that sweetcron was able to associate with the item
```

Some of this raw data is very useful.  For example, if sweetcron was unable to find tags for a particular feed, but you know they exist somewhere, you will probably be able to find them in the ['categories'] or ['enclosures'] key.  Then, you would do something like this:

```
function pre_db($item)
   //sweetcron was unable to find tags so we'll try to find them ourself...
   $tags = do_something_clever($item->item_data['categories']);
   $item->item_data['tags'] = $tags;
   return $item;
```

You can store completely custom data by simply adding to the item\_data array.

```
$item->item_data['foo'] = 'bar';
```

# Media Types #

Sweetcron's theme API has three functions for getting media:

get\_image()
get\_video()
get\_audio()

These functions look inside the item\_data array to see if there is anything set.  Sweetcron itself is usually pretty good at setting the item\_data['image'] content by itself, but for item\_data['video'] and item\_data['audio'] you will need to help it with a plugin.  In the case of the youtube plugin, item\_data['video'] is set by the plugin so the user can then echo out a video snippet in their theme by calling get\_video()

```
	function pre_display($item)
	{
		$link = $item->item_data['permalink'];
		$link = str_replace('?v=', '/v/', $link);
				
		$item->item_data['video'] = '<object width="212" height="178"><param name="movie" value="'.$link.'&hl=en&fs=1"></param><param name="allowFullScreen" value="true"></param><embed src="'.$link.'&hl=en&fs=1" type="application/x-shockwave-flash" allowfullscreen="true" width="212" height="178"></embed></object>';
		return $item;
	}
```