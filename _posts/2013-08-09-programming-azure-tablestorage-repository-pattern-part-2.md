title: Programming Azure TableStorage : Repository Pattern Part -2
link: https://pratapgowda.wordpress.com/2013/08/09/programming-azure-tablestorage-repository-pattern-part-2/
author: pratapgowda
description: 
post_id: 98
created: 2013/08/09 05:21:00
created_gmt: 2013/08/08 23:51:00
comment_status: open
post_name: programming-azure-tablestorage-repository-pattern-part-2
status: publish
post_type: post

# Programming Azure TableStorage : Repository Pattern Part -2

<p>In my previous <a href="http://pratapgowda.wordpress.com/2013/08/04/programming-azure-tablestorage-repository-patternpart-1/" target="_blank">blog</a>, i mentioned the basics and the usage of the repository pattern with the TableStorage (only CRUD operations without the ‘R’ reading and querying capabilities).&#160; I did that because we can leverage the <a href="http://www.martinfowler.com/eaaCatalog/queryObject.html" target="_blank">Query Object</a> pattern to do the reading/querying the storage.</p>  <p>Query object pattern – simply put its representing all the queries as objects</p>  <p><strong>Querying the TableStorage – </strong>TableStorage indexes the table data based on PartitionKey and RowKey, but one could still query based on other properties in entity.However querying non key properties would cause <em>full table scan</em>, which will definitely degrade the performance.&#160; So while designing the table one need to extra careful about the usage of partition key and row key.&#160; More info - <a href="http://msdn.microsoft.com/en-us/library/windowsazure/hh508997.aspx">http://msdn.microsoft.com/en-us/library/windowsazure/hh508997.aspx</a>.</p>  <p><strong>ITableReader&lt;T&gt;&#160; - </strong>ITableReader is an interface designed to execute the queries on the table of type T. </p>  <div id="scid:9D7513F9-C04C-4721-824A-2B34F0212519:8083ba83-7343-4336-9a8d-15d5c9933f81" class="wlWriterEditableSmartContent" style="float:none;margin:0;display:inline;padding:0;"><pre style="width:550px;height:465px;background-color:White;overflow:auto;"><div><span style="color:#008080;"> 1</span> <span style="color:#000000;">     </span><span style="color:#0000FF;">public</span><span style="color:#000000;"> </span><span style="color:#0000FF;">interface</span><span style="color:#000000;"> ITableReader</span><span style="color:#000000;">&lt;</span><span style="color:#000000;">TTable</span><span style="color:#000000;">&gt;</span><span style="color:#000000;">

## Comments

**[Guy](#149 "2014-06-24 17:29:42"):** Hi, nice article, but I got some issue, may be you can assist, ICloudTableContext used in TableStore constructor doesn't exist as TYpe, may be obsolete, but didn't find any documentation. Another Issue, you used RepositoryHelper.dind't find any implementation for this static class.

**[Pratap Bhaskar](#193 "2014-08-19 15:39:30"):** Repository helper just gives the Table names using the attributes which we have created

**[Pratap Bhaskar](#192 "2014-08-19 15:37:36"):** Hi Guy,

