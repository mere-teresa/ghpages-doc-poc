EzPublishCoreBundle Configuration
=================================

Created and last modified by <jerome.vieilledent@ez.no> on Jul 01, 2014

Icon

To get an overview of EzPublishCoreBundle's configuration, run the
following command-line script:

``` {.sourceCode .theme:}
php ezpublish/console config:dump-reference ezpublish
```

Default page
------------

Version compatibility

Icon

This setting is available as of **5.3.2 / 2014.07**

Default page is the default page to show or redirect to.

If set, it will be used for default redirection after user login,
overriding Symfony's `default_target_path`, giving the opportunity to
configure it by SiteAccess.

``` {.sourceCode .theme:}
ezpublish:
    system:
        ezdemo_site:
            default_page: "/Getting-Started"

        ezdemo_site_admin:
            # For admin, redirect to dashboard after login.
            default_page: "/content/dashboard"
```

This setting **does not change anything to Symfony behavior** regarding
redirection after login. If set, it will only substitute the value set
for `default_target_path`. It is therefore still possible to specify a
custom target path using a dedicated form parameter.

**Order of precedence is not modified.**
