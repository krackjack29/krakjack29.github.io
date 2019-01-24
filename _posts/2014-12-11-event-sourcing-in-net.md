title: Event Sourcing in .Net
link: https://pratapgowda.wordpress.com/2014/12/11/event-sourcing-in-net/
author: pratapgowda
description: 
post_id: 189
created: 2014/12/11 17:53:00
created_gmt: 2014/12/11 12:23:00
comment_status: open
post_name: event-sourcing-in-net
status: publish
post_type: post

# Event Sourcing in .Net

<blockquote> <p><font size="3">The fundamental idea of Event Sourcing is that of ensuring every change to the state of an application is captured in an event object, and that these event objects are themselves stored in the sequence they were applied for the same lifetime as the application state itself.</font></p> <p><font size="3">- Martin Fowler</font></p></blockquote> <p>Basically Event Sourcing says anything that happens in the system domain to be captured via events (POCO) and these events are stored (serialized) in a simple data store (document db or even SQL).</p> <p><strong>Why Events?</strong></p> <ul> <li>Events are immutable (already happened in the past), so can be stored in append only data store.  <li>Events are simple POCOs that describe some action as it occurred, together with associated data it can easily take domain context and meaning e.g. OrderItemAdded, ProductAddedToCart, UserSignedUp etc.</li></ul> <p><strong>How to Create an event in .Net?</strong></p> <p>As mentioned above events is a simple .Net class, similar to the one mentioned below. </p> <div id="scid:9D7513F9-C04C-4721-824A-2B34F0212519:3045cbc2-84d4-470f-a646-e65741f45a2a" class="wlWriterEditableSmartContent" style="float:none;margin:0;display:inline;padding:0;"><pre style="width:561px;height:289px;background-color:White;overflow:visible;"><div><span style="color:#000000;">    </span><span style="color:#0000FF;">public</span><span style="color:#000000;"> </span><span style="color:#0000FF;">abstract</span><span style="color:#000000;"> </span><span style="color:#0000FF;">class</span><span style="color:#000000;"> DomainEvent