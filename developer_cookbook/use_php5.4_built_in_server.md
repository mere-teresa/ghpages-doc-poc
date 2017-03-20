Use PHP5.4 built-in server
==========================

Version compatibility

Icon

This recipe is compatible with **eZ Publish 5.2 / 2013.05**

 

Description
-----------

PHP 5.4 comes with a [built-in webserver for development
purpose](http://php.net/manual/en/features.commandline.webserver.php).
This very handy as it makes you **kickstart development quickly**,
getting rid of Apache / Nginx configuration hassle. All you need here is
PHP 5.4+ with command line binary.

Usage
-----

Symfony comes with a wrapper script for booting the built-in
webserver: `server:run`. It's a nice shortcut as it will correctly set
the web root depending on your configuration. However, you can't use it
out of the box with eZ Publish for several reasons (i.e. we don't share
the same front controllers and we need specific rewrite rules).

Icon

Use this command for **development purpose only** !\
**DO NOT use it in production !**

To be able to run this command for eZ Publish development purpose, we
need to use the provided router script which handles all the rewrite
rules for you.

The following example will start the webserver on `localhost:8000`, so
you'll be able to run your eZ application by going
to <http://localhost:8000> from your browser.

``` {.sourceCode .theme:}
php ezpublish/console server:run -r ../bin/ezrouter.php localhost:8000
```

Icon

`../bin/ezrouter.php` is needed because with `server:run` command,
working directory is `web/`.
