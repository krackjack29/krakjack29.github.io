title: Windows Azure Table Storage
link: https://pratapgowda.wordpress.com/2013/05/09/windows-azure-table-storageoverview/
author: pratapgowda
description: 
post_id: 88
created: 2013/05/09 19:32:00
created_gmt: 2013/05/09 14:02:00
comment_status: open
post_name: windows-azure-table-storageoverview
status: publish
post_type: post

# Windows Azure Table Storage

<p>&#160;</p>  <p>Windows Azure Storage account provides access to Blob, Queue and Table Storage services. By default we get up to 100 TB storage (all 3 types of storage including). Read more about it <a href="http://www.windowsazure.com/en-us/manage/services/storage/what-is-a-storage-account/">here</a>. </p>  <p>Table storage is a No-Sql storage which stores data (entities, .Net objects) as rows in a table. The entities are indexed based on the partition key and row key. Combination of these two properties would form a unique identifier for the entity. </p>  <p>The image below gives a conceptual storage model of the windows azure storage account and table storage.</p>  <p><a href="http://pratapgowda.files.wordpress.com/2013/05/storageaccount.png"><img title="StorageAccount" style="background-image:none;padding-top:0;padding-left:0;margin:0 5px;display:inline;padding-right:0;border-width:0;" border="0" alt="StorageAccount" src="http://pratapgowda.files.wordpress.com/2013/05/storageaccount_thumb.png" width="426" height="232" /></a></p>  <p><strong>Advantages of Table Storage</strong></p>  <ul>   <li>Its <strong>cheap</strong>, 100 GB storage account costs around $10 per month , while SQL Database on cloud for a storage size of 10GB costs $45 per month.&#160; You can check for yourself <a href="http://www.windowsazure.com/en-us/pricing/calculator/?scenario=data-management">here</a>. </li>    <li>Table storage provides <strong>querying</strong> capabilities. One can easily access the entities using Partition keys and Row keys. (Querying using properties of the entity other than PK and RK is also supported, but its a performance hazard). </li>    <li>Its POCO, the data that needs to be stored is Pure Old classes and objects. </li>    <li>The azure storage library also supports OData protocol to access the entities which means RESTful APIs to access data. </li> </ul>  <p><strong>Disadvantages of Table Storage</strong></p>  <ul>   <li>The entities should be less than 1MB of size, so large datasets cannot be stored. </li>    <li>Its No-Sql storage hence you cannot create relationships between two tables. Of course no joins or any SQL kind of stuff </li> </ul>  <p>By the above mentioned advantages and disadvantages one could gauge when to use Table Storage and not. </p>  <p>Next blog is about programming windows azure table storage. </p>