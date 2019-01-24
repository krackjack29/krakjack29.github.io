title: Partitioning and retrieving all the actors in service fabric
link: https://pratapgowda.wordpress.com/2016/07/09/partitioning-and-retrieving-all-the-actors-in-service-fabric/
author: pratapgowda
description: 
post_id: 236
created: 2016/07/09 11:22:00
created_gmt: 2016/07/09 05:52:00
comment_status: open
post_name: partitioning-and-retrieving-all-the-actors-in-service-fabric
status: publish
post_type: post

# Partitioning and retrieving all the actors in service fabric

<p>A Reliable actor service is partitioned across the service fabric cluster. Partitioning in the context of service fabric is process of determining the particular service partition is responsible for complete state of service.</p> <p>By default service fabric uses “ranged partitioning” for the stateful/actor service. In ranged partitioning the actors are divided into n number of buckets (n being the partition count of the service) with each bucket capable of holding x number of keys/partition based on low key and high key.</p> <p>e.g. Lets assume you have an actor service with partition count of 5, high key = 100 &amp; low key =&nbsp; –100. (you can find the default values in the ApplicationManifest.xml), the keys or actorIds would fall in below bucket range.</p> <p><a href="http://pratapgowda.files.wordpress.com/2016/07/image.png"><img title="image" style="background-image:none;padding-top:0;padding-left:0;display:inline;padding-right:0;border-width:0;" border="0" alt="image" src="http://pratapgowda.files.wordpress.com/2016/07/image_thumb.png" width="657" height="205"></a></p> <p>When an Actor with ActorId 15 is created Partition 3 maintains the state for that instance. Now there are scenarios when you would want to retrieve all the actors of a type in Service Fabric cluster, below is the code to achieve the same.</p> <div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:5d3006fc-1fe9-4fe0-9ffc-0b05622db09a" class="wlWriterEditableSmartContent" style="float:none;margin:0;display:inline;padding:0;"><pre style="white-space:normal;">