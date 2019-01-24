title: Akka.Net - Create actor if not exists
link: https://pratapgowda.wordpress.com/2017/04/25/akka-net-create-actor-if-not-exists/
author: pratapgowda
description: 
post_id: 274
created: 2017/04/25 13:56:08
created_gmt: 2017/04/25 08:26:08
comment_status: open
post_name: akka-net-create-actor-if-not-exists
status: publish
post_type: post

# Akka.Net - Create actor if not exists

<blockquote></blockquote> <p>If you’d have an actorsystem within an web application, you would have faced the problem of creating an actor with a unique id. Since web is by default stateless it becomes imperative for the actors to be identified by the right names and Uri. </p> <p>Akka.net doesn’t allow actors to be created with same name, it would throw up an exception. And it misses CreateIfNotExists method on actor creation. So I ended up writing a helper method to check if an actor is already created. If it is, method returns an IActorRef else creates a new actor with the actor name passed.</p> <div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:73cc0a1c-bb3a-48ca-9824-cc6b047b9340" class="wlWriterEditableSmartContent" style="float:none;margin:0;display:inline;padding:0;"><pre style="white-space:normal;">