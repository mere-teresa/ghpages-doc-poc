How to retrieve root location
=============================

Version compatibility

Icon

This recipe is compatible with **eZ Publish 5.2 / 2013.09**

 

-   [Retrieving root location
    ID](#Howtoretrieverootlocation-RetrievingrootlocationID)
-   [Retrieving the root
    location](#Howtoretrieverootlocation-Retrievingtherootlocation)

EZP 5.2 / 2013.09

Description
-----------

Knowledge of the root location is important since it can be a starting
point for API queries, or even links to home page, but as eZ Publish can
be used for multisite
development &lt;/development\_and\_administration\_guides/features/multisite\_using\_content\_root&gt;,
and as such **root location can vary**.

By default, root location ID is `2`, but as it can be easily be changed
by configuration, **the best practice is to retrieve it dynamically**.

Retrieving root location ID
---------------------------

Root location ID can be retrieved easily from
[ConfigResolver](Configuration_2720538.html#Configuration-Configuration-DynamicconfigurationwiththeConfigResolver).
Parameter name is `content.tree_root.location_id`.

**In a controller**

``` {.sourceCode .theme:}
<?php
namespace Acme\TestBundle\Controller;

use eZ\Bundle\EzPublishCoreBundle\Controller;


class DefaultController extends Controller
{
    public function fooAction()
    {
        // ...
 
        $rootLocationId = $this->getConfigResolver()->getParameter( 'content.tree_root.location_id' );
 
        // ...
    }
}
```

Retrieving the root location
----------------------------

### From a template

Root location is exposed in the global Twig
helper &lt;/development\_and\_administration\_guides/features/mvc\_and\_application/templating/twig\_helper&gt;.

**Making a link to homepage**

``` {.sourceCode .theme:}
<a href="{{ path( ezpublish.rootLocation ) }}" title="Link to homepage">Home page</a>
```

### From a controller

**eZ Publish 5.2+ / 2013.11+**

``` {.sourceCode .theme:}
<?php
namespace Acme\TestBundle\Controller;

use eZ\Bundle\EzPublishCoreBundle\Controller;

class DefaultController extends Controller
{
    public function fooAction()
    {
        // ...

        $rootLocation = $this->getRootLocation();

        // ...
    }
}
```

**Before 5.2 / 2013.11**

``` {.sourceCode .theme:}
<?php
namespace Acme\TestBundle\Controller;

use eZ\Bundle\EzPublishCoreBundle\Controller;

class DefaultController extends Controller
{
    public function fooAction()
    {
        // ...

        $rootLocationId = $this->getConfigResolver()->getParameter( 'content.tree_root.location_id' );
        $rootLocation = $this->getRepository()->getLocationService()->loadLocation( $rootLocationId );

        // ...
    }
}
```

 

 
