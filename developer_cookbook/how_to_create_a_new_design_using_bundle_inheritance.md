How to create a new design using Bundle Inheritance
===================================================

eZ Publish being built using the Symfony 2 framework, it is possible to
benefit from most of its stock feature such as Bundle Inheritance. You
should check out the [related symfony
documentation](http://symfony.com/doc/current/cookbook/bundles/override.html).

Bundle Inheritance allows to customize a template from a parent bundle.
This is very convenient to create a custom design for an already
existing piece of code.

In this cookbook we will create a customized version of a template  from
the DemoBundle.

Creating a bundle
-----------------

Create a new bundle to host your design using the dedicated command
(from your ezpublish installation):

``` {.sourceCode .theme:}
php ezpublish/console generate:bundle
```

Configuring bundle to inherit from another
------------------------------------------

Following the related [Symfony
documentation](http://symfony.com/doc/current/cookbook/bundles/inheritance.html),
modify your bundle to make it inherit from the "eZDemoBundle". Then copy
a template from the DemoBundle in the new bundle, following the same
directory structure. Customize this template, clear application caches
and reload the page. You custom design should be available.

Known limitation
----------------

If you are experiencing problems with routes not working after adding
your bundle take a look at [this
issue](https://jira.ez.no/browse/EZP-23575).

 

 
