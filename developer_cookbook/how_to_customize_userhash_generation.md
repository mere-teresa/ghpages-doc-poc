How to customize UserHash generation
====================================

Version compatibility

Icon

Compatible with **eZ Publish 5.25.3 and 2013.07 (up to 2014.11)**

Deprecated

Icon

| This recipe is **deprecated** as of 5.4 / 2014.11. | Use [user
context providers from
FOSHttpCacheBundle](http://foshttpcachebundle.readthedocs.org/en/latest/reference/configuration/user-context.html#custom-context-providers)
instead.

 

-   [Securing hash generation
    request](#HowtocustomizeUserHashgeneration-Securinghashgenerationrequest)

Icon

Be sure to have read Context aware HTTP
cache &lt;/development\_and\_administration\_guides/features/cache/context\_aware\_http\_cache&gt;
documentation before reading this recipe.

Description
-----------

When user hash generation is requested, eZ Publish will create
a **hashable User Identity** object.

One can add information to the Identity object making the resulted hash
vary. This can be done by registering **Identity definers**.

 

For this, all you need to do is to declare a service with
**`ezpublish.identity_definer`** tag. Class for this
service **must** implement \**`eZ\Publish\SPI\User\IdentityAware`*\* interface.

Example
-------

**services.yml (inside a bundle)**

``` {.sourceCode .theme:}
parameters:
    my_identity_definer.class: Acme\TestBundle\Identity\MyDefiner
 
services:
    my_identity_definer:
        class: %my_identity_definer.class%
        tags:
            - { name: ezpublish.identity_definer }
```

``` {.sourceCode .theme:}
<?php

namespace Acme\TestBundle\Identity;

use eZ\Publish\SPI\User\IdentityAware;
use eZ\Publish\SPI\User\Identity;

class MyDefiner implements IdentityAware
{
    public function setIdentity( Identity $identity )
    {
        // Here I can add information to $identity.
        // value MUST be scalar.
        $identity->setInformation( 'my_key', 'my_value' );
    }
}
```

Securing hash generation request
--------------------------------

By default, hash generation requests are granted for localhost
(`127.0.0.1`, `::1`, `fe80::1`).

If you want to enlarge the scope (e.g. if your Varnish server is not
running on the same machine), you can
override `canGenerateUserHash()` protected method in your main kernel
class (mostly `EzPublishKernel`).

Comments:
---------

+--------------------------------------------------------------------------+
| Please ignore if the question/comment is too far off, I'm diving in this |
| topic now but I am not anymore a                                         |
| developer ![(smile)](images/icons/emoticons/smile.png) so I might miss a |
| few things...                                                            |
|                                                                          |
| Here are the 2 questions:                                                |
|                                                                          |
| -   About Securing hash generation request: is this specific to          |
|     customized UserHash generation or valid in all cases? If so, might   |
|     belong more to the main documentation? (Context aware HTTP           |
|     cache &lt;/development\_and\_administration\_guides/features/cache/c |
| ontext\_aware\_http\_cache&gt;)                                          |
| -   It seems a fairly usual use case to run Varnish on                   |
|     different machines. Should this kind of setup be possible by         |
|     configuration and not code?                                          |
|                                                                          |
|                                                                          |
|                                                                          |
| ![image2](images/icons/contenttypes/comment_16.png) Posted by            |
| <Roland.Benedetti@ez.no> at May 14, 2014 22:17                           |
+--------------------------------------------------------------------------+


