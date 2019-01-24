title: Programming Azure TableStorage : Repository Pattern
link: https://pratapgowda.wordpress.com/2013/08/04/programming-azure-tablestorage-repository-patternpart-1/
author: pratapgowda
description: 
post_id: 94
created: 2013/08/04 00:54:00
created_gmt: 2013/08/03 19:24:00
comment_status: open
post_name: programming-azure-tablestorage-repository-patternpart-1
status: publish
post_type: post

# Programming Azure TableStorage : Repository Pattern

<h4>Repository Pattern</h4>  <blockquote>   <p><b>by Edward Hieatt and Rob Mee</b></p>    <p><i>Mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects.</i></p> </blockquote>  <p>Repository pattern according to me simply put is an <strong>in-memory database </strong>which abstracts away the actually operations that one needs to do with the physical database (CRUD operations). One could implement their own caching mechanisms in the repository, as well as do transactional operations. </p>  <p>You can learn more about the Repository Pattern here <a href="http://msdn.microsoft.com/en-us/library/ff649690.aspx">http://msdn.microsoft.com/en-us/library/ff649690.aspx</a> and <a href="http://martinfowler.com/eaaCatalog/repository.html">http://martinfowler.com/eaaCatalog/repository.html</a>.</p>  <p>In my previous <a href="http://pratapgowda.wordpress.com/2013/07/15/tableentity-and-tablestorage/" target="_blank">blog</a> i mentioned about the class TableEntity and how one should create their own version of BaseTableEntity . In this one i would show how to use Repository pattern to perform Create, Update and Delete operations, i would keep the reading and Query object pattern to the next one.</p>  <h4>ITableStore&lt;TTable&gt; </h4>  <p>One of the most important and efficient things that .Net provides is Generics, in this situation we could use one TableStore generic class to cater to any entity that client requires.&#160; First we declare a contract for the table store.    <div id="scid:9D7513F9-C04C-4721-824A-2B34F0212519:71124846-5185-43b1-bc42-8695c653e940" class="wlWriterEditableSmartContent" style="float:none;margin:0;display:inline;padding:0;"><pre style="width:576px;height:164px;background-color:White;overflow:visible;"><div><span style="color:#008080;">1</span> <span style="color:#0000FF;">public</span><span style="color:#000000;"> </span><span style="color:#0000FF;">interface</span><span style="color:#000000;"> ITableStore</span><span style="color:#000000;">&lt;</span><span style="color:#000000;">TTable</span><span style="color:#000000;">&gt;</span><span style="color:#000000;"> : ITableReader</span><span style="color:#000000;">&lt;</span><span style="color:#000000;">TTable</span><span style="color:#000000;">&gt;</span><span style="color:#000000;">