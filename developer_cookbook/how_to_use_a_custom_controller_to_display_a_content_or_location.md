How to use a custom controller to display a content or location
===========

Version compatibility

Icon

This recipe is compatible with **eZ Publish 5.2 / 2013.07**

 

-   [Enhanced views for
    Content/Location](#Howtouseacustomcontrollertodisplayacontentorlocation-EnhancedviewsforContent/Location)

Enhanced views for Content/Location
===================================

In some cases, displaying a content/location via the built-in
`ViewController` is not sufficient and will lead you to do many
sub-requests in order to access different parameters.

Typical use cases are access to:

-   Settings (either coming from `ConfigResolver` or `ServiceContainer`)
-   Current content's `ContentType` object
-   Current location's parent
-   Current location's children count
-   Main location and alternative locations for the current content
-   etc…

In those cases, you may want to **use your own controller** to display
the current content/location instead of using the
built-in `ViewController`.

Description
-----------

This feature covers 2 general use cases:

-   Lets you configure a custom controller with the configured
    matcher rules.
-   Lets you override the built-in view controller in a clean way.

Matching custom controllers
---------------------------

This is possible with the following piece of configuration:

``` {.sourceCode .theme:}
ezpublish:
    system:
        my_siteaccess:
            location_view:
                full:
                    # Defining a ruleset matching a location and pointing to a controller
                    my_ruleset:
                        # The following will let you use your own custom controller for location #123
                        # (Here it will use AcmeTestBundle/Controller/DefaultController::viewLocationAction(),
                        # following the Symfony controller notation convention.
                        # Method viewLocationAction() must follow the same prototype as in the built-in ViewController
                        controller: AcmeTestBundle:Default:viewLocation
                        match:
                            Id\Location: 123
```

Icon

You can point to any kind of [controller supported by
Symfony](http://symfony.com/doc/current/book/page_creation.html#step-2-create-the-controller)
(including [controllers as a
service](http://symfony.com/doc/current/cookbook/controller/service.html)).

The only requirement here is that your action method has a similar
signature
than `ViewController::viewLocation()` or `ViewController::viewContent()` (depending
on what you're matching of course). However, note that all arguments are
not mandatory since [Symfony is clever enough to know what to inject in
your action
method](http://symfony.com/doc/current/book/routing.html#route-parameters-and-controller-arguments).
Hence **you're not forced to mimic the `ViewController`'s signature
strictly**. For example, if you omit `$layout` and `$params` arguments,
it will be still valid. Symfony will just avoid to inject them in your
action method.

### Original ViewController signatures

**viewLocation() signature**

``` {.sourceCode .theme:}
/**
 * Main action for viewing content through a location in the repository.
 *
 * @param int $locationId
 * @param string $viewType
 * @param boolean $layout
 * @param array $params
 *
 * @throws \Symfony\Component\Security\Core\Exception\AccessDeniedException
 * @throws \Exception
 *
 * @return \Symfony\Component\HttpFoundation\Response
 */
public function viewLocation( $locationId, $viewType, $layout = false, array $params = array() )
```

**viewContent() signature**

``` {.sourceCode .theme:}
/**
 * Main action for viewing content.
 *
 * @param int $contentId
 * @param string $viewType
 * @param boolean $layout
 * @param array $params
 *
 * @throws \Symfony\Component\Security\Core\Exception\AccessDeniedException
 * @throws \Exception
 *
 * @return \Symfony\Component\HttpFoundation\Response
 */
public function viewContent( $contentId, $viewType, $layout = false, array $params = array() )
```

Note

Icon

Controller selection doesn't apply to `block_view` since you can already
[use your own controller to display
blocks](The-Page-FieldType_12124915.html#ThePageFieldType-ThePageFieldType-Renderingblocks).

Warning on caching

Icon

Using your own controller, **it is your responsibility to define cache
rules**, like for every custom controller !

So don't forget to **set cache rules** and the
appropriate **`X-Location-Id` header** in the
returned `Response` object.

[See built-in
ViewController](https://github.com/ezsystems/ezpublish-kernel/blob/master/eZ/Publish/Core/MVC/Symfony/Controller/Content/ViewController.php#L76) for
more details on this.

Examples
--------

### Enriching built-in ViewController

This example shows how to use a custom controller to enrich the final
configured view template. Your controller will here forward the request
to the built-in `ViewController` with some additional parameters.

Icon

**This is usually the recommended way to use a custom controller.**

Icon

| Always ensure to add new parameters to existing `$params` associative
array, using [**`+`** union
operator](http://php.net/manual/en/language.operators.array.php) or
`array_merge()`. | **Not doing so (e.g. only passing your custom
parameters array) can result with unexpected issues with content
preview**. Previewed content and other parameters are indeed passed in
`$params`.

**ezpublish.yml**

``` {.sourceCode .theme:}
ezpublish:
    system:
        ezdemo_frontend_group:
            location_view:
                full:
                    article_test:
                        # Configuring both controller and template as the controller will forward
                        # the request to the ViewController which will render the configured template.
                        controller: AcmeTestBundle:Default:articleViewEnhanced
                        template: AcmeTestBundle:full:article_test.html.twig
                        match:
                            Identifier\ContentType: [article]
```

**Controller**

``` {.sourceCode .theme:}
<?php
namespace Acme\TestBundle\Controller;
use Symfony\Component\HttpFoundation\Response;
use eZ\Bundle\EzPublishCoreBundle\Controller;

class DefaultController extends Controller
{
    public function articleViewEnhancedAction( $locationId, $viewType, $layout = false, array $params = array() )
    {
        // Add custom parameters to existing ones.
        $params += array( 'myCustomVariable' => "Hey, I'm a custom message!" );
        // Forward the request to the original ViewController
        // And get the response. Eventually alter it (here we change the smax-age for cache).
        $response = $this->get( 'ez_content' )->viewLocation( $locationId, $viewType, $layout, $params );
        $response->setSharedMaxAge( 600 );

        return $response;
    }
}
```


### Using a custom controller to get full control

This example shows you how to configure and use your own controller to
handle a location.



**ezpublish.yml**

``` {.sourceCode .theme:}
ezpublish:
    system:
        ezdemo_frontend_group:
            location_view:
                full:
                    my_ruleset:
                        controller: AcmeTestBundle:Default:viewFolder
                        match:
                            Identifier\ContentType: [folder]
                            Identifier\Section: [standard]
```

Icon

Always ensure to have \$params argument and to add new parameters to it,
using [**`+`** union
operator](http://php.net/manual/en/language.operators.array.php) or
`array_merge()`.

**Not doing so (e.g. only passing your custom parameters array) can
result with unexpected issues with content preview**. Previewed content
and other parameters are indeed passed in `$params`.

**Controller**

``` {.sourceCode .theme:}
<?php
namespace Acme\TestBundle\Controller;
use Symfony\Component\HttpFoundation\Response;
use eZ\Bundle\EzPublishCoreBundle\Controller;

class DefaultController extends Controller
{
    public function viewFolderAction( $locationId, $layout = false, $params = array() )
    {
        $repository = $this->getRepository();
        $location = $repository->getLocationService()->loadLocation( $locationId );
        // Check if content is not already passed. Can be the case when using content preview.
        $content = isset( $params['content'] ) ? $params['content'] : $repository->getContentService()->loadContentByContentInfo( $location->getContentInfo() )
        $response = new Response();
        $response->headers->set( 'X-Location-Id', $locationId );
        // Caching for 1h and make the cache vary on user hash
        $response->setSharedMaxAge( 3600 );
        $response->setVary( 'X-User-Hash' );
        return $this->render(
            'AcmeTestBundle::custom_controller_folder.html.twig',
            array(
                'location' => $location,
                'content' => $content,
                'foo' => 'Hey world!!!',
                'osTypes' => array( 'osx', 'linux', 'losedows' )
            ) + $params
        );
    }
}
```

**custom\_controller\_folder.html.twig**

``` {.sourceCode .theme:}
{% extends "eZDemoBundle::pagelayout.html.twig" %}

{% block content %}
<h1>{{ ez_render_field( content, 'title' ) }}</h1>
    <h1>{{ foo }}</h1>
    <ul>
    {% for os in osTypes %}
        <li>{{ os }}</li>
    {% endfor %}
    </ul>
{% endblock %}
```

Overriding the built-in ViewController
--------------------------------------

One other way to keep control on what is passed to the view is to use
your own controller instead of the built-in ViewController.

Base ViewController being defined as a service, with a service alias,
this can be easily achieved from your bundle's configuration:

``` {.sourceCode .theme:}
parameters:
    my.custom.view_controller.class: Acme\TestBundle\MyViewController

services:
    my.custom.view_controller:
        class: %my.custom.view_controller.class%
        arguments: [@some_dependency, @other_dependency]

    # Change the alias here and make it point to your own controller
    ez_content:
        alias: my.custom.view_controller
```

Warning

Icon

Doing so will completely override the built-in ViewController! Use this
at your own risk!

See also

Icon

See also

[How to Display a default text while asynchronous loading of a
controller](https://confluence.ez.no/display/EZP/Content+view#Contentview-Displayadefaulttext)

[How to render an embedded content from a Twig
template](https://doc.ez.no/display/EZP/Content+view#Contentview-Renderembeddedcontentobjects)

 

 

Comments:
---------

+--------------------------------------------------------------------------+
| Do I need to set the configuration in ezpublish/config/ezpublish.yml     |
| file                                                                     |
|                                                                          |
| Or there is a way to put it in the Bundle?                               |
|                                                                          |
| ![image6](images/icons/contenttypes/comment_16.png) Posted by gof at Jan |
| 23, 2014 23:00                                                           |
+--------------------------------------------------------------------------+
| You can of course put it in a bundle, as explained                       |
| here &lt;https://confluence.ez.no/display/EZP/Import+settings+from+a+bun |
| dle                                                                      |
| &gt;\_\_ ![(smile)](images/icons/emoticons/smile.png).                   |
|                                                                          |
| And there are examples in the DemoBundle.                                |
|                                                                          |
| ![image7](images/icons/contenttypes/comment_16.png) Posted by            |
| <jerome.vieilledent@ez.no> at Jan 24, 2014 08:56                         |
+--------------------------------------------------------------------------+
| Got it. Thanks!                                                          |
|                                                                          |
| ![image8](images/icons/contenttypes/comment_16.png) Posted by gof at Jan |
| 24, 2014 09:06                                                           |
+--------------------------------------------------------------------------+
| Shouldn't "viewLocation()" and "viewContent()" in this page be replaced  |
| with "viewLocationAction()" and "viewContentAction()" ?                  |
|                                                                          |
| ![image9](images/icons/contenttypes/comment_16.png) Posted by taenadil   |
| at Aug 29, 2014 12:14                                                    |
+--------------------------------------------------------------------------+
| [Xavier Van Herpe](https://doc.ez.no/display/~taenadil): No, we just     |
| present the base ViewController method signatures here.                  |
|                                                                          |
| ![image10](images/icons/contenttypes/comment_16.png) Posted by           |
| <jerome.vieilledent@ez.no> at Aug 29, 2014 21:14                         |
+--------------------------------------------------------------------------+


