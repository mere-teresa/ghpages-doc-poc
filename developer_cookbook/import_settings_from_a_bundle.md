Import settings from a bundle
=============================

Icon

The following recipe is valid for any type of settings supported by
Symfony framework.

 

Use case

Usually, you develop your website using one or several custom bundles as
this is a best practice. However, dealing with core bundles semantic
configuration can be a bit tedious if you maintain it in the main
`ezpublish/config/ezpublish.yml` configuration file.

This recipe will show you how to import configuration from a bundle the
manual way and the implicit way.

The manual way
--------------

This is the simplest way of doing and it has the advantage to be
explicit. The idea is to use the `imports` statement in your main
`ezpublish.yml`:

**ezpublish/config/ezpublish.yml**

``` {.sourceCode .theme:}
imports:
    # Let's import our template selection rules that reside in our custom bundle.
    # MyCustomBundle is the actual bundle name
    - {resource: "@AcmeTestBundle/Resources/config/templates_rules.yml"}
 
ezpublish:
    # ...
```

**templates\_rules.yml, placed under Resources/config folder in
AcmeTestBundle**

``` {.sourceCode .theme:}
# Here I need to reproduce the right configuration tree.
# It will be merged with the main one
ezpublish:
    system:
        my_siteaccess:
            ezpage:
                layouts:
                    2ZonesLayout1:
                        name: "2 zones (layout 1)"
                        template: "AcmeTestBundle:zone:2zoneslayout1.html.twig"

            location_view:
                full:
                    article_test:
                        template: "AcmeTestBundle:full:article_test.html.twig"
                        match:
                            Id\Location: [144,149]
                    another_test:
                        template: "::another_test.html.twig"
                        match:
                            Id\Content: 142

            block_view:
                campaign:
                    template: "AcmeTestBundle:block:campaign.html.twig"
                    match:
                        Type: "Campaign"
```

Icon

During the merge process, if the imported configuration files contain
entries that are already defined in the main configuration file, **they
will override them**.

Tip

Icon

If you want to import configuration for development use only, you can do
so in your `ezpublish_dev.yml`
![(wink)](images/icons/emoticons/wink.png)

The implicit way
----------------

Icon

**Compatible with eZ Publish 5.1+ / Symfony 2.2+** as it uses
`Symfony\Component\DependencyInjection\Extension\PrependExtensionInterface`
which is available as of Symfony 2.2 ([more info on Symfony
cookbook](http://symfony.com/doc/current/cookbook/bundles/prepend_extension.html)).

The following example will show you **how to implicitly load settings to
be configure eZ Publish kernel**. Note that this is also valid for any
bundle !

We assume here that you're aware of [service container
extensions](http://symfony.com/doc/current/book/service_container.html#importing-configuration-via-container-extensions).

**Acme/TestBundle/DependencyInjection/AcmeTestExtension**

``` {.sourceCode .theme:}
<?php

namespace Acme\TestBundle\DependencyInjection;

use Symfony\Component\DependencyInjection\ContainerBuilder;
use Symfony\Component\Config\Resource\FileResource;
use Symfony\Component\Config\FileLocator;
use Symfony\Component\DependencyInjection\Extension\PrependExtensionInterface;
use Symfony\Component\HttpKernel\DependencyInjection\Extension;
use Symfony\Component\DependencyInjection\Loader;
use Symfony\Component\Yaml\Yaml;

/**
 * This is the class that loads and manages your bundle configuration
 *
 * To learn more see {@link http://symfony.com/doc/current/cookbook/bundles/extension.html}
 */
class AcmeTestExtension extends Extension implements PrependExtensionInterface
{
    // ...

    /**
     * Allow an extension to prepend the extension configurations.
     * Here we will load our template selection rules.

     *
     * @param ContainerBuilder $container
     */
    public function prepend( ContainerBuilder $container )
    {
        // Loading our YAML file containing our template rules
        $configFile = __DIR__ . '/../Resources/config/template_rules.yml';
        $config = Yaml::parse( file_get_contents( $configFile ) );
        // We explicitly prepend loaded configuration for "ezpublish" namespace.
        // So it will be placed under the "ezpublish" configuration key, like in ezpublish.yml.
        $container->prependExtensionConfig( 'ezpublish', $config );
        $container->addResource( new FileResource( $configFile ) );
    }
}
```

**AcmeTestBundle/Resources/config/template\_rules.yml**

``` {.sourceCode .theme:}
# We explicitly prepend config for "ezpublish" namespace in service container extension, 
# so no need to repeat it here
system:
    ezdemo_frontend_group:
        ezpage:
            layouts:
                2ZonesLayout1:
                    name: "2 zones (layout 1)"
                    template: "AcmeTestBundle:zone:2zoneslayout1.html.twig"

        location_view:
            full:
                article_test:
                    template: "AcmeTestBundle:full:article_test.html.twig"
                    match:
                        Id\Location: 144
                another_test:
                    template: "::another_test.html.twig"
                    match:
                        Id\Content: 142

        block_view:
            campaign:
                template: "AcmeTestBundle:block:campaign.html.twig"
                match:
                    Type: "Campaign"
```

Regarding performance

Icon

Service container extensions are called only when the container is being
compiled, so there is nothing to worry about regarding performance.

Icon

Configuration loaded this way will be overridden by the main
`ezpublish.yml` file.

 

 

Comments:
---------

+--------------------------------------------------------------------------+
| If I have 20 siteaccesses, one for each language, do I have to define    |
| the rules 20 times in the templates\_rules.yml?                          |
|                                                                          |
| ![image5](images/icons/contenttypes/comment_16.png) Posted by xrow at    |
| Jul 15, 2013 07:59                                                       |
+--------------------------------------------------------------------------+
| No, you can simply create a group with the 20 siteaccesses and add the   |
| general definitions to the group                                         |
|                                                                          |
| ![image6](images/icons/contenttypes/comment_16.png) Posted by            |
| <pedro.resende@ez.no> at Jul 15, 2013 09:45                              |
+--------------------------------------------------------------------------+
| Hi, regarding your warning about clearing the cache, i noticed in the eZ |
| Publish Demo Bundle that they are adding another line which after my     |
| test relieves you from clearing the cache                                |
|                                                                          |
| **Code snippet** .. code:: theme:                                        |
|                                                                          |
| > use SymfonyComponentConfigResourceFileResource;                        |
| >                                                                        |
| > // ...                                                                 |
| >                                                                        |
| > \$configFile = \_\_DIR\_\_ .                                           |
| > '/../Resources/config/template\_rules.yml';                            |
| > \$container-&gt;addResource( new FileResource( \$configFile ) );       |
|                                                                          |
| | link to the Dependency Injection Extension of eZ Publish Demo Bundle:  |
| | https://github.com/ezsystems/DemoBundle/blob/master/DependencyInjecti  |
| on/eZDemoExtension.php &lt;https://github.com/ezsystems/DemoBundle/blob/ |
| mas                                                                      |
| ter/DependencyInjection/eZDemoExtension.php&gt;\_\_                      |
|                                                                          |
| ![image7](images/icons/contenttypes/comment_16.png) Posted by pr pr at   |
| Aug 19, 2014 13:46                                                       |
+--------------------------------------------------------------------------+
| Hi, according to my tests, if a configuration option is present both in  |
| the main ezpublish.yml config file and in an imported config file, it is |
| the value present in the main config file that takes precedence, and not |
| the opposite, and this is true for the manual and implicite ways.        |
|                                                                          |
| ![image8](images/icons/contenttypes/comment_16.png) Posted by pr pr at   |
| Aug 19, 2014 14:42                                                       |
+--------------------------------------------------------------------------+


