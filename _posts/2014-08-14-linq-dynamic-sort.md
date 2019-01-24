title: LINQ
link: https://pratapgowda.wordpress.com/2014/08/14/linq-dynamic-sort/
author: pratapgowda
description: 
post_id: 184
created: 2014/08/14 22:19:00
created_gmt: 2014/08/14 16:49:00
comment_status: open
post_name: linq-dynamic-sort
status: publish
post_type: post

# LINQ

<p>We faced a scenario where we had a dataset (EF Linq query) which needs to be sorted based on the user selection (user was given the selection option with the table columns display name), we ended up making an extension method which could be used by anyone for dynamically selecting the sorting key. </p> <p>We used Linq expressions and reflection to create parameter and property selector from the column name. Below is the code </p> <div id="scid:9D7513F9-C04C-4721-824A-2B34F0212519:b996776c-34b9-48df-ab6e-eef57c8303ba" style="float:none;margin:0;display:inline;padding:0;"><pre style="overflow:auto;height:306px;width:596px;background-color:white;"><div><span style="color:#0000ff;">public</span><span style="color:#000000;"> </span><span style="color:#0000ff;">static</span><span style="color:#000000;"> IQueryable</span><span style="color:#000000;">&lt;</span><span style="color:#000000;">T</span><span style="color:#000000;">&gt;</span><span style="color:#000000;"> Sort</span><span style="color:#000000;">&lt;</span><span style="color:#000000;">T</span><span style="color:#000000;">&gt;</span><span style="color:#000000;">(</span><span style="color:#0000ff;">this</span><span style="color:#000000;"> IQueryable</span><span style="color:#000000;">&lt;</span><span style="color:#000000;">T</span><span style="color:#000000;">&gt;</span><span style="color:#000000;"> source, </span><span style="color:#0000ff;">string</span><span style="color:#000000;"> propertyName, </span><span style="color:#0000ff;">bool</span><span style="color:#000000;"> isDescending)