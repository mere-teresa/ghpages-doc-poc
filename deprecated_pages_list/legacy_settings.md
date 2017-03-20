Legacy Settings
===============

Created and last modified by <ricardo.correia@ez.no> on Jul 12, 2013

This section includes information about newly introduced legacy settings
in eZ Publish 5.

For details on the 4.x legacy settings please refer to the related
[section on
doc.ez.no](http://doc.ez.no/eZ-Publish/Technical-manual/4.x/Reference/Configuration-files/).

`menu.ini`
==========

`[Leftmenu_<menu>]`
-------------------

 

### `Enabled_<Links_index>[]`

Icon

This legacy setting has been introduced in eZ Publish 5.2.

| The `Enabled_<Links_index>[]` setting has been **introduced in 5.2**,
and allows you to make links from a menu clickable on not clickable,
which links are defined in the setting arrays `Links[]` (for the URL)
and in `LinkNames[]` (for the link text). | Consider `<Links_index>` to
be the same index defined in `Links[<Links_index>]` and
`LinkNames[<Links_index>]`.

This setting expects the specification of an index, which can represents
the the current
[ui\_context](http://doc.ez.no/eZ-Publish/Technical-manual/4.7/Templates/The-pagelayout/Variables-in-pagelayout),
or default, for the link to be clickable or not clickable in all
*ui\_contexts* (navigation, edit, browse, etc), or in a particular one.
The setting expects a `true` or `false` value, and the default value is
`true`.

**Usage example**

``` {.sourceCode .theme:}
Enabled_<Links_index>[browse]=false
```

 

 

 

#### Comments:

+--------------------------------------------------------------------------+
| This doc should have been on doc.ez.no to keep it among the other legacy |
| settings for consistency.                                                |
|                                                                          |
| ![image2](images/icons/contenttypes/comment_16.png) Posted by            |
| <andre.romcke@ez.no> at Aug 10, 2013 17:01                               |
+--------------------------------------------------------------------------+
| In part I agree with that, but in this case this setting has been        |
| introduced in 5.2, and it doesn't exist in any of the previous versions. |
| This is why I avoided documenting this legacy setting in 4.x.            |
|                                                                          |
| ![image3](images/icons/contenttypes/comment_16.png) Posted by            |
| <ricardo.correia@ez.no> at Aug 19, 2013 10:48                            |
+--------------------------------------------------------------------------+


