Installing Symfony - Requirements and Checks
============================================

Icon

Pre-requisite: You must have downloaded Symfony to your computer.

After downloading Symfony, run the following script:

``` {.sourceCode .theme:}
php app/check/check.php
```

| Take the actions needed to correct any problem that occurs, and then
click on the "**Re-check Configuration**" link to start the checking
again. | This script will take you to the basic configurations of your
project. You can also do the same by editing the
'*app/config/parameters.ini*' file.

Please refer to [the official Symfony
documentation](http://symfony.com/doc/master/reference/requirements.html)
to requirements  so you can successfully install and use it with in your
eZ Publish 5 projects.

 

Icon

Note that the PHP CLI can use a different php.ini file than the one used
with your web server. In this case please, also lunch this utility from
your web server.

When you run the 'php.check' script, it will show you the path of the
php.ini file used by PHP, for instance: "*php used by PHP:
/opt/local/etc/php5/php.ini*"

 

 

Comments:
---------

+--------------------------------------------------------------------------+
| Ahh that's nice ![(smile)](images/icons/emoticons/smile.png) but i have  |
| a question.. why not keeping check.php from Symfony2 stack ? could be    |
| easier to maintain no ?                                                  |
|                                                                          |
| ![image4](images/icons/contenttypes/comment_16.png) Posted by            |
| <philippe.vincent-royol@ez.no> at Dec 19, 2012 09:09                     |
+--------------------------------------------------------------------------+
| Is there a page on symfony.com that explains this? If so then            |
| we shouldn't duplicate it as this kind of info has a tendency to change  |
| more often then you like to keep track of.                               |
|                                                                          |
| ![image5](images/icons/contenttypes/comment_16.png) Posted by            |
| <andre.romcke@ez.no> at Mar 07, 2013 19:52                               |
+--------------------------------------------------------------------------+
| On Symfony.com there is only this                                        |
| page: http://symfony.com/doc/2.1/reference/requirements.html &lt;http:// |
| symfony.com/doc/2.1/reference/requirements.html&gt;\_\_                  |
|                                                                          |
| ![image6](images/icons/contenttypes/comment_16.png) Posted by            |
| <philippe.vincent-royol@ez.no> at Mar 07, 2013 20:11                     |
+--------------------------------------------------------------------------+


