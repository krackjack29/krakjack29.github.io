title: Random Number Generator
link: https://pratapgowda.wordpress.com/2014/01/14/random-number-generator/
author: pratapgowda
description: 
post_id: 163
created: 2014/01/14 15:29:00
created_gmt: 2014/01/14 09:59:00
comment_status: open
post_name: random-number-generator
status: publish
post_type: post

# Random Number Generator

<p>.Net has a very useful class called “Random” which generates a random number between a range. You can read about the Random class <a href="http://msdn.microsoft.com/en-us/library/system.random(v=vs.110).aspx" target="_blank">here</a>. </p> <p>But one problem with the random class in C# is that it would give up duplicates every now and then. There are scenarios when you would want a set of numbers within a range to be shuffled or randomized. </p> <p>i.e if the range is between 0 – 5 you would want {5,0,2,1,4,3} or some set that would be random and within the limits and end up not giving duplicates.</p> <p>following is the code which generates the random numbers and can be iterated over (as it implements IEnumerable).</p> <div id="scid:57F11A72-B0E5-49c7-9094-E3A15BD5B5E6:ee3c0e05-f4db-49b7-91ba-a0f4c206e28e" class="wlWriterEditableSmartContent" style="float:none;margin:0;display:inline;padding:0;"><pre style="background-color:#FFFFFF;white-space:-moz-pre-wrap;word-wrap:break-word;overflow:auto;"><span style="color:#0000FF;">using</span><span style="color:#000000;"> System;