# WordPress Heroku

This project is a template for getting [WordPress](http://wordpress.org/) up and running on [Heroku](http://www.heroku.com/). It comes with [PostgreSQL for WordPress](http://wordpress.org/extend/plugins/postgresql-for-wordpress/) pre-installed in order to use Heroku's existing Postgres backend. Originally made by [mhoofman](https://github.com/mhoofman/wordpress-heroku), I've updated it with instructions on how to use the new Heroku Postgres installation instead of the soon-to-be deprecated Shared database.

Installation
============

Clone the repository from Github

    $ git clone git://github.com/hanchang/wordpress-heroku.git
    
With the [Heroku gem](http://devcenter.heroku.com/articles/heroku-command), create your app

    $ cd wordpress-heroku
    $ heroku create --stack cedar
		Creating infinite-earth-1517... done, stack is cedar
		http://infinite-earth-1517.herokuapp.com/ | git@heroku.com:infinite-earth-1517.git
		Git remote heroku added

Add a database to your app

    $ heroku addons:add heroku-postgresql:dev
		Adding heroku-postgresql:dev on infinite-earth-1517... done, v2 (free)
		Attached as HEROKU_POSTGRESQL_COPPER
		Database has been created and is available
			! WARNING: dev is in beta
			!          increased risk of data loss and downtime
			!          send feedback to dod-feedback@heroku.com
		Use `heroku addons:docs heroku-postgresql:dev` to view documentation.

Create a new branch for any configuration/setup changes needed

    $ git checkout -b production
    
Copy the `wp-config.php`

    $ cp wp-config-sample.php wp-config.php

Find out the database information, specifying the database that was generated above (in my example, it is HEROKU_POSTGRESQL_COPPER)

		$ heroku pg:credentials HEROKU_DATABASE_NAME
		Connection info string:
		 "dbname=xxxxxxxx host=xxxxxxxx port=5432 user=xxxxxxxx password=xxxxxxxx sslmode=require"

Edit the `wp-config.php` and insert the values provided for DB_NAME, DB_HOST, DB_USER, and DB_PASSWORD

Clear `.gitignore` and commit `wp-config.php`

    $ >.gitignore
    $ git add .
    $ git commit -m "Yay pg-wordpress on heroku!"
    
Deploy to Heroku

    $ git push heroku production:master
    > -----> Heroku receiving push
    > -----> PHP app detected
    > -----> Bundling Apache v2.2.19
    > -----> Bundling PHP v5.3.6
    > -----> Discovering process types
    >        Procfile declares types -> (none)
    >        Default types for PHP   -> web
    > -----> Compiled slug size is 24.9MB
    > -----> Launcing... done, v5
    >        http://strange-turtle-1234.herokuapp.com deployed to Heroku
    >
    > To git@heroku:strange-turtle-1234.git
    > * [new branch]    production -> master 

After deployment WordPress has a few more steps to setup and thats it!

Usage
========

* Because a file cannot be written to Heroku's file system, updating and installing plugins or themes should be done locally and then pushed to Heroku.
* To run WordPress locally on Mac OS X try [MAMP](http://codex.wordpress.org/Installing_WordPress_Locally_on_Your_Mac_With_MAMP).

Updating
========

Updating your WordPress version is just a matter of merging the updates into
the branch created from the installation.

    $ git pull # Get the latest

Using the same branch name from our installation:

    $ git checkout production
    $ git merge master # Merge latest
    $ git push heroku production:master

WordPress needs to update the database. After push, navigate to:

    http://your-app-url.herokuapp.com/wp-admin

WordPress will prompt for updating the database. After that you'll be good
to go.

Custom Domains
==============

Heroku allows you to add custom domains to your site hosted with them.  To add your custom domain, enter in the follow commands.

    $ heroku domains:add www.example.com
    > Added www.example.com as a custom domain name to myapp.heroku.com

You'll also want to cover the non "www" side of the url.

    $ heroku domains:add example.com
    > Added example.com as a custom domain name to myapp.heroku.com

Once Heroku recognizes your custom domain(s) you'll then need to setup separate DNS A records for the following ip addresses to point to your domain:

    75.101.163.44
    75.101.145.87
    174.129.212.2

Once the DNS A records propagate, then simply test out your change by hitting the url in the browser to make sure you are good to go.  If you are in need of cheap DNS hosting then I would recommend [DNSimple](https://dnsimple.com/r/571e28804df06f).

The last step is updating your WordPress installation to recognize the new domain.  You'll need to open up the WordPress Admin Dashboard and go to Settings --> General.  From there just update the URL for the WordPress address and you're done.

If you find yourself running into problems, there is a guide posted in the Heroku Docs which can be found [here](https://devcenter.heroku.com/articles/custom-domains).
