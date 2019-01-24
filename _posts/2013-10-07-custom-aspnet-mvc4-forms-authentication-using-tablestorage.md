title: Custom ASP.Net MVC4 Forms Authentication using TableStorage
link: https://pratapgowda.wordpress.com/2013/10/07/custom-aspnet-mvc4-forms-authentication-using-tablestorage/
author: pratapgowda
description: 
post_id: 127
created: 2013/10/07 18:16:00
created_gmt: 2013/10/07 12:46:00
comment_status: open
post_name: custom-aspnet-mvc4-forms-authentication-using-tablestorage
status: publish
post_type: post

# Custom ASP.Net MVC4 Forms Authentication using TableStorage

<p>As part of MVC4 Microsoft released new authentication provider(<a href="http://msdn.microsoft.com/en-us/library/webmatrix.webdata.extendedmembershipprovider(v=vs.111).aspx" target="_blank">WebMatrix.WebData.ExtendedMembershipProvider</a>) support open authentication and new entity framework. </p>  <p>If we have to write custom authentication provider we would have to derive from this new abstract class which has around 20-30 methods to be implemented. I am going to list down few important ones and when it is used. </p>  <ul>   <li>public virtual void <strong>Initialize </strong>(string name, NameValueCollection config)<strong>&#160;</strong> :&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; this is the entry point for your provider, the config parameter, which contains the name of provider, connection string, password attempts and so on, is filled in by the framework from your webconfig.       <div id="scid:9D7513F9-C04C-4721-824A-2B34F0212519:000a8ed9-5f98-4a76-91a9-f2094b587bcb" class="wlWriterEditableSmartContent" style="float:none;margin:0;display:inline;padding:0;"><pre style="width:515px;height:231px;background-color:White;overflow:auto;"><div><span style="color:#0000FF;">&lt;</span><span style="color:#800000;">membership </span><span style="color:#FF0000;">defaultProvider</span><span style="color:#0000FF;">=&quot;TableStorageMembershipProvider&quot;</span><span style="color:#0000FF;">&gt;</span><span style="color:#000000;">

## Comments

**[Alpee](#37 "2013-10-10 12:18:40"):** Good one Pratap..

