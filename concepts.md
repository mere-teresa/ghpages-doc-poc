Concepts
========

WORK IN PROGRESS

Reference for eZ Publish Platforms domain-specific language (DSL),
meaning this page is describing concepts further used in specification
(BDD), user stories and the rest of the documentation.

Overall
-------

 

Concept:

Description:

Replaces "Legacy" concept:

Legacy

Refers to eZ Publish part of eZ Publish Platform which is based on eZ
Publish 4.x, sub topics: "Legacy Stack" and "Legacy StorageEngine"

 

Platform

Refers to part introduced in 5.0 together with Legacy to together form
eZ Publish Platform, will become eZ Platform, aka "6.x".

 

 

System
------

 

Refers to concepts referring to specific ways to setup the system as a
whole. 

 

Concept:

Description:

Replaces "Legacy" concept:

Clustering

Clustering &lt;/installation\_and\_upgrade\_guides/installation/clustering&gt;
refers to how you can setup eZ Publish Platform to scale up to several
servers (typically scaling up web servers).

 

Repository
----------

Refers to the Content Repository, where you place your content and all
it's related domains, like Content Type.

 

Concept:

Description:

Replaces "Legacy" concept:

Content (Object)

 

(Content) Object

ContentType (Object)

 

(Content) Class

Location (Object)

 

Node

User (Object)

 

 

Web
---

Refers to web development, specifically concepts provided by and
extending the Symfony development framework.

Concept:

Description:

Replaces "Legacy" concept:

Contextual Override

Refers to rules based overriding of Content / Location
templates &lt;/development\_and\_administration\_guides/features/mvc\_and\_application/configuration/view\_provider\_configuration&gt;,
and advance [rule-based overrides of Content/Location
controllers](https://doc.ez.no/display/EZP6/How+to+use+a+custom+controller+to+display+a+content+or+location).

Override\[.ini\]

Override

Refers to Symfony [Bundle
override](http://symfony.com/doc/current/cookbook/bundles/override.html),
and [Bundle
inheritance](http://symfony.com/doc/current/cookbook/bundles/inheritance.html).

Placement/Design override

Sessions

Sessions &lt;/development\_and\_administration\_guides/features/mvc\_and\_application/session&gt;
are standard web based sessions keeping track of for instance who is
logged in.

 

Anonymous Session

A `Session` representing a `A``nonymous User,` typically started on user
actions, which do not require being logged in, like adding products to a
basket.

 

Anonymous User

A `User` in the Repository configured to represent a *non* logged-in
`User`.

 

 
