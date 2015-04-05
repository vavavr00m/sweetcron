# Download #

Download the latest release of Sweetcron from the "Download" area:
http://code.google.com/p/sweetcron/downloads/list

# Requirements #

  * A web server with at least PHP5 and MySQL4.1
  * A web server with mod\_rewrite installed
  * Some rss feeds of your activity across the web

For those who do not know what the above server requirements mean, do not worry.  In all likelihood, your server supports them.  They are more or less standard components of any modern web server.

# Before You Start #

Create a new database on your server.  If you are unsure of how to do this, please contact your hosting provider.  Remember the username and password you set for your database as you will need it in the next step.

# Config & .htaccess #

Using FTP or a similar tool, copy the contents of the `sweetcron` folder to your web server.  This step-by-step guide assumes you want to install Sweetcron as your main site (in the root of your public folder).  If you would like to install Sweetcron in a subfolder instead, please see the notes after this guide.  **IMPORTANT: careful you don't miss the .htaccess file in the base of the folder**

If you are unable to locate the .htaccess file (it should be sitting right next to index.php, but most operating systems hide this file type from view) then you will need to create it on your server manually.  It should look like this:

```
Options +FollowSymLinks
RewriteEngine On

RewriteBase /

RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php?/$1 [L]
```

To configure Sweetcron

  1. In **system/application/config/** rename **config-sample.php** to **config.php**
  1. Open **config.php** in a text editor and replace `http://www.your-site.com/` with your full site url, including trailing slash.
  1. In **system/application/config/** rename **database-sample.php** to **database.php**
  1. Open **database.php** in a text editor and fill in the `username`, `password` and `database`.
  1. That's it!  Now if you go to your site address you should see instructions for installation.

**If you are installing Sweetcron in a subfolder (e.g. `http://www.your-site.com/lifestream/`), you will need to make these extra changes**

  1. In `config.php` make sure the base\_url is the full url path to your Sweetcron install, i.e. including subfolder.
  1. Open the `.htaccess` file found at the base of Sweetcron and change `RewriteBase /` to `RewriteBase /<YOUR_SUBFOLDER>`

# After Installation #

There are 5 main sections of sweetcron:

## Dashboard ##

Shows your last 5 public items and has links to this project page and the google groups page.

## Write ##

Allows you to post directly to your lifestream.  The content area supports markdown and regular HTML.

## Items ##

A list of all your published / unpublished imported and directly-posted items.  You can also delete, edit, unpublish/publish, search and browse by tag from this section.

## Feeds ##

A list of your active feeds.  You can add new feeds by clicking "Add Feed".

## Options ##

Basic Sweetcron options panel.

# What is the difference between Pseudo Cron and True Cron? #

A cron job is simply a computer task that is fired off at a set interval.  In the world of Sweetcron this refers to your item imports - and this can either happen through Pseudo Cron or True Cron.  The options panel provides a switch between both.  Here is a brief description:

**Pseudo Cron** - zero configuration.  The only disadvantage is that once every 30 minutes, one visitor to your website may experience a slow load time as they will have triggered the automatic import.

**True Cron** - you will need to set up a manual cron job.  The advantage is that updates happen without any user ever knowing (i.e. no slowdowns).  The other advantage is that you can control how frequently Sweetcron imports new items (Pseudo Cron only imports once every 30 mins).  To set up a true cron you can either do it through your web host control panel or through a 3rd party service such as http://www.webcron.org