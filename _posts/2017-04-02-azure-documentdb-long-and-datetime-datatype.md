title: Azure DocumentDb: long and DateTime datatype
link: https://pratapgowda.wordpress.com/2017/04/02/azure-documentdb-long-and-datetime-datatype/
author: pratapgowda
description: 
post_id: 270
created: 2017/04/02 12:29:00
created_gmt: 2017/04/02 06:59:00
comment_status: open
post_name: azure-documentdb-long-and-datetime-datatype
status: publish
post_type: post

# Azure DocumentDb: long and DateTime datatype

<p>One good thing i liked about Service Fabric is the actor systems, which led me a Java port of Akka/Scala called Akka.Net. You must have a look at it, actors are new paradigm for programming, one can create a scalable concurrent distributed system within minutes. This blog isn't about Akka.Net , they have got great documentation about it <a href="http://getakka.net/docs/">here</a>.  <p>Akka.Net has a persistence layer, to store actor states, which works with most data sources and also comes with extensible framework so that you can create your own. They didn't have any DocumentDb libraries, since i was using DocumentDb as data source in our project I decided to write one. You can find the source code at <a href="https://github.com/pratapbhaskar/Akka.Persistence.DocumentDb">https://github.com/pratapbhaskar/Akka.Persistence.DocumentDb</a>.  <p>While creating this project I came across a unique problem in DocumentDb. To maintain order in the snapshot store of Akka actors we need to persist the time of snapshot creation with precision, If stored as Datetime wouldn’t have the capability of queries other than equal to. More information <a href="https://azure.microsoft.com/en-in/blog/working-with-dates-in-azure-documentdb-4/" target="_blank">here</a>. <p>Hence <strong>DateTime</strong> is stored as <strong>Ticks</strong> (what is a Tick?? explained <a href="http://stackoverflow.com/questions/9644608/datetime-ticks-in-net-its-usage-and-why-it-is-used">here</a>).&nbsp; Ticks would normally generate a long number e.g. 636267296223504225. DocumentDb works with Json documents and JavaScript can handle up to 2^53 only. Hence we serialize Ticks (long) it loses precision.&nbsp;&nbsp; <p> <div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:686c136d-27e5-48c9-a387-56388b25deec" class="wlWriterEditableSmartContent" style="float:none;margin:0;display:inline;padding:0;"><pre style="white-space:normal;">