title: Adapter Pattern
link: https://pratapgowda.wordpress.com/2013/09/30/adapter-pattern/
author: pratapgowda
description: 
post_id: 119
created: 2013/09/30 14:36:00
created_gmt: 2013/09/30 09:06:00
comment_status: open
post_name: adapter-pattern
status: publish
post_type: post

# Adapter Pattern

<p>As the name itself suggests “Adapter” pattern allows an interface to ‘adapt’ to another interface. Similar to the universal adapter we all carry when we go outside India helps us to adapt to 110V signal (input interface) to a 220V signal (output interface).</p>  <p><a href="http://pratapgowda.files.wordpress.com/2013/09/adapter.png"><img title="adapter" style="background-image:none;padding-top:0;padding-left:0;display:inline;padding-right:0;border-width:0;" border="0" alt="adapter" src="http://pratapgowda.files.wordpress.com/2013/09/adapter_thumb.png" width="120" height="125" /></a></p>  <p>You can read about the basics of Adapter pattern <a href="http://dofactory.com/Patterns/PatternAdapter.aspx#_self2" target="_blank">here</a> , and following is the walkthrough of the usage in one of the real world scenario.</p>  <p><strong>Problem</strong>:&#160; Azure table storage requires the entities to be stored within <a href="http://pratapgowda.wordpress.com/2013/07/15/tableentity-and-tablestorage/" target="_blank">TableStorage</a> to implement ITableEntity interface or TableEntity class, this forms a tight coupling between Azure SDK and the entities that we develop to be stored. What if we decided to move to AWS tomorrow ?</p>  <p><strong>Solution</strong>: We created our own interface (an abstract class in this example) called it EntityModel </p>  <div id="scid:9D7513F9-C04C-4721-824A-2B34F0212519:0ee12a36-41ea-4d20-a2e8-e622a2c70453" class="wlWriterEditableSmartContent" style="float:none;margin:0;display:inline;padding:0;"><pre style="width:605px;height:246px;background-color:White;overflow:auto;"><div><span style="color:#008080;"> 1</span> <span style="color:#000000;">    </span><span style="color:#0000FF;">public</span><span style="color:#000000;"> </span><span style="color:#0000FF;">abstract</span><span style="color:#000000;"> </span><span style="color:#0000FF;">class</span><span style="color:#000000;"> EntityModel 