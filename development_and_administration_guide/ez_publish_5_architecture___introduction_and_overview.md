eZ Publish 5 Architecture - Introduction & Overview
===================================================

(Version : EZP &gt;= 5)

With the 5.0 release eZ Publish is making an important leap forwards in
terms of technology.

-   [Why this change in
    technology](#eZPublish5Architecture-Introduction&Overview-Whythischangeintechnology)
-   [What are the
    changes?](#eZPublish5Architecture-Introduction&Overview-Whatarethechanges?)
-   [What will be
    gained?](#eZPublish5Architecture-Introduction&Overview-Whatwillbegained?)

<!-- -->

-   [Understanding the architecture in more
    detail](#eZPublish5Architecture-Introduction&Overview-Understandingthearchitectureinmoredetail)

<!-- -->

-   [Summary on the ways to use eZ Publish
    5](#eZPublish5Architecture-Introduction&Overview-SummaryonthewaystouseeZPublish5)

In summary eZ Publish 5.0 introduces a new technology stack referred to
as "Platform" \*(earlier sometimes referred to as "new stack", "6.x
stack" or "Symfony stack")\* next to the existing eZ Publish 4.x
technology, herby referred to as *Legacy.* Platform Stack will
throughout the 5.x series mature for each release until it becomes ready
to be next evolution of eZ Publish. This approach allows for very high
degree of forward and backwards compatibility.

This document will explain why **we are renewing our technology
platform**, and to some degree explain what this **evolution in
architecture** means to eZ Publish developers and users, and last but
not least **how eZ Systems is affected** by these changes.

Why this change in technology
-----------------------------

The background for the changes is to meet the challenges ahead:

-   Customer and User Experience Management requirements: beyond simple
    web content management, enable the building of any digital user
    experience
-   Performance and scalability: deliver on the ever increasing need for
    performance and scalability
-   The big data situation:  embrace big data situation as an
    opportunity to build new digital services and not view it as a
    hurdle
-   Fulfill the multichannel vision: enable content to live in any
    screen, any app, any device

The overall goal is **to reach Digital Service Excellence**, ie, we
would like our partners to be able to offer the customers exactly what
they want, within acceptable timeframes and price tags on the eZ Publish
Platform.

What are the changes?
---------------------

eZ Systems is now introducing the following as the new "Platform":

-   **An extended and sustainable Public API**. This will speed up
    developments on the platform and improve their quality and
    maintainability both for eZ core developers and extension
    developers, which in turn will lead to more efficient implementation
    projects
-   **An improved REST API** with read and write functionality and
    support all the core content management functionality.  This way, eZ
    Publish will better integrate with any programming framework, for
    instance into mobile applications or any other web application.
    Better meaning faster and simpler. in order to fulfill the
    multichannel vision and execute on the User Experience demands and
    needs of customers and users of the eZ Publish platform
-   Introducing **Symfony** as the Web Framework (PHP) under the
    platform to make developers lives easier and to make developments on
    eZ Publish accessible for more developers to further support
    innovation in eZ Publish
-   Introducing **Twig as the template engine** to simplify working with
    templates in eZ Publish. And as a standard template engine this will
    also make eZ Publish templating accessible to more developers
-   Introducing**a new storage system** for scalability, performance and
    maintainability reasons

And as a result of all these changes, a major last item is

-    Introducing **a backward compatibility** between the new
    *"Platform"* architecture resulting from the above and the
    *"legacy"* architecture as known in eZ Publish 4.x

What will be gained?
--------------------

### Symfony2

With [Symfony](http://symfony.com/) as the web framework eZ Publish will
be more accessible. Thanks to the framework, it will be used for
instance to extend eZ Publish applications with new features,
potentially not based at all on the content repository (for example,
business specific application logic) using a standard PHP framework and
a very clean application design with minimum interlocking with eZ
Publish itself.

Any developer knowing Symfony will then be able to easily develop eZ
Publish Bundles &lt;/installation\_and\_upgrade\_guides/installation/extensions&gt; the
equivalent of eZ Publish legacy extensions.

This will cater for :

-    a better quality of the code
-    a lower entry point to do developments in eZ Publish
-    more innovation faster since more developers will use a standard
    PHP framework
-    Symfony has an open-source community that will help developers
-    Symfony is commercially backed up, so they are in it for the long
    run and Sensio Lab (maker of Symfony) will naturally extend the eZ
    offering to the framework level if need be. 

### Twig

Initially, eZ Systems has developed its own templating engine. eZ has
also worked and redeveloped a second one at some point as part of the eZ
Component project. When designing eZ Publish 5, we knew the old template
engine was not good enough anymore and needed to be replaced. We knew
the eZ Components template engine we contributed and originated was an
option, but we decided to go for another one:
[Twig](http://twig.sensiolabs.org/). Reasons are multiple: quality of
course (even if the eZ components one was also very high quality),
integration and cohesion to use it with the Symfony stack and finally
again the strength and size of the community using it today. Symfony has
developed Twig as a standard template engine, and eZ Systems is using it
to further the following:

-   to update the legacy template engine
-   to simplify template coding in eZ Publish
-   to have faster page rendering
-   to benefit of more features which were not in the original engine
-   | to make extension developments easier to speed up innovation in eZ
    Publish |  

This is a move that will influence every stakeholder in the eZ Publish
development. As developers will implement faster, this also mean project
will significantly improve in total cost of ownership as well as in
**time to market**. 

### New storage systems

In order to meet performance and scalability requirements in the future,
eZ introduces new storage systems with the version 5 serie.

In 5.0, this storage system lives beside the legacy storage system and
data model, but will use the new API to access the data.

Also in 5.0 version, this new storage engine only support MySQL
relational database, nevertheless it is designed to allow the
development of drivers for other storage engines through the Persistence
SPI (service provider interface) and in the future will include drivers
for NoSQL and Document based storage engine.

The ultimate goal is to open for custom storage developments.

### Rest API

| In order to meet all multi-channel requirements we are developing a
Rest API &lt;/development\_and\_administration\_guides/features/ez\_publish\_rest\_api&gt;
that cover all core feature of content management so we can integrate
with any application on any channel in any programming language. | The
gain for users will be for anyone integrating eZ Publish with other
applications, not only the development will be significantly improved
but more importantly, the value of an API also lies in the
**maintainability and sustainability** it offers. the new rest api is
designed to stay and will remain identical in all future 5.x version.
this means that development done on top of the api will seamlessly
support eZ Publish version upgrades.

### PHP API

The PHP API, also called Public
API &lt;/development\_and\_administration\_guides/features/ez\_publish\_public\_api/public\_api\_basics&gt;,
is the development glue and will a create shield between internal and
external developments on eZ Publish. This will cater for an easier
maintainability of code and speed up the performance of eZ Publish. PHP
developers will experience a better extensibility which in turn  will
enable them to create extensions to eZ Publish faster and easier.

| The
Public API &lt;/development\_and\_administration\_guides/features/ez\_publish\_public\_api/public\_api\_basics&gt;
is key to development speed, shorter projects and better quality.
Important to be noted: the php api is the foundation for the rest api
and the second is naturally relying on the first.

### Compatibility with the legacy architecture

| When we introduce changes of this magnitude, eZ Systems as an
international software house must also consider the reality of the
installed customer base. Every installation must be able to take care of
the old and create on the new architecture. | The reason for change is
of course to be able to meet new requirements and the need to enable
progressive changes. 

Understanding the architecture in more detail
---------------------------------------------

### The target architecture: "eZ Platform"

The first important thing to understand about the new architecture is to
explain it standalone, without considering the old legacy architecture.

The following diagram shows a simplistic view of this new architecture,
and a more detailed view for developers.

  

Simplified view

Detailed view

![image0](https://lh5.googleusercontent.com/HxSTBlMNSYkYyQxVCgj_zhHlEcl_YbKv14x8B65gZ1D8ROJilIQqk5zfblgJr4-EQOD5RCB5T_tMF1V7aNCQ0wOlTgugHLl8e1gSVEURJMvhqGecjvzx)

![image1](attachments/11403666/26902595.png)

The new architecture is layered and uses clearly defined API’s between
the layers.

-   The **business logic** is defined in a new kernel. This business
    logic is exposed to applications via an API (the Public
    API &lt;/development\_and\_administration\_guides/features/ez\_publish\_public\_api/public\_api\_basics&gt;).
    Developers rely on this to develop websites and web applications
    using Symfony to organize the way they develop the user
    interface layer.
-   User interfaces are developed using the Twig template engine but
    directly querying the Public
    API &lt;/development\_and\_administration\_guides/features/ez\_publish\_public\_api/public\_api\_basics&gt;.
-   Integration of eZ Publish in other applications are done using the
    Rest API &lt;/development\_and\_administration\_guides/features/ez\_publish\_rest\_api&gt;,
    which itself relies also on
    the Public API &lt;/development\_and\_administration\_guides/features/ez\_publish\_public\_api/public\_api\_basics&gt;.
-   finally development of extensions of eZ Publish is done using the
    Symfony framework when it comes to the structure of the code, and
    once again relying on the Public API when it comes to accessing
    content management functions

To a lower level, the new architecture also totally redefined the way
the system store data. while this is not finalized in version 5.0 (where
the new storage system is only shipped with MySQL support), the
architecture, when finalized will rely on a storage API that will be
used to develop drivers to any kind of storage subsystem.

A motto for this new architecture is to **heavily use APIs** that will
be maintained on the long term to **ease upgrades and provide lossless
couplings** between each part of the architecture, improving the
migration capabilities of the system at the same time.

 

### The "real" version 5.x architecture: "Legacy" and "Platform" together

The chapter above is only explaining the new architecture but, as
mentioned, version 5 also offers a way to run the *legacy* eZ Publish
stack, in order to simplify upgrade and switch to version 5. This result
in the end in a more sophisticated architecture that is illustrated in
the diagram below.

 ![image2](https://lh4.googleusercontent.com/-HnGbujQWt5MWUdOYkKK_6nBj3sc-vaFEuSzRPaEto-Hi_LRqKQZcf014eJ2e0be8P4yVwfAq8D4y0A4p2PahKy9f6Kd3gDMv0A0X1lwNhV8HDu-0IJ2) \
 

The main difference is, **the cohabitation** between the new
architecture explained in the previous chapter (on the right) and the
previous architecture (on the left).

| If we look at the old architecture, we can see that it is more
monolithic: no defined public PHP API, a business logic implemented in
the kernel but very dependent of the storage system and the underlying
data model, an existing rest api but limited to read access to the
content repository. |  

**This whole legacy architecture is in its whole included with version
5, and can be used as is.**

This means that, for people having developed 4.x websites and that are
reluctant to invest time in migrating or even learning the new
architecture components, they can use version 5 exactly as they were
using version 4. Even the controller (access to the application through
the web server) can totally bypass the new architecture (in that case
the Symfony framework controller) and directly call the legacy eZ
Publish controller and the legacy template engine.

 

On its side, the new architecture has been implemented, and eZ will
implement new features and applications on top of it subsequently. So,
as part of 5.0, the new architecture is in place, but does not provide
yet the full application scope.

 

What is more interesting to understand is how these two integrate:

 

1.  First on the presentation side, the new eZ Publish 5 controller
    makes it **possible to serve pages and functions that are either
    resulting from the new template engine or the legacy template
    engine**. This is a first level of dual compatibility that will help
    developers in a smooth transition from one architecture to the
    other, starting with legacy templates and progressively replacing
    them with templates for the new system, Twig.  
2.  Second, on the api side, the Public
    API &lt;/development\_and\_administration\_guides/features/ez\_publish\_public\_api/public\_api\_basics&gt;
    has been designed to work against the business logic and **to be
    used either on top of the legacy storage or on top of the new
    storage system**. This means that, by implementing the new
    architecture and embracing the PHP Public API, developers enable
    **an easy transition from the old data model to the new one.**  An
    extension developed on top of
    the Public API &lt;/development\_and\_administration\_guides/features/ez\_publish\_public\_api/public\_api\_basics&gt;
    will equally work on an old content repository or on a brand new one
    based on the new architecture.

 

These two ways to implement a compatibility between the past
architecture and the new one offers a wide range of possibilities and a
smooth transition path.

Summary on the ways to use eZ Publish 5
---------------------------------------

### Using eZ Publish 5 in full legacy mode

This way is the less disruptive. In this way, eZ Publish 5.0 totally
behave as if it was an eZ Publish 4.7, or we should say 4.8. This is
ideal for users who have large existing applications with large amount
of data and who are not willing to invest in learning and migrating them
immediately.

 

In this way, even the siteaccess and vhost configuration bypass the
legacy stack, and developers will see almost no differences.

 ![image3](https://lh6.googleusercontent.com/Ad_iBlE0hJCQMJT8WGO5-6Hvx1OxMt5xJCMNZVgJ5kL_4_Gp-TCi5_Zd5DMz0LZXv8AY3hrPvmtQtY2Qh_lheRF9Oa76iwSMRs6dMyWhotL4hvIPCB5R) 

 

### Using ez publish 5.0 through the legacy stack but relying on the new controller and new template system as well as the new kernel.

 

This way offers a transition and allows to combine old template and new
templates in the same application. In this case, the users will rely on
the administration interface of eZ Publish as well as on the ez tool bar
for front-end editing, through the legacy templates, but the front end
will be either based on legacy or new twig based templates.

 

In this model, the two kernels can be used and the system can this way
benefit from the Public API and the new REST API built on top.

 ![image4](https://lh5.googleusercontent.com/Lk9x17V7USgSzT537l_PBhrPFWQzQXev9LkK5PwBDXSGHD2H5CGAAobyk5kCTJ_QOgZj0LptwkiNA31kykr9QcY9fFFFakkHBXbNM1NgRsMduk-XrpJA) 

### Using the brand new "Platform"architecture only

This case is the one that will deliver very strong improvements in
scalability and performance, in this case the whole new architecture is
used and there is no way to reuse components from the legacy
architecture.

 

This means that:

-   the administration interface is not available in 5.0
-   existing templates and site won't run without having been migrated
-   the old storage system is not used any more

 \*Note: because of these restrictions, a new storage engine has not
been made yet. Similar setup can use the "Legacy storage engine" for the
time being to be ready to migrate data and not have to change code when
a new engine is added in the future.\*

While this might sound restricting for the time being, it is clearly the
foundation of the future of eZ Publish.

In the context of eZ Publish 5, it can be useful for new projects
relying only on the concept of "content as a service" the platform is a
high performance and scalability content repository with very advanced
services but provide no editorial user interface. for traditional
content management.

 ![image5](https://lh4.googleusercontent.com/f-3X7xtnw6j-powG51msjXYT1HrJy4fah-bEK0lsmgxGrMiBixIoX9sQw78tjyNKD7xNqwauoDJJj4BJjl1XJoCcAvNKEp-TceIw5yKyJv7WlBmUi03g)

Video : Overview of the eZ Publish 5 architecture

Icon

 Learn more with this video :

Attachments:
------------

| ![image6](images/icons/bullet_blue.gif)
[ez5-architecture-platform.png](attachments/11403666/26902596.png)
(image/png) | ![image7](images/icons/bullet_blue.gif)
[ez5-architecture-platform.png](attachments/11403666/26902595.png)
(image/png)
