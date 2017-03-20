How to convert request parameters into API objects
==================================================

Created and last modified by <yannick.roger@ez.no> on Feb 02, 2015

&gt;= 2015.01

What it does
============

In lots of cases, request will provide a contentId or a locationId.
Before using them, you will have to load API object within your
controller.

For example:

``` {.sourceCode .theme:}
public function listBlogPostsAction( $locationId )
{
    $location = $repository->getLocationService()->loadLocation( $locationId );
```

Thanks to the param converter, you can directly  have the API object at
your disposal. All you have to do is:

-   For Locations:
    -   In your controllers signature, type int the variable
        to Location.
    -   Make sure a parameter named "locationId" is provided by
        the request.
-   For Contents:
    -   In your controller's signature, typeint the variable to Content
    -   Make sure a parameter named "contentId" is provided by the
        request

Example using Locations:

``` {.sourceCode .theme:}
use eZ\Publish\API\Repository\Values\Content\Location;

public function listBlogPostsAction( Location $location )
{
    // use my $location object
```

How it works
============

If you want to understand how it works you can check [Symfony's param
converter
documentation](http://symfony.com/doc/master/bundles/SensioFrameworkExtraBundle/annotations/converters.html)
and [the pull request implementing the Repository
ParamConverters](https://github.com/ezsystems/ezpublish-kernel/pull/1128).

How to migrate your current application
=======================================

[See example pull request on the
DemoBundle](https://github.com/ezsystems/DemoBundle/pull/129/files) it
provides a few concrete examples.
