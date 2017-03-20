How to authenticate a user with multiple user providers
=======================================================

Version compatibility

Icon

This recipe is compatible with **eZ Publish 5.3 / 2014.01**

 

-   EZP &gt;= 5.3 /
    2014.01 &lt;\#Howtoauthenticateauserwithmultipleuserproviders-YellowEZP&gt;=5.3/2014.01&gt;\_\_

EZP &gt;= 5.3 / 2014.01
-----------------------

Description
-----------

Symfony provides native support for [multiple user
providers](http://symfony.com/doc/2.3/book/security.html#using-multiple-user-providers).
This makes it easy to integrate any kind of login handlers, including
SSO and existing 3rd party bundles
(e.g. [FR3DLdapBundle](https://github.com/Maks3w/FR3DLdapBundle), [HWIOauthBundle](https://github.com/hwi/HWIOAuthBundle), [FOSUserBundle](https://github.com/FriendsOfSymfony/FOSUserBundle), [BeSimpleSsoAuthBundle](http://github.com/BeSimple/BeSimpleSsoAuthBundle)...).

However, to be able to use *external* user providers with eZ, a valid eZ
user needs to be injected in the repository. This is mainly for the
kernel to be able to manage content related permissions (but not limited
to).

Depending on your context, you will either want to create an eZ
user `on-the-fly`, return an existing user, or even always use a generic
user.

 

Solution
--------

Whenever a *external* user is matched (i.e. that does not come from eZ
repository, like coming from LDAP), eZ kernel fires
an `MVCEvents::INTERACTIVE_LOGIN` event. Every service listening to this
event will receive
a `eZ\Publish\Core\MVC\Symfony\Event\InteractiveLoginEvent` object which
contains the original security token (that holds the matched user) and
the request.

It's then up to the listener to retrieve an eZ user from repository and
assign it back to the event object. This user will be injected in the
repository and used for the rest of the request.

Icon

If no eZ user is returned, the anonymous user will then be used.

### User exposed and security token

When a *external* user is matched, a different token will be injected in
the security context, the `InteractiveLoginToken`. This token holds
a `UserWrapped` instance which contains the originally matched user and
the *API user* (the one from the eZ repository).

Note that the *API user* is mainly used for permission checks against
the repository and thus stays *under the hood*.

### Customizing the user class

It is possible to customize the user class used by
extending `ezpublish.security.login_listener` service, which defaults
to `eZ\Publish\Core\MVC\Symfony\Security\EventListener\SecurityListener`.

You can override `getUser()` to return whatever user class you want, as
long as it
implements `eZ\Publish\Core\MVC\Symfony\Security\UserInterface`.

Example
-------

Here is a very simple example using the in-memory user provider.

**ezpublish/config/security.yml**

``` {.sourceCode .theme:}
security:
    providers:
        # Chaining in_memory and ezpublish user providers
        chain_provider:
            chain:
                providers: [in_memory, ezpublish]
        ezpublish:
            id: ezpublish.security.user_provider
        in_memory:
            memory:
                users:
                    # You will then be able to login with username "user" and password "userpass"
                    user:  { password: userpass, roles: [ 'ROLE_USER' ] }
    # The "in memory" provider requires an encoder for Symfony\Component\Security\Core\User\User
 encoders:
        Symfony\Component\Security\Core\User\User: plaintext
```

### Implementing the listener

**services.yml in your AcmeTestBundle**

``` {.sourceCode .theme:}
parameters:
    acme_test.interactive_event_listener.class: Acme\TestBundle\EventListener\InteractiveLoginListener

services:
    acme_test.interactive_event_listener:
        class: %acme_test.interactive_event_listener.class%
        arguments: [@ezpublish.api.service.user]
        tags:
            - { name: kernel.event_subscriber } 
```

Icon

Do not mix `MVCEvents::INTERACTIVE_LOGIN` event (specific to eZ Publish)
and `SecurityEvents::INTERACTIVE_LOGIN` event (fired by Symfony security
component)

**Interactive login listener**

``` {.sourceCode .theme:}
<?php
namespace Acme\TestBundle\EventListener;

use eZ\Publish\API\Repository\UserService;
use eZ\Publish\Core\MVC\Symfony\Event\InteractiveLoginEvent;
use eZ\Publish\Core\MVC\Symfony\MVCEvents;
use Symfony\Component\EventDispatcher\EventSubscriberInterface;

class InteractiveLoginListener implements EventSubscriberInterface
{
    /**
     * @var \eZ\Publish\API\Repository\UserService
     */
    private $userService;

    public function __construct( UserService $userService )
    {
        $this->userService = $userService;
    }

    public static function getSubscribedEvents()
    {
        return array(
            MVCEvents::INTERACTIVE_LOGIN => 'onInteractiveLogin'
        );
    }

    public function onInteractiveLogin( InteractiveLoginEvent $event )
    {
        // We just load a generic user and assign it back to the event.
        // You may want to create users here, or even load predefined users depending on your own rules.
        $event->setApiUser( $this->userService->loadUserByLogin( 'lolautruche' ) );
    }
} 
```

*\**\*

 

 

 

+--------------------------------------------------------------------------+
| -   \# The "in memory" provider requires an encoder for                  |
|     Symfony\\Component\\Security\\Core\\User\\User                       |
+--------------------------------------------------------------------------+

 

 

  ------
   
  ------

+------------------+
| -   encoders:    |
+------------------+

 

 

  ------
   
  ------

-   Symfony\\Component\\Security\\Core\\User\\User:

Comments:
---------

+--------------------------------------------------------------------------+
| A couple of questions:                                                   |
|                                                                          |
| 1.  when multiple user providers are chained, will the                   |
|     interactiveloginevent be fired only once (from first the user        |
|     provider which matched the user credentials) ? Is it possible to     |
|     distinguish the provider which actually did match?                   |
| 2.  "user exposed and security token" paragraph: this is for code which  |
|     gets executed after the user login process, I suppose ?              |
| 3.  "customising the user class" paragraph: any example of why/where     |
|     this could be useful?                                                |
| 4.  if I get it correctly, to implement the equivalent of an eZ4         |
|     custom-login-handler, the dev needs to write both an                 |
|     sf-user-provider and the event-listener described here. Could        |
|     someone point to a very simple example of implementation of the      |
|     former?                                                              |
|                                                                          |
| ![image4](images/icons/contenttypes/comment_16.png) Posted by            |
| <gaetano.giunta@ez.no> at Sep 10, 2014 11:04                             |
+--------------------------------------------------------------------------+
| 1.  Mainly from the token type, embedded in the event object. Matched    |
|     used object is inside the token.                                     |
| 2.  This paragraph is about the eZ user (from Repository) you want to    |
|     use in the end, for Repository authentication.                       |
| 3.  Really depends on your needs...                                      |
| 4.  Yes, but "sf-user-provider" are usually reused from bundles in the   |
|     Symfony community, unless very specific.                             |
|                                                                          |
|     > Could someone point to a very simple example of implementation of  |
|     > the former?                                                        |
|                                                                          |
| You mean a full implementation of custom login handler?                  |
|                                                                          |
|                                                                          |
|                                                                          |
| ![image5](images/icons/contenttypes/comment_16.png) Posted by            |
| <jerome.vieilledent@ez.no> at Sep 10, 2014 12:21                         |
+--------------------------------------------------------------------------+
| It would be great to have a more detailed example of implementation with |
| an external ldap user provider:                                          |
|                                                                          |
|     providers[ldap, ezpublish]                                           |
|                                                                          |
| I managed so far to install/config BorisMorel LdapBundle:                |
| https://github.com/BorisMorel/LdapBundle &lt;https://github.com/BorisMor |
| el                                                                       |
| /LdapBundle&gt;\_\_                                                      |
|                                                                          |
| I can even connect to the ldap server and authenticate an external user  |
| until ez pulish refreshes the page and reloads user from user provider.  |
| After reloading user from user provider, symfony debug shows Logged in   |
| as external user but Authenticated = No, and redirects to login again.   |
|                                                                          |
| Do I have to customize the User class provided by the ldap bundle        |
| (https://github.com/BorisMorel/LdapBundle/blob/master/User/LdapUser.php  |
|  &lt;https://github.com/BorisMorel/LdapBundle/blob/master/User/LdapUser. |
| php                                                                      |
| &gt;\_\_) ?                                                              |
|                                                                          |
| Do I have to generate a new LoginToken ?                                 |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
|                                                                          |
| ![image6](images/icons/contenttypes/comment_16.png) Posted by kaonan at  |
| Sep 12, 2014 06:43                                                       |
+--------------------------------------------------------------------------+
| After implementing a pure symfony user login system with custom          |
| UserProvider and AuthenticationProvider using a 3rd party API, failure   |
| and succes handlers, I started integrating the bundle in eZ5.            |
|                                                                          |
| Managed to cable it in and get the user logged in, BUT I realized that I |
| ended up with UserWrapped type and not my custom User Type. This causes  |
| a lot of issues when using FormTypes in my Controllers.I am using        |
| getUser() in the Controllers which return an UserWrapped which actually  |
| contains my customUser in a private wrappedUser to which unfortunately   |
| we do not have access.                                                   |
|                                                                          |
| Documentation suggests to override getUser() to get your custom user     |
| class but as long as you implement eZUserInterface, which I can not do   |
| as I want to develop a stand alone bundle, so I am implementing directly |
| Symfony's UserInterface.                                                 |
|                                                                          |
| *You can override `getUser()` to return whatever user class you want, as |
| long as it                                                               |
| implements `eZ\Publish\Core\MVC\Symfony\Security\UserInterface`.*        |
|                                                                          |
| I am aware that this means not having a repo user at the end so it might |
| cause issues when working with content due to roles, but that is not the |
| case (at least for the moment).                                          |
|                                                                          |
| So what I was thinking is to implement a Listener that will listen to    |
| SecurityEvents::INTERACTIVE\_LOGIN with a higher priority, and stop      |
| propagation afterwards so it does not end up caught in  ez               |
| SecurityListener, thus not ending up with UserWrapped and                |
| InteractiveLoginToken.                                                   |
|                                                                          |
|                                                                          |
|                                                                          |
| What is your opinion on this approach?                                   |
|                                                                          |
| Thank you in advance.                                                    |
|                                                                          |
| ![image7](images/icons/contenttypes/comment_16.png) Posted by valmaior   |
| at Jan 12, 2015 16:17                                                    |
+--------------------------------------------------------------------------+


