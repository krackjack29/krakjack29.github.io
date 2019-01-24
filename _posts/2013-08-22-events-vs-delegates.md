title: Events Vs Delegates
link: https://pratapgowda.wordpress.com/2013/08/22/events-vs-delegates/
author: pratapgowda
description: 
post_id: 109
created: 2013/08/22 23:11:00
created_gmt: 2013/08/22 17:41:00
comment_status: open
post_name: events-vs-delegates
status: publish
post_type: post

# Events Vs Delegates

<p>You probably would have heard each interviewer asking this question to you in each and every interview you would have attended. I have asked it many million times myself and listened to a different answer each time. </p>  <p>Even i didn’t know the absolute answer, so decided to dig deep into the topic and below are my findings</p>  <p>Delegates and Events are almost the same, both are function pointers which can be used as members in the classes, entities who have the handle to these members can subscribe and unsubscribe using += and –= operators. Both are by default multi cast. </p>  <p>Following is the code piece for using a delegate and event. </p>  <div id="scid:9D7513F9-C04C-4721-824A-2B34F0212519:141acbbc-890c-42db-8dbe-7944157e8a69" class="wlWriterEditableSmartContent" style="float:none;margin:0;display:inline;padding:0;"><pre style="width:616px;height:329px;background-color:White;overflow:auto;"><div><span style="color:#008080;"> 1</span> <span style="color:#0000FF;">namespace</span><span style="color:#000000;"> Events_And_Delegates