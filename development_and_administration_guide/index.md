Development & Administration Guides
===================================

 

eZ Publish 5.x is a "Dual kernel" software. This means it contains both
a *legacy* *kernel* (effectively a "4.y" version) aka "Legacy Stack",
and a new *5.x kernel*, aka "Symfony Stack" (because it uses
[Symfony2](http://symfony.com/about) Fullstack framework).

Legacy kernel
-------------

For documentation on the *legacy kernel*, please go to
[doc.ez.no](http://doc.ez.no) where you'll find documentation on
everything from its GUIs, official extensions to template coding ( 5.x
doc: [User manual](http://doc.ez.no/eZ-Publish/User-manual/5.x)
& [Technical manual](http://doc.ez.no/eZ-Publish/Technical-manual/5.x)
). If you are looking for documentation on how to develop extensions for
legacy in php, then also see relevant material on
[share.ez.no](http://share.ez.no) (
[Tutorials](http://share.ez.no/learn/ez-publish), [Blogs](http://share.ez.no/blogs) ).

5.x kernel
----------

The new 5.x kernel is built using an HMVC model. It currently consists
of Public API (Model), Symfony2 Fullstack Framework (Hierarchical View
Controller) with Twig (template engine) enhanced with eZ
Publish concepts (Core Bundle). On top of these you'll find REST v2 API,
Demo Bundle and a Legacy Bundle allowing some integration between the
two kernels.

For documentation on Symfony2 go to
[symfony.com](http://symfony.com/doc/current/index.html) (for Twig see
[here](http://symfony.com/doc/current/book/templating.html)) and for the
eZ Publish parts of 5.x kernel continue reading in this section.
