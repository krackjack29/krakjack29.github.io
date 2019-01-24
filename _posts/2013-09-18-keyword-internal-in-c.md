title: Keyword 
link: https://pratapgowda.wordpress.com/2013/09/18/keyword-internal-in-c/
author: pratapgowda
description: 
post_id: 115
created: 2013/09/18 12:38:00
created_gmt: 2013/09/18 07:08:00
comment_status: open
post_name: keyword-internal-in-c
status: publish
post_type: post

# Keyword 

<p>&quot;Internal&quot; in C# is used to make the members /methods /classes to be accessible only within that assembly. </p>  <p>Past few days i had been working on a framework to handle multiple NoSQL databases (namely Azure Table Storage, and Simple DB from Amazon) . The problem was any other assembly referring to the framework would also have to refer to the Amazon dlls and Azure dlls, even though i had factories to create them. </p>  <p>Mistake was I had all my classes as public both abstract as well as concrete implementations (yup i am douchebag!!!).&#160; </p>  <p>Then came the &quot;Internal&quot; keyword to the rescue. I put all my concrete implementations as internal classes and methods which were accessing the core Azure apis, while exposed only the interfaces (pure .net) and voila , now when i refer this framework to other assemblies they don't have to get any other assemblies at all. </p>  <p>You can still have friend assemblies for your unit testing :-) read more @ <a href="http://msdn.microsoft.com/en-us/library/0tke9fxk.aspx">http://msdn.microsoft.com/en-us/library/0tke9fxk.aspx</a></p>  <p>PS: You definitely need them at run time, but this makes my life so easy. </p>  <p>NOTE: If you think that the classes aren't really used outside the assembly please leave it unmarked as public as compiler by default makes that assembly as internal.</p>

## Comments

**[Theodore](#33 "2013-09-18 15:21:12"):** The [C# Menu Controls](http://www.kettic.com/winforms_ui/menu.shtml) for Windows Forms offers much more amazing navigation menus than the classic Office style menus

